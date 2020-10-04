# ansible-network

Repository for Network Automation with Ansible
This Repository is primarily meant as a testdrive for Ansible together with a Panorama

## Repository structure
Based on default directory structures

```
──inventory
  ├───dev
  │   ├───goup_vars
  │   │   └───all
  │   └───host_vars
  └───test
      ├───goup_vars
      │   └───all
      └───host_vars
──playbooks
      └───roles
          ├───action_*
          ├───xxxx_*
          └───panos_*
──vault
```

## Getting Started

These instructions will prepare the basic setup with an example lab for a Panorama staged with some basic config by an Ansible host.
Copy the project files form Git and make sure the host system meets the prerequisites.

### Prerequisites

When running on a local installation a System with at least 16G memory is recommended

* Install vagrant https://www.vagrantup.com/downloads.html
* Install Virtualbox https://www.virtualbox.org/wiki/Downloads
* Install Vagrant + plugins on the host system
* INstall VMware Workstation 16 Player

```
vagrant plugin install vagrant-host-shell
vagrant plugin install vagrant-vbguest
```
### System description
Development Machines:
* Unbuntu host - running Ansible in Virtualbox
* VM Panorama - running in VMware Workstation

The unbuntu host has roughly the following installed:
* A Python build with all essentials to run Ansible
* Ansible - provisioning, configuration management tool
* Napalm - Network Automation and Programmability Abstraction Layer with Multivendor support

The Panorama hosts image used for testing is a Panorama 9.1.2

###  Network design Development ###
```
┌──────────────┐
│  VirtualBox  │
├──────────────┤
│    ansible   │
└─────┬────────┘
      |
┌─────┴─────┐
│ VMware WS │
├───────────┤
│ Panorama  │
└───────────┘
```

### Installing

A step by step series of examples that tell you how to get a development env running.

```
git clone {{repo_loaction}}
cd ~working_dir~/ansible-network/
vagrant up
```
Initial installation will take time: (Also due to some heavy memory utilisation)
* downloading of several base boxes from Vagrant cloud depends also on speeds of the internets may take upto 3 minutes
* Initial provisioning of 1 x Ansible-host,
* Deploying a playbook with Ansible approx 1 minute

After initial installation use when ever posssible:
```
vagrant suspend
vagrant resume
```

Setup and install Panorama:

In VMware WS 16 Player > Player > File > open > "Panorama-ESX-9.1.2.ova"

Adjust setting:
* Ethernet Host only (uses "192.168.159.x" as default ip-range VMware)

Start the image (....wait a couple minutes for "PA-CMS login:" and use admin & admin untill reponse is as below)

```
Enter old password: admin
Enter new password: {{this_is_your_new_pass}}
Confirm password: {{this_is_your_new_pass}}

config
set deviceconfig system ip-address 192.168.159.200
commit
```

https://192.168.159.200 > login : admin & {{this_is_your_new_pass}}

## Running the playbooks

Start off course with the Prerequisites and Installation.
Then test the setup by running a first playbook:

[playbook.yml](./playbooks/playbooks.md#playbook)
```
vagrant ssh ansible
ansible-playbook /vagrant/playbook/playbook.yml -vv
```

When using vault
```
vagrant ssh ansible
ansible-playbook /vagrant/playbook/playbook.yml -vv --ask-vault-pass
```

More info and playbooks here:
[playbooks](./playbooks/playbooks.md)
## Deployment

There are 2 environmemnts where this code is used:

* Development - Only running localy on developer laptop with Vagrant and Virtualbox
  * Deploy config only from Ansible host within same setup
  * Limited functionality (example: no AWX , no config of panorama as in production, no CICD pipelines)
* Test -

How to deploy:


## Built With
* Anisble - The Ansible framework as configuration deployment tool
* Vagrant - Used as orchestration tool to build an environment of connected devices
* Virtualbox - Used to virtualise
* Jinja2 - Templating language (loosly based on a python like syntax)
* YAML - Dictionary files used for
  * variable files as in the ./inventory/
  * Ansible playbooks
* Anisble Vault - Used to save secrets within the repo "Vault password"

## Authors

* **Roeland Siegers**
