# vagrant-dev-ubuntu

Vagrant-based development environment using [Lubuntu](http://lubuntu.net/), with GUI (i.e., not headless).

The Vagrant base box is a Lubuntu 16.04 64-bit installation, [available from Hashicorp Atlas](https://atlas.hashicorp.com/halvards/boxes/lubuntu1604/).

## Installation

Prerequisites:

* [VirtualBox](https://www.virtualbox.org/wiki/Downloads). This has been tested with version 5.0.20
* [Vagrant](https://vagrantup.com/downloads.html). This has been tested using version 1.8.1
* [Ansible](https://github.com/ansible/ansible). This has been tested using version 2.1.0.0 (not required for Windows hosts)

Clone this repository, then from the repository folder run this command:

    vagrant up

The `vagrant` user password is `vagrant`.

## Notes

* Edit `playbook.yml` to select your NodeJS version. Options are `node_4.x` and `node_6.x`.
* [nvm](https://github.com/creationix/nvm) is also installed, so it provides another mechanism to switch to other NodeJS versions.
* Don't log in as user `vagrant` via the GUI until provisioning is complete. `~/.profile` is modified as part of provisioning, so if you log in before this is finished
* The login form appears twice after the VM has been created the first time. On subsequent reboots, it only appears once.
* The Vagrantfile copies your private key to the VM so you can access Github et al. via SSH. It assumes the private key file can be found in `~/.ssh/id_rsa`
* You can also SSH to the VM, using `vagrant ssh`
