Ansible Automation Platform Daily Demo for PaloAlto
=========
A demo designed to build the PaloAlto Infrastructure needed to showcase many of the use cases related to PaloAlto.  This builds a standalone PaloAlto at Amazon and sets the admin password so you will be ready to go when the playbook is done building the infrastructure. This demo infrastructure takes 20 minutes to spin up.

Notes
=========
1. This demo is designed to work with the Red Hat Demo Platform. Please see the aap.as.code repo below. [aap.as.code](https://github.com/ericcames/aap.as.code "aap.as.code")
2. This demo works with Amazon only currently.
3. Must accept Terms and conditions at this link before running this. https://aws.amazon.com/marketplace/pp/prodview-mn63yjbq37n4c
![alt text](https://github.com/ericcames/aap.dailydemo.Panos/blob/main/images/panosoptin.png "Terms")

Day 0 - Configuration as code (CAC) a repeatable build process for this demo
=========
Configuration as code give you an easy way to recover/move your ansible related artifacts to a new platform.  That includes your hardcoded credentials.  The hardcoded credentials can be safely vaulted in an ansible vault file.  Check out the setup_demo.yml for the configurations for setting up this demo using configuration as code.

[Setup - Panos Daily Demo - CAC](https://github.com/ericcames/aap.dailydemo.Panos/blob/main/playbooks/setup_demo.yml "Setup - Panos Daily Demo - CAC")<br>

Variables used in the setup template
```
my_organization: AmesCO
timezone_id: America/Phoenix
my_vault: Eric Ames
my_aap_credential: Controller Credential
my_aws_credential: AWS
aap_configuration_async_retries: 60
my_remote_vault: >-
  https://raw.githubusercontent.com/ericcames/sourcefiles/refs/heads/main/vault_ames.yml
my_remote_ssh_pub_key: >-
  https://raw.githubusercontent.com/ericcames/sourcefiles/refs/heads/main/id_rsa.pub
```

Day 1
=========

# The PaloAlto User Interface

![alt text](https://github.com/ericcames/aap.dailydemo.Panos/blob/main/images/palouipre.png "Pre Login")
![alt text](https://github.com/ericcames/aap.dailydemo.Panos/blob/main/images/palouipost.png "Post Login")

**PaloAlto API Documentation**

![alt text](https://github.com/ericcames/aap.dailydemo.Panos/blob/main/images/palorestdoc.png "API Docs")

```
https://FQDN/restapi-doc/
```

**The job logs contain the URL needed to login to the gui**
![alt text](https://github.com/ericcames/aap.dailydemo.Panos/blob/main/images/palojoblog.png "Job Log")

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

[1. Create or Delete PaloAlto](https://github.com/ericcames/aap.dailydemo.Panos/blob/main/playbooks/main.yml "main.yml")<br>
![alt text](https://github.com/ericcames/aap.dailydemo.Panos/blob/main/images/palomain.png "Main Playbook")<br>

Tag used:
```
create
  or
remove
```
**The Credentials Types**

Red Hat Ansible Automation Platform<br>
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
Looking for other Daily Demos?
=========

- [AAP Daily Demo Windows](https://github.com/ericcames/aap.dailydemo.windows "AAP Daily Demo Windows")
- [AAP Daily Demo Linux](https://github.com/ericcames/aap.dailydemo.linux "AAP Daily Demo Linux")
- [AAP Daily Demo F5](https://github.com/ericcames/aap.dailydemo.F5 "AAP Daily Demo F5")
- [AAP Daily Demo Panos](https://github.com/ericcames/aap.dailydemo.Panos "AAP Daily Demo Panos")
- [AAP Daily Demo Satellite](https://github.com/ericcames/aap.dailydemo.satellite "AAP Daily Demo Satellite")
