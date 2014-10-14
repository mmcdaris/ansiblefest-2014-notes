## What are Roles?

### Getting Started

Creating the initial role layout
They have a dir structure

```
ansible-galaxy init roles/myrole
```

### Role Dependencies

```
dependencies:
  - foo
  - { role: foo }
  - { role: /path/to/some/dir/bar }
  - { role: bam, some_var: "hello world!" }
```

### Tags & Conditionals with Role Dependencies

When applying a tag or conditional statement to a role definition, they
are appended to those specified on any tasks within the role. For
Example:

```
# main yaml file
roles:
  - { role: foo, tags: 'foo', when: some_var == "bar" }
```

```
# roles/foo/meta/main.yml
dependencies:
  - { role: bar }
```

All tasks defined within 'bar' will also have the above tags and 'when'
statement applied to them, in addition to any other tags or conditionals
defined on the task.

### Cross-Platform Roles: tips and tricks

Use gathered facts and include statement

```
# apache/tasks/main.yml
- include: redhat.yml
  when: ansible_os_faily == "RedHat"
- include: debian.yml
  when: ansible_os_faily == "Debian"
```

```
# apache/tasks/redhat.yml
- name: install apache packages
  yum: name=httpd state=present
```

```
# apache/tasks/debian.yml
- name: install apache packages
  apt: name=apache2 state=present
```

### Handlers! Let's do the same!

**You Can't!**

###### Why doesn't this work?

1. Handler names in Ansible must be unique.
- Included files are always read and parsed at load time, BUT the
  conditionals are not evaluated until the task is executed...
- Result: the last handler defined with the name "wins", and the wrong
  handler may be run..

### Solution: use set_fact

```
# apache/tasks/redhat.yml
- name: install apache packages
  yum: name=httpd state=present
- name: set the name of the service
  set_fact: apache_service_name=httpd

# apache/tasks/debian.yml
- name: install apache packages
  apt: name=apache2 state=present
- name: set the name of the service
  set_fact: apache_service_name=apache2
```

```
# not complete snippet
# apache/handlers/main.yml
- name: restart apache service
  service: name={{apache_service_name}} state=...
```

### Dealing with other Differences

We can use set_fact again in the platform-specific includes:

```
# apache/tasks/redhat.yml
- name: set variables for this OS
  set_fact:
     apache_service_name: httpd
     apache_user: apache
     apache_httpd_conf_path: /etc/httpd/conf/httpd.conf
     apache_http_confd_path: /etc/httpd/conf.d
```

```
# apache/tasks/debian.yml
- name: set variables for this OS
  set_fact:
     apache_service_name: apache2
     apache_user: www-data
     apache_httpd_conf_path: /etc/apache2/conf/apache2.conf
     apache_http_confd_path: /etc/apache2/conf.d
```

And in the common tasks/main.yml:

```
# possibly not complete snippet
# apache/tasks/main.yml
# (continuted)
- name: deploy apache configuration
  template:
    src: httpd.conf.j2
    dest: "{{apache_httpd_conf_path}}"
    owner: "{{apache_user}}"
    mode: "0640"
  notify: restart apache service

- name: make sure service is running and enabled
  service:
    name: "{{apache_service_name}}"
    state: running
    enabled: yes
```

### Alternative for variables

It is possible to use the include_vars and with_first_found lookup to
include the distro-specific varibales file, and to fall back to a
default set of variables:

```
# apache/tasks/main.yml
- include_vars: "{{item}}"
  with_first_found:
    - "{{ansible_os_family}}.yml"
    - "default.yml"
```

### Galaxy Best Practices

- Creating Accurate Metadata
  - when you use the ansible-galaxy CLI commands to init a new role, a
    sample meta/main.yml is automatically created for you:

```
---
galaxy_info:
  author: your name here
  description:
  company: your company (optional)
  licence: BSD
  min_ansible_version: 1.2
  # platforms:
  #   name: EL
  #   versions:
  #   - all
  #   - 5
  #   - 6
  #   - 7
  # ...
  # categories:
  # - cloud
```

  - listing sane dependencies
    - Dependencies listed in teh metadata should only point to other
      Galaxy roles
  - steps to add to github.com

```
ansible-galaxy init foo
cd foo
git init

# edit your files here

git add .
git commit -m "initial commit for my role"
git remote add origin
# location https://github.com/username/foo
git push origin master
```

### Questions

**Q:**
  any support for templates for creating custom roles
**A:**
  No but it sounds like a fun feature.

**Q:**
  Version Pinning and repeatability are you going to add this?
**A:**
  Galaxy CLI can specify user and role name and version tag/number
  you can also specify a github sha
  requirements file is a yaml file

**Q:**
  What if your roles are tagged?
**A:**
  limited to one repo per role

**Q:**
  How will ansible galaxy handle the problem of many forks
**A:**
  Use galaxy as a starting point and rely on the rating system
  Any roles will have github issues links and roles

**Q:**
  Open source Galaxy?
**A:**
  It may be nice from the company level
