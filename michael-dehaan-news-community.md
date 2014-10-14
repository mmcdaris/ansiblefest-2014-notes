#### Windows Management
uses windows powershel remoting modules in PowerShell or can push raw
PowerShell setup script

#### Run_once
This info from: [run_once docs](http://docs.ansible.com/playbooks_delegation.html#run-once)
In some cases there may be a need to only run a task one time and only
on one host. This can be achieved by configuring “run_once” on a task:
```
---
# ...

  tasks:

    # ...

    - command: /opt/application/upgrade_db.py
      run_once: true

    # ...
```

This can be optionally paired with “delegate_to” to specify an
individual host to execute on:

```
- command: /opt/application/upgrade_db.py
  run_once: true
  delegate_to: web01.example.org
```

When “run_once” is not used with “delegate_to” it will execute on the
first host, as defined by inventory, in the group(s) of hosts targeted
by the play. e.g. webservers[0] if the play targeted “hosts:
webservers”.

This aproach is similar, although more concise and cleaner than applying
a conditional to a task such as:

```
- command: /opt/application/upgrade_db.py
  when: inventory_hostname == webservers[0]
```

#### Fact Caching
Uses redis or memcached to store playbook variables / facts between
executions
[fact cache docs](see docs: http://docs.ansible.com/playbooks_variables.html#id34)

#### no_log tweaks
Prev surpressed syslog now srpresses CLI/callback output as well
[no_log_docs](http://docs.ansible.com/faq.html#how-do-i-keep-secret-data-in-my-playbook)

#### Galaxy!
people sharing playbooks https://galaxy.ansible.com/
galaxy requirements file

#### Load Balancing
Additional load balancer support(A10 models)

#### Docker Images
Base images for faster ansible playbook builds using DocerHub
Ubuntu and CentOs

#### Refactoring(Ongoing 1.9/..)
Reorganize after years of glorious OSS contributions Hailstorm
focus on hybrid-oo/functional architecture, TDD
enable features like blocks. try/except, future language changes
simplify module and templating internals
endable line number reporting
if there is a yaml error
there will be file line crash reports

#### Additional Plans!
Join the project! Share your problems!

#### Contact info
michael@ansible.com @laserllama
