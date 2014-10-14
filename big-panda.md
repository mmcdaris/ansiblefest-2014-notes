## BigPanda - Automating ALL THE THINGS with a one-man Ops team
**author:** Dan Chuparkoff
> Dan will present how, in less than 6 months and with a 1-person team,
they went from no infrastructure automation, to having all of their
infrastructure automated with Ansible.  Learn how BigPanda handles
zero-downtime infrastructure updates and connects ansible with their
chat infrastructure, and some strategies on managing automation projects
with very small teams.

#### Universal ran a superbowl add Feb 1 2009, with FREE TICKETS!
viewers: **140 Million** with an **M**
comercial # 1 airs
Instant crash since so many people were trying to get free tickets!
Re-run comercial!
100,000 free tickets in 42 seconds
authorized to give out 300,000 tickets!

Universal Studio guy said something like:
> just get more servers! and run it again!

#### One Man Ops team
Create infrasturture around The one man:

#### Ansible Modules

No matter what the complexity of the tesk it boils down to running and
ansible module, which makes playbooks easy to write and easy to review
later.

#### Simplicty
#### Idempotence
run as many times as you want and it won't hurt anything!
#### Reusable
Every script we write is used 2 times
once when deploying a new version
and again when provisioning a new server
using tags we run our skip tasks or plays as needed:
```
./playbook.yml -i hosts -- --tags infra
# infra could be replaced with run_once
```
#### Extensible
Can be easily extended with JSON
Correlating monitoring alerts with application deployments reduces MTTR
**MTTR:** Mean Time to Repair

#### Automate all the things
No one touches servers, go through ansible

#### 6 steps to automating
###### 1 - Create an ansible workshop
Leverage peer review behavior
Bring agile dev stuff to the ops world
Made into a meetup
[bigpanda/ansible-workshop](https://github.com/bigpandaio/ansible-workshop)

###### 2 - Playbook Playbooks
```
# People can make mistakes here!
ansible-playbook playbook.yml -i hosts -e var1=val1 -e var2=val2
```

Make it better! [bigpanda/ansible-exec](https://github.com/bigpandaio/ansible-exec)

###### 3 - Make Deployments the easiest task!
```
deploy bigpanda
```
There are a very small number of commands like this in bigpanda
Learn the hard stuff one time and make sure the playbook is perfect
Kept healthy through peer reveiw!

Ansible at this point is so easy, everything is an edit from a playbook

###### 4 - BeanBot
bigpanda uses hipchat
Deployment talk happens here
BeanBot listens to hipchat and waits for something like
```
Hey Beanbot!
```

BeanBot - ender's game reference to Ender's friend Bean
Based from HuBot

###### 5 - Infrastructure Confidence
- Able to replace manual deploys
- ensured that everything was configured exactly as intended
- 100% ansible created
all with zero downtime

###### 6 - Connect Alert & Changes
Now that everything is in ansible
We get a changelog for our inrfrastructure
There is an ansible plugin for bigPanda
You get a list of alerts that occur for your ingfrastructure
This bundles problems together

#### Questions!
**Q:**
  You Correlate what was changed and who changed it?
**A:**
  User id is passed when you do a beanbot deploy
  thus ownership is established
