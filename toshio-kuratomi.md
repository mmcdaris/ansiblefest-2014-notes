## Fun with Containers and Ansible
Worked for red hat on the fedora

### Docker
- Manages Linux Containers
- Deploy Applications in Isolation
- Not as heavey weight as a VM
- Docker Hub ecosystem for distributing general containers

#### Images
- made of layers
- base images basically os instals
- immutable
  - other people can build off your layers easily
- Customize by creating a new layer on top

#### What can I do with it?
- Lightweight cloud
- Tool for encapsulating applications

###### LightWeight Cloud
- Low runtime overhead
- Layered filesystem


###### Encapsulated Applications
- low overhead
- run application on startup

### Ansible-Docker?
Dockerfile without ansible
```
# Incomplete
FROM centos:stable
RUN yum -y upgrade
RUN yum install httpd
COPY
```

```
FROM ansible/centos7-ansible:stable
ADD ansible /srv/example/
WORKDIR /srv/example
RUN ansible-playbook site.yaml -c local
EXPOSE 80
ENTRYPOINT ["/usr/sbin/apachectl", ".DFOREGROUND"]
```

### What makes ansible better?
- Power and Consistency
- Same method to configure images as your hosts
  - why learn docker when you can re-use ansible
- Templates and Conditionals
- Portability
- no separate docker files, You don't need to maintain multiple docker
  files
- Try out new things via Galaxy roles
  - try out elastic search
  - deoploy and check it out

### Example Level 2 - Use a conditional to provision based on target OS
```
- hosts: localhost
  vars:
    apache_group:
      Debian: "root"
      RedHat: "apache"
    apache_ctl:
      Debian: "/usr/sbin/apache2ctl"
      RedHat: "/usr/sbin/apachectl"
```
```
tasks:
  - yum:  pkg=httpd state=present
    when: ansible_os_family == "RedHat"

  - apt:  update_cache=yes name="apache2" state=present
    when: ansible_os_family == "Debian"

  - name: Make an apachectl symlink that is the same between distros
    file:
      state: link
      src: "{{apache_ctl[ansible_os_family]}}"
      dest: /usr/local/bin/apachectl
```

```
- name: Apply httpd configuration from template
  template:
    src: httpd.conf.j2
    dest: /etc/httpd/conf/httpd.conf
    group: "{{apache_group[ansible_os_family] }}"
```

### Images for Building
- Ansible Layered on a Base OS Image
- Building Four Images
- Two Images for Ubuntu-14.04
- Two images for CentOS-7
- An image for Ansible HEAD
- An image for the Ansible realease on pypi

### Contact Info & Resources
- https://registry.hub.docker.com/u/ansbile/centos7-ansible
- https://registry.hub.docker.com/u/ansbile/ubuntu14.04-ansible
- http://ansible.github.com/ansible/ansible-docker-base
- https://registry.hub.docker.com/u/ansbile/
- http://ansible.com
- tkuratomi@ansible.com
