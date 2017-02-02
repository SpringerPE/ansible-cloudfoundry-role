# ansible-cloudfoundry-role

Ansible role to setup Cloud Foundry env variables, feature flags, domains,
security groups, quotas, users, organizations and spaces


# Dependencies

This role uses the Cloud Foundry modules included in the `library` folder.
Those modules have one requirement: https://github.com/SpringerPE/python-cfconfigurator
You can install all requirements for the modules by running *pip*:
`pip install -r requirements.txt`


# Usage and Configuration

First of all, install the role using `ansible-galaxy`. The role is not available
in https://galaxy.ansible.com/ because they do not support organizations. But,
fortunately `ansible-galaxy` understands github repos, so you can install this
role and use it in your playbooks by creating a `requirements.yml`:

```
- name: cf
  src: https://github.com/SpringerPE/ansible-cloudfoundry-role
  version: origin/master
```
and type:
```
ansible-galaxy install -p ./roles -r requirements.yml
```

Next, for testing, create a simple playbook `cf.yml`:

```
---
- name: cf playbook
  hosts: cf
  serial: 1
  gather_facts: false

  roles:
  - role: cf
```

and a inventory `test.ini` with the proper CF settings:

```
[cf:children]
test

[test:children]
# You can add here more environments, they are just names!
# and for each enviroment you can override the variables
# defined in groups_vars/test.yml by creating a new file
# with the same name.
api.test.cf.springer.com


[api.test.cf.springer.com]
api.test.cf.springer.com

[api.test.cf.springer.com:vars]
cf_admin_user=pepe
cf_admin_password="password"
cf_api="https://api.test.cf.springer.com"


[client]
# Client is where the actions will be executed, it points to
# localhost, but it could point to a different host to perform
# actions from there
localhost ansible_connection=local ansible_become=false
```

after setting up the variables defined in `defaults/main.yml` using
`groups_vars` or `host_vars` features, run ansible:

```
ansible-playbook -i test.ini  cf.yml
```

and done!



# Author

Jose Riguera Lopez, jose.riguera@springer.com
SpringerNaure Platform Engineering

Copyright 2017 SpringerNature


