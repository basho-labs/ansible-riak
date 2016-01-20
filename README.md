# Ansible Role for Riak KV

**Ansible Riak** is an Ansible role designed to install & configure Riak KV on a single node.

1. [Installation](#installation)
1. [Documentation](#documentation)
1. [Examples](#examples)
1. [Contributing](#contributing)
	* [An honest disclaimer](#an-honest-disclaimer)
1. [Roadmap](#roadmap)
1. [License and Authors](#license-and-authors)

## Installation

### Dependencies

* Ansible 1.6+

### Ansible Galaxy Install

`$ ansible-galaxy install christophermancini.riak-kv`

Then reference the role in your playbooks or other roles using the key `christophermancini.riak-kv`.

### Manual Install

To manually install this role, clone the repository or extract the distribution package to your roles directory.

For example, if you use the default location for roles, the roles directory would be a sibling of your playbook.yml file, `roles/`. If you clone this repository to your roles directory, you would then reference the role with the key `ansible-riak`.

## Documentation

All documentation for this role is included within this section of the README.

### Variables

All of the variables that can be used in this role can be found in the [variable defaults file](defaults/main.yml). To override any of the default variables, all you have to do is set them in the `vars:` section of your playbook.yml or create your own role that defines this role as a dependency, using the same var names used in this role.

### Templates

There are currently four templates included with the role. The most important is the `riak.conf.j2` template, as it configures the operating parameters for Riak KV. If you have specific operating requirements for your ring that differ significantly from the distribution configuration, you can override this template with your own.

There are two different ways to override the default template:

* Override the [riak_conf_template](defaults/main.yml#L14) variable and set it to the absolute / relative path to the template on the local system
* Create a new role that defines this role as a dependency and save your template file in the templates directory using the same exact name as the template you want to override, in this case `riak.conf.j2`

### Examples

#### Overriding Default Variables via Playbook

```yaml
---
- hosts: riak
  sudo: true
  roles:
    - { role: ansible-riak }
  vars:
  	riak_pb_bind_ip: 10.29.7.192
  	riak_pb_port:    10017
```

#### Overriding Default Variables via Role Dependency

Internally, we have a [vagrant-ansible package](basho-labs/riak-clients-vagrant) that some of us use to test our client libs. In this package, we [created a role](https://github.com/basho-labs/riak-clients-vagrant/tree/master/provisioning/roles/integration_testing) that sets up the environment needed for our library tests and declares [this role as a dependency](https://github.com/basho-labs/riak-clients-vagrant/blob/master/provisioning/roles/integration_testing/meta/main.yml).

#### Building a Cluster



## Contributing

This repo's maintainers are engineers at Basho and we welcome your contribution to the project! You can start by reviewing [CONTRIBUTING.md](CONTRIBUTING.md) for information on everything from testing to coding standards.

### An honest disclaimer

Due to our obsession with stability and our rich ecosystem of users, community updates on this repo may take a little longer to review.

The most helpful way to contribute is by reporting your experience through issues. Issues may not be updated while we review internally, but they're still incredibly appreciated.

Thank you for being part of the community! We love you for it.

## Roadmap

* Nothing planned at this time.

## License and Authors

* Author: Bryan Hunt (https://github.com/binarytemple)
* Author: Christopher Mancini (https://github.com/christophermancini)

Copyright (c) 2016 Basho Technologies, Inc. Licensed under the Apache License, Version 2.0 (the "License"). For more details, see [License](License).