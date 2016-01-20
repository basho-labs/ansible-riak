## Contributing

This repo's maintainers are engineers at Basho and we welcome your contribution to the project!

This role is designed to be used with the [Ansible Galaxy](https://galaxy.ansible.com/) project, but can also work independently.  Details on how to use Ansible Galaxy can be found on the project's website, but to install a role to your ansible controller you would run:

	ansible-galaxy install christophermancini.riak-kv

 What's a role?  How do I use them? Check out[ this link](http://docs.ansible.com/playbooks_roles.html#roles).

If you wish to tweak this role yourself, you'll need to name your local repo the same way Ansible Galaxy would because module dependencies are mapped with Ansible Galaxy's naming scheme.

Place your roles in a role directory.  Make sure that role directory is specified in
$HOME/.ansible.cfg, default is `/etc/ansible/roles`.

	[defaults]
	roles_path = /path/to/my/roles

Example:

* official repo name: ansible-riak
* galaxy name: christophermancini.riak-kv

	cd /path/to/my/roles
	git clone https://github.com/basho/ansible-riak.git christophermancini.riak-kv

### An honest disclaimer

Due to our obsession with stability and our rich ecosystem of users, community updates on this repo may take a little longer to review.

The most helpful way to contribute is by reporting your experience through issues. Issues may not be updated while we review internally, but they're still incredibly appreciated.

Thank you for being part of the community! We love you for it.

## Helping through sample code

The most immediately helpful way you can benefit this project is by forking the repo, **adding further [examples](README.md#examples)** and submitting a pull request.

## How-to contribute to the Ansible Riak role

**_IMPORTANT_**: This is an open source project licensed under the Apache 2.0 License. We encourage contributions to the project from the community. We ask that you keep in mind these considerations when planning your contribution.

* Whether your contribution is for a bug fix or a feature request, **create an [Issue](https://github.com/basho-labs/ansible-riak/issues)** and let us know what you are thinking.
* **For bugs**, if you have already found a fix, feel free to submit a Pull Request referencing the Issue you created.
* **For feature requests**, we want to improve upon the library incrementally which means small changes at a time. In order ensure your PR can be reviewed in a timely manner, please keep PRs small, e.g. <10 files and <500 lines changed. If you think this is unrealistic, then mention that within the Issue and we can discuss it.
* Before you open the PR, please review the following regarding Coding Standards, Docblock comments and unit / integration tests to reduce delays in getting your changes approved.

### Pull Request Process

Here’s how to get started:

* Fork the appropriate sub-projects that are affected by your change.
* Create a topic branch for your change and checkout that branch.
     `git checkout -b some-topic-branch`
* Make your changes and run the test suite if one is provided. (see below)
* Commit your changes and push them to your fork.
* Open pull requests for the appropriate projects.
* Contributors will review your pull request, suggest changes, and merge it when it’s ready and/or offer feedback.
`

## Thank You

You can [read the full guidelines for bug reporting and code contributions](http://docs.basho.com/riak/latest/community/bugs/) on the Riak Docs.

And **thank you!** Your contribution is incredible important to us.