# Ansible Role for Riak KV & TS

[![Build Status](https://travis-ci.org/basho-labs/puppet-riak.svg?branch=master)](https://travis-ci.org/basho-labs/ansible-riak)
[![Ansible Galaxy](https://img.shields.io/ansible/role/8095.svg?maxAge=2592000)](https://galaxy.ansible.com/basho-labs/riak-kv/)

**Ansible Riak** is an Ansible role designed to install & configure Riak KV & TS. In combination with Ansible hosts, it can be used to configure a single node or an [entire cluster](#building-a-cluster).

1. [Installation](#installation)
1. [Documentation](#documentation)
1. [Examples](#examples)
1. [Contributing](#contributing)
1. [Roadmap](#roadmap)
1. [License and Authors](#license-and-authors)

## Installation

### Dependencies

* Ansible 2.1+

### Ansible Galaxy Install

`$ ansible-galaxy install basho-labs.riak-kv`

Then reference the role in your playbooks or other roles using the key `basho-labs.riak-kv`.

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

Internally, we have a [vagrant-ansible package](basho-labs/riak-clients-vagrant) that some of us use to test our client libs. We also [created a role](https://github.com/basho-labs/ansible-roles/blob/b2a93e362a36bdcb0ec5dbbb781cac0b6a1f8f90/README.md#riak-kv-testing) that sets up the environment needed for our library tests and declares [this role as a dependency](https://github.com/basho-labs/ansible-roles/blob/b2a93e362a36bdcb0ec5dbbb781cac0b6a1f8f90/riak_testing/meta/main.yml#L4-L5).

#### Installing Riak TS

```yaml
---
- hosts: riakts
  sudo: true
  roles:
    - { role: ansible-riak }
  vars:
    riak_package: 'riak-ts'
    riak_backend: leveldb
    riak_node_name: "riak@{{ ansible_default_ipv4['address'] }}"
    riak_shell_group: 'riak-ts'
    riak_anti_entropy: off
  tasks:
    - name: Set object size warning threshold
      lineinfile: 'dest=/etc/riak/riak.conf line="object.size.warning_threshold = 50K" regexp: "^object.size.warning_threshold ="'

    - name: Set object size maximum threshold
      lineinfile: 'dest=/etc/riak/riak.conf line="object.size.maximum = 500K" regexp: "^object.size.maximum ="'
```

#### Building a Cluster

To [build a cluster](http://docs.basho.com/riak/latest/ops/building/basic-cluster-setup/), you need to command your Riak node to [join the cluster](http://docs.basho.com/riak/latest/ops/running/cluster-admin/#join) by providing it the ring leader. With this role, there are two ways you can do this. Via the [command module](http://docs.ansible.com/ansible/command_module.html) and cli tool riak-admin or via the [Ansible Riak module](http://docs.ansible.com/ansible/riak_module.html).

#### Command Module

```yaml
---
- hosts: riak
  sudo: true
  roles:
    - { role: ansible-riak }
  vars:
    ring_leader: riak1@127.0.0.1
  tasks:
    - name: Join the cluster
      command: '{{ riak_admin }} cluster join {{ ring_leader }}'

    - name: Check Riak Ring
      command: '{{ riak_admin }} cluster status'
      register: riak_ring_status

    - name: Plan the cluster
      command: '{{ riak_admin }} cluster plan'
      when: riak_ring_status.stdout.find('joining') > 0

    - name: Commit the cluster
      command: '{{ riak_admin }} cluster commit'
      when: riak_ring_status.stdout.find('joining') > 0
```

#### Riak Module

The Riak module has the additional benefit of using the wait_for_ring & wait_for_handoffs functionality.


```yaml
---
- hosts: riak
  sudo: true
  roles:
    - { role: ansible-riak }
  vars:
    ring_leader: riak1@127.0.0.1
  tasks:
    - name: Join the cluster
      riak: command=join target_node={{ ring_leader }}

    - name: Check Riak Ring
      command: 'riak-admin cluster status'
      register: riak_ring_status

    - name: Plan the cluster
      riak: command=plan wait_for_ring=300
      when: riak_ring_status.stdout.find('joining') > 0

    - name: Commit the cluster
      riak: command=commit wait_for_handoffs=300
      when: riak_ring_status.stdout.find('joining') > 0
```

## Contributing

This repo's maintainers are engineers at Basho and we welcome your contribution to the project! You can start by reviewing [CONTRIBUTING.md](CONTRIBUTING.md) for information on everything from testing to coding standards.

## Roadmap

* Nothing planned at this time.

## License and Authors

* Author: Bryan Hunt (https://github.com/binarytemple)
* Author: Christopher Mancini (https://github.com/christophermancini)

Copyright (c) 2016 Basho Technologies, Inc. Licensed under the Apache License, Version 2.0 (the "License"). For more details, see [License](License).
