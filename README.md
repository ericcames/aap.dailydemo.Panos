Ansible Automation Platform Daily Demo for PaloAlto
=========
A demo designed to build the PaloAlto Infrastructure needed to showcase many of the use cases related to PaloAlto.  This builds a standalone PaloAlto at Amazon and sets the admin password so you will be ready to go when the playbook is done building the infrastructure.

Day 1
=========

# The PaloAlto User Interface

![alt text](https://github.com/ericcames/aap.dailydemo.Panos/blob/main/images/palouipre.png "Pre Login")
![alt text](https://github.com/ericcames/aap.dailydemo.Panos/blob/main/images/palouipost.png "Post Login")

**The job logs contain the URL needed to login to the gui**
![alt text](https://github.com/ericcames/aap.dailydemo.F5/blob/main/images/F5joblog.png "Job Log")

```
https://ec2-54-67-87-75.us-west-1.compute.amazonaws.com
```
# The command line; there's no place like home :-)

![alt text](https://github.com/ericcames/aap.dailydemo.Panos/blob/main/images/palocli.png "The command line")

Example login using your ssh key that was shared with Amazon
```
ssh admin@ec2-54-67-87-75.us-west-1.compute.amazonaws.com
```

**The playbooks**

[1. Create the PaloAlto](https://github.com/ericcames/aap.dailydemo.Panos/blob/main/playbooks/palo-create.yml "palo-create.yml")<br>
![alt text](https://github.com/ericcames/aap.dailydemo.Panos/blob/main/images/palocreate.png "Create")<br>

Tag used:
```
create
```
[2. Remove the PaloAlto](https://github.com/ericcames/aap.dailydemo.Panos/blob/main/playbooks/palo-remove.yml "palo-remove.yml")<br>
![alt text](https://github.com/ericcames/aap.dailydemo.Panos/blob/main/images/paloremove.png "Remove")<br>

Tags used:
```
remove
```

**The Credentials Types**

Ansible Controller Credential<br>
Input configuration
```
fields:
  - id: url
    type: string
    label: Controller URL
  - id: user
    type: string
    label: Controller Username
  - id: password
    type: string
    label: Controller Password
    secret: true
required:
  - url
  - user
  - password
```
Injector configuration
```
extra_vars:
  controller_url: '{{url}}'
  controller_user: '{{user}}'
  controller_passwd: '{{password}}'
```
Daily Demo Panos Machine Credential<br>
![alt text](https://github.com/ericcames/aap.dailydemo.Panos/blob/main/images/palomachinecred.png "Machine Credential")<br>

Amazon Web Services Credential<br>

**The AAP Managed Inventory**

![alt text](https://github.com/ericcames/aap.dailydemo.Panos/blob/main/images/paloinventory.png "AAP Managed Inventory")<br>

Group name
```
panosdemo
```

Day 2
=========

**Example Playbooks**
- [Panos example playbooks](https://gitlab.com/mlowcher/panos "Panos example playbooks")


**Execution Environment**<br>
- [Panos](https://quay.io/repository/locust61/panos_ee "Panos Execution Environment")
```
quay.io/repository/locust61/panos_ee
```
f5-execution-environment.yml
```
---
version: 3

dependencies:
  galaxy: requirements.yml

  python:
    - pan-os-python
    - pan-python
    - panos-upgrade-assurance
    - xmltodict
  #system: []

options:
  package_manager_path: /usr/bin/microdnf

images:
  base_image:
    name: registry.redhat.io/ansible-automation-platform-24/ee-minimal-rhel8:latest

```

**A Custom Credential is reguired for Day 2 ops**

Input configuration
```
fields:
  - id: api_key
    type: string
    label: Palo Alto api key
    secret: true
  - id: username
    type: string
    label: Username
  - id: password
    type: string
    label: Password
    secret: true
required:
  - api_key
  - username
  - password
```
Injector configuration
```
extra_vars:
  api_key: '{{ api_key }}'
  username: '{{ username }}'
  password: '{{ password }}'
```
# Looking for the Windows Daily Demo?

- [AAP Daily Demo Windows](https://github.com/ericcames/aap.dailydemo.windows "AAP Daily Demo Windows")

# Looking for the Linux Daily Demo?

- [AAP Daily Demo Linux](https://github.com/ericcames/aap.dailydemo.linux "AAP Daily Demo Linux")
