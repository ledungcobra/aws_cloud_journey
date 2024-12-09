---
title: "Vagrant"
date: "`r Sys.Date()`"
weight: 4
chapter: false
pre: " <b> 4. </b> "
---

## What is Vagrant?
- This is a tool for building and managing virtual machine environments in a single workflow.

## How to install Vagrant?

- You can refer this link to install vagrant [Vagrant](https://developer.hashicorp.com/vagrant/downloads)

```shell
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/RHEL/hashicorp.repo
sudo yum -y install vagrant
```

## How to use Vagrant?

- First you need to create a vagrant file in your project directory.

```shell
vagrant init
```

- Then you need to create a box by running this command

```shell
vagrant box add centos/7
```

- To run the virtual machine, you can run this command

```shell
vagrant up
```

- To stop the virtual machine, you can run this command

```shell
vagrant halt
```

- To destroy the virtual machine, you can run this command

```shell
vagrant destroy
```

- To update the vagrant file, and you want to have the changes reflect in the virtual machine you can run this command

```shell
vagrant reload
```

- To take snapshot of the virtual machine you can run this command

```shell
vagrant snapshot take <snapshot_name>
```

- To restore the snapshot you can run this command

```shell
vagrant snapshot restore <snapshot_name>
```

- To list all the snapshots you can run this command

```shell
vagrant snapshot list
```

- To delete a snapshot you can run this command

```shell
vagrant snapshot delete <snapshot_name>
```

- To restore the snapshot you can run this command

```shell
vagrant snapshot restore <snapshot_name>
```

- To ssh into the virtual machine you can run this command

```shell
vagrant ssh
```
- The Vagrantfile should look like this

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"
  
  # Forwarding port from host to guest port 80 is port the VM, 8080 is port on host machine
  config.vm.network "forwarded_port", guest: 80, host: 8080

  # Syncing folder from host to guest the ./data is the folder on host machine and /vagrant is the folder on guest machine  
  config.vm.synced_folder "./data", "/vagrant"

  # Setting up the provider and the memory and cpu for the virtual machine
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "1024"
    vb.cpus = "2"
  end

  # configure shell script to run when the virtual machine is provisioned
  config.vm.provision "shell", inline: <<-SHELL
    yum update
    yum install -y git
  SHELL
end
```
## Vagrant Providers
- Vagrant supports multiple providers like VirtualBox, VMware, Hyper-V, Docker, custom providers etc.


