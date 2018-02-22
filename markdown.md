name: inverse
layout: true
class: center, middle, inverse
---
# Using Ansible<sup>*</sup> effectively for deployment

<sup>*</sup> Can be applied to other configuration management tools

.footnote[.pull-right[![odecee icon](http://odecee.com.au/wp-content/themes/odecee/library/images/logo-white.svg)]]

???
Opportunity to talk and share

Something I picked up 5-6 years back, but want to share as I still don't see it being used and want to increase it's adoption.

---
layout:false
class: center, middle, inverse

## Who am I?
.right[
### Chao Luan
.pull-right[![Gravatar](https://s.gravatar.com/avatar/53fe10fa1ac0c9073119e7227647f69f?s=80)]

Lead DevOps Engineer @ Odecee

.fa[.fa-github[]] mistaka0s | .fa[.fa-linkedin-square[]] chao.luan
]

---
class: middle

.left-column[
Who am I?
]
.right-column[.middle[
- 16 years in IT, working in consultancies.

- Systems Admin background

- Middleware/Build Engineer ~ 2009

- Systems automation since 2010.

- Building platforms since then.

- Puppet in 2009/2010

- Ansible since 2012

]]


---
class: center, middle, inverse
# Agenda

---
class: middle

.left-column[
Agenda
]
.right-column[
1. Intro.

2. What Ansible (and others) good at

3. Idempotency

4. Packaging your application

5. Variable order and precedence

6. Dynamic Inventory

7. Roles

8. Host Groups

]

---
class: center, middle, inverse
# Intro

---
class: middle

.left-column[
Intro
]

.right-column[
- Not about: how to use Ansible

- Techniques and principles
    + Can be applied to:
        + Puppet
        + Chef
        + Salt
        + et al

- Focus on Ansible
    + Best tool for the job at this moment.
    + Low barrier to entry (YAML based, not it's own DSL)
    + You get a lot out of the box.
    + Masterless, don't need a server, just somewhere with Ansible.
]

---
class: center, middle, inverse
# What Ansible (and others) are good at



---
class: middle
name: how
.left-column[
What Ansible (and others) are good at
]

.right-column[
All software configuration tools are good at three(3) things.
- Services
- Packages
- Files
]

--
.right-column[
- Interacting with APIs/remote services (Ansible)
]

---
class: center, middle, inverse
# Walk before you run

---
class: middle
name: how
.left-column[
Walk before you run
]

.right-column[
- Take the time to plan

- Run through things manually, end to end
    + What happens when you run the commands/instructions again
        * Does it fail? How graceful is it? How destructive is it?

- Document the steps you want to automate
    + What do you need to support this?
]

---
class: middle
name: how
.left-column[
Walk before you run
]

.right-column[
- Focus on the outcome, not the steps.
    + 'I need to configure a virtual host'
    + 'I need to edit an Apache/NGINX config file with this content'

- Start fresh each time
    + Note down dependencies
    + Changes made without writing it down - especially when you're trying to figure things out
]

---
class: middle
name: how
.left-column[
Walk before you run

(an example)
]

.right-column[
**Adding a user**

First time
```
# useradd splunk
# echo $?
0
```
SUCCESS!


Run it again...
```
# useradd splunk
useradd: user 'splunk' already exists
# echo $?
9
```
OH....
]
???
nice segue into idempotency

---
class: center, middle, inverse
# Idempotency

---
class: middle
name: how
.left-column[
Idempotency
]

.right-column[
*Idempotency*

```
In computer science, the term idempotent is used more
comprehensively to describe an operation that will
produce the same results if executed once or multiple times.
```
-- Wikipedia

]

---
class: middle
name: how
.left-column[
Idempotency
]
.right-column[
Should always be aiming for idempotency when writing your automation code

- Run your Ansible playbook multiple times, do you get the same behaviour? Any changes made between runs?

- Most tasks within Ansible are idempotent
    + In the `useradd` example before
        + have to check where or not the user exists first
        + probably wrapped up in a script

    + Ansible's `user` task takes care of that for you.

- Try to avoid `command` and `shell`
    + you'd have to make sure it's idempotent yourself.
]


---
class: center, middle, inverse
# Packaging

---
class: middle
name: how
.left-column[
Packaging
]

.right-column[
Leverage the operating system's package management system to do more of the complex stuff, if you can.

- Packaging helps with idempotency

- Not used often enough
    + simplifies a lot of things if you do

- Transactional
    + Every RPM installed is a transaction in itself

- Each file is tracked in the YUM/RPM database

- Can easily determine what version of the application is deployed
    + 

]
