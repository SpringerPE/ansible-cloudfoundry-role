# ansible-cloudfoundry-role

Ansible role to setup Cloud Foundry env variables, feature flags, domains,
security groups, quotas, users, organizations and spaces


# Dependencies

This role uses the Cloud Foundry modules included in the `library` folder.
Those modules have only one requirement: https://github.com/SpringerPE/python-cfconfigurator .
You can install it by running *pip*: `pip install -r requirements.txt`


# Usage

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
cf_admin_user=pepeCloud Foundry resource automation using Ansible

Have a look at the file: https://github.com/SpringerPE/ansible-cloudfoundry/blob/master/inventory/group_vars/cf.yml to see how to define the resources: feature flags, domains, security groups, quotas, environment variables, users, organizations and spaces.
cf_admin_password="password"
cf_api="https://api.test.cf.springer.com"


[client]
# Client is where the actions will be executed, it points to
# localhost, but it could point to a different host to perform
# actions from there
localhost ansible_connection=local ansible_become=false
```

Now, have a look at the file `defaults/main.yml` to see how to define the
resources: feature flags, domains, security groups, quotas, environment
variables, users, organizations and spaces.

You can manage different Cloud Foundry environments by using inventory files
like this one: https://github.com/SpringerPE/ansible-cloudfoundry/blob/master/inventory/cf.ini
It makes possible to define some common global configuration variables by splitting
them in different files (Ansible superpower!)

Once the CF credentials are defined in the inventory and the resources in the manifest,
just run ansible:

```
ansible-playbook -i inventory/cf.ini cf.yml
```

and done!


## Components

Inside the `library` folder there are a set of Ansible modules to manage Cloud
Foundry configuration entities, not aimed to manage apps, routes, service
brokers, etc.

Current available modules make possible to manage:

* **cf_config**: Environment variables, feature flags and default security groups.
* **cf_domain**: Private (with owner/shared organizations) and shared domains
* **cf_org**: Organizations (and user roles: user, manager, auditor and billing_manager)
* **cf_space**: Spaces (and user roles: user, manager, auditor)
* **cf_quota**: Organization and space Quotas
* **cf_secgroup**: Security groups
* **cf_secgroup_rule**: Security group rules
* **cf_user**: Manage CF users via UAA
* **cf_org_facts**: Get facts from a CF Org or Space

They depend on https://github.com/SpringerPE/python-cfconfigurator ,
just install it via pip.

For examples, have a look at `tests` folder.


# Author

Jose Riguera Lopez, jose.riguera@springer.com
SpringerNaure Platform Engineering

Copyright 2017 SpringerNature


