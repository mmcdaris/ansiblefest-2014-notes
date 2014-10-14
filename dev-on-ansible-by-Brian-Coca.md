## Developing On And Extending Ansible
**author:** Brian Coca
Brian Coca is one of the more notable presences on Ansible’s IRC and
mailing list, and has numerous commits in Ansible Core.  Brian will
discuss how to develop on ansible, including writing modules and various
plugins, and give more detail about various internals.  If you’ve wanted
to contribute to Ansible or extend it for any reason, this should be a
great talk.

#### Who is Brian
programmer, DBA,system Admin and more!

#### What is Ansible?
Can it do this?
- Config Management?
- Automation Platform?
- Release Management?
- Versioning?

Ansible boiled down: "just runs a task on a host"

#### The Guts: Ansible Internal Objects
- inventory: defines my targets
- runner: does work on target
- connection: how to get to my targets
- playbook: all plays in current invocation (in memory)
  - play: marries host and task
    - host: where to do it
    - task: "it" aka "what to do"
- callbacks: show me what was done
  - "hey I did it/ I didn't/ I ignored it"

#### Example hack - the core
- 10 sec to patch to add play_hosts
- 3h to find the correct spot to patch
- learned a lot about playbook/host iteration
- did not take "serial:" into account
- runner rewrite in progress

#### The Plugin System
The sum of the parts is less than the whole!
PluginLoader - `lib/ansible/utils/plugins.py`
Read this file! [plugins]

###### Plugins
- Library: host tasks/actions/modules
- Action: master side tasks/actions/modules
- Cache: fact caching
- Callback: play output:
    -modify what you get from ansible
- Connection: host connections
    - ssh
    - accelerate
- Shell: What shell to use to execute tasks
- Lookup: **undervalued** master side info lookup
- Vars: **undervalued** variable imports
- Inventory Plugins: aside from inventory scripts
  - script_stop.py
  - host and groups plugins
  - small files, easy to read
- Filter Jinja2 filters for data modification
  - chainable
  - look exactly like unix pipes!
- Doc Fragment: shared docs for library
  - helps with maintaining docs

finished in 1.8

###### Filters Jinja2'ism
- best way to change data
- chainable pipes
- simple to expand
- can hide complexity from users
Example:
```
[hello, world]|join(' ')
```

Example - filter
```
import random
def rrandomize_list(myList):
  random.shuffle(myList)
```

###### Lookups
- you are using them with `with_ <lookup>`
- they execute on the **master** (the machine you are running ansible on)

[lib/ansible/runner/lookup_plugins/etcd.py](
  https://sourcegraph.com/github.com/ansible/ansible@devel/.tree/lib/ansible/runner/lookup_plugins/etcd.py
) written by Jan-Piet Mens

###### Callback example
[callback_plugins/profile_tasks.py]()

###### Cache module

[lib/ansible/cache/memory.py](https://sourcegraph.com/github.com/ansible/ansible@devel/.tree/lib/ansible/cache/memory.py)
[lib/ansible/cache/redis.py](https://sourcegraph.com/github.com/ansible/ansible@devel/.tree/lib/ansible/cache/redis.py)

###### Tests: test/
- presenter bad at tests
- tests are great! Prevent regressions!
- assert: the test module
- destructivem non-destructive, integration
- perform change; `make tests` - runs unit tests!

###### Before you submit
- update documentation (it is aslso in the repo)
- create tests, assert! (also in the repo)
- prepare a clear usage/example to post in the PR
  - why do you want this in core
  - how is it usedful to people
  - posting an example playbook is essential and will speed up merge
- it's a dialog, be ready to make your 'use case'
  - trying to convince is beneficial to them and to more people
  - mostly people are rational, sometimes not. lol
- patience ... ... ... ...
  - requests merged after: 3 minutes, 6 months, not at all.
- not everything belongs in core
