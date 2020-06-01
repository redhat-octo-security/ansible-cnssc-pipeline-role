# WORK IN PROGRESS.

Ansible Cloud Native Secure Supply Chain Role
=============================================

![CI](https://github.com/redhat-octo-security/ansible-cloud-native-ssc/workflows/CI/badge.svg)

Ansible role to deploy an environment for development or demonstration of a
cloud native secure supply chain.

This role will set a complete golang environment for the root user. We use root
as this role has no business being used in a production environment at present
so a security context is not a concern.

All golang variables and other tools to go get can be found in:

`roles/ansible-cloud-native-ssc/vars/main.yml`

Planned Projects
----------------

* - [x] [Tekton](https://tekton.dev/)
* - [x] [In-toto](https://in-toto.io/) (golang version)
* - [ ] [Open-Policy-Agent](https://www.openpolicyagent.org/)
* - [ ] [Keylime](https://keylime.dev/)
* - [ ] [Trillian](https://github.com/google/trillian)

Usage
-----

Run the example playbook against your target remote node(s).

```
ansible-playbook -i your_hosts playbook.yml
```
Vagrant
-------

A `Vagrantfile` is available for provisioning.

This requires `vagrant-reload`:

`vagrant plugin install vagrant-reload`

This requires `vagrant-sshfs`:

`vagrant plugin install vagrant-sshfs`


Clone the repository and then simply run with the following additional args
added to the `vagrant` command:

* `--repo`: This mounts a local folder into the virtual machine.
* `--cpus`: The amount of CPU's. If not provided, it defaults to `2`
* `--memory`: The amount of memory to assign.  If not provided, it defaults to `2048`


Note `--repo` could be a folder which contains local repositories you wish to
test or build against within the VM. This will be found in `/root/my_repos`.


An example using libvirt:

```
vagrant --repo=/home/jdoe/pipeline --cpus=4 --memory=4096  up --provider libvirt --provision
```

An example using VirtualBox:

```
vagrant --repo=/home/jdoe/pipeline --cpus=4 --memory=4096  up --provider virtualbox --provision
```

| NOTE: Customized args (`--repos` etc), come before the main vagrant args (such as `up`, `--provider`) |
| --- |

Once the VM is started, `vagrant ssh` into the VM and `sudo -i` to root
