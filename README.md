Windows host/Linux guest configuration tool
=========

This role will provision all required tools and utils for hybrid environment development.

Requirements
------------

* [Vagrant](https://www.vagrantup.com/)
* [VirtualBox](https://www.virtualbox.org/)
* WinRM should be configured. [This script](https://github.com/ansible/ansible/blob/devel/examples/scripts/ConfigureRemotingForAnsible.ps1) can be used to set up the basics

How-to
------------

* Place your ansible-vault password to `credentials/ansible_vault_password`.
* Place your windows login & password to `credentials/win_inventory`.
* Modify `Vagrantfile` and enter your username and update link to github public key.
* Add correct values in `host.yml` & `guest.yml`.
* Execute `vagrant up` in project dir and see how magic happenes.
