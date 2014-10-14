#### Ansible O'reilly Book will feature a cow!

#### Who is telescope
Participation media - big voting tv shows use a realtime participation
method.
e.g.

```
american idol
the voice
```

Sometimes it's overnight sometimes it's like 5 minutes
They work with all the big media channels: abc fox cbs bbc disney mtv

Ryan seacrest says vote and america does it!

#### Background
Moved from bare metal to AWS
Why choose ansible for automation
How ansible solved our challenges

#### First love
6 racks across 2 buildings
2 sysadmins responsible for hardware and operations
no env for environment to play :(
hardware was restricting ability to grow
long lead time to build/increase totla capacit

#### Maybe we should look for a new love
- Hard to justify building up a datacenter
- Telescope's case required signigicant burst capacity
- business needs change rapidly

#### Lots of other options
- puppet
- chef
- ansible
- bconfig2
- salt
- cfengine
- vagrant cobbler

#### AWS
Strong APIs

####  Can you make a build in one step?
quote from: Joel Spolsky, "**the joel test:** 12 steps to better code"

#### Puppet
Too Needy!

#### Chef
Hard to understand & complicated!

#### Ansible
Launch, Push, Scale(up,down)
Tight AWS integration
ec2-module - instances in 10 minutes
load-balancers
elastic IPs!

## Enter Ian!
Ian Plans for boxes to go down at any time.

#### Playbook
1. Create Instances
- Install things
- checkout code and build
- pull artifacts in maven

###### Internal debate
Compile code at build or pull down a pre-build version

###### Monitoring
**Ansible templates give flexibility with infelxible tools**
e.g. Zabbix for monitoring

###### Tags
Tags have been useful with: ppTier+environment use

#### Dynamic Inventory File
http://docs.ansible.com/intro_dynamic_inventory.html

> Often a user of a configuration management system will want to keep
inventory in a different software system. Ansible provides a basic
text-based system as described in Inventory but what if you want to use
something else?

#### What's left?
- Chronos as a replacement for Chron
- zabbix module for better automations
  - scaling down
  - adding JMX interfaces
  - eventually express all items/triggers from a dictionary
- try out other providers
- onboarding is easier since the architecture is clearly defined!

#### modules are fun and easy to write
http://docs.ansible.com/modules.html

#### Questions
**Q:**
How to do you manage resources on AWS?
Deletion?
**A:**
Security groups used
Scaling down is interesting

**Q:**
Inventory?
**A:**
We use ec2.py

**Q:**
How long does a deploy take?
**A:**
About 30 minutes!
Build process is slowest part of the deploy
That's why pre-created artifacts may work well.

**Q:**
Do you use AMI's
**A:**
Nah

**Q:**
How do you test?
**A:**
Still Working on it.
Verification tests: everything is up and working
Benchmarks wanted: identify problems in the wild.
No Benchmarks piece yet. They use Grinder.
test strategies in ansible docs:
http://docs.ansible.com/test_strategies.html

**Q:**
Zero Downtime Deploy?
**A:**
They update one server at a time
If any more that 10% of the builds fail, they deploy will be halted

**Q:**
How do you deal with multiple zones?
**A:**
We don't want to manage AMI's
We can't catch spikes with autoscale.
We don't do a lot of autoscaling because spikes are clearly define.
We do 2 definitions in 2 zones in a dc

**Q:**
Continuous Integration?
**A:**
bamboo and ansible
