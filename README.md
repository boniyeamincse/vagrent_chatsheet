# Vagrant Cheatsheet and Tutorial

## What is Vagrant?

Vagrant is an open-source tool for building and managing virtualized development environments. It provides a simple and consistent workflow for developers to create lightweight, portable, and reproducible environments. Vagrant abstracts the complexities of setting up virtual machines (VMs) and allows developers to define their infrastructure as code.

## Why Use Vagrant?

🎯 Consistency: Ensures that development environments are identical across different machines.

🎯 Portability: Allows developers to share environments easily using a Vagrantfile.

🎯 Automation: Automates VM provisioning using scripts.

🎯 Supports Multiple Providers: Works with VirtualBox, VMware, Hyper-V, Docker, and more.

🎯 Integration: Can be used with tools like Ansible, Puppet, and Chef for configuration management.
Typing `vagrant` from the command line will display a list of all available commands.

Be sure that you are in the same directory as the Vagrantfile when running these commands!



# Installing Vagrant

## Prerequisites

1. Install a virtualization provider:

    🎯VirtualBox (Recommended) - Download here

    🎯VMware (Requires Vagrant VMware plugin)

   🎯Hyper-V (Available on Windows Pro/Enterprise)

    🎯Docker (For containerized environments)

## Download and install Vagrant:

Download Vagrant




# Creating a VM
- `vagrant init`           -- Initialize Vagrant with a Vagrantfile and ./.vagrant directory, using no specified base image. Before you can do vagrant up, you'll need to specify a base image in the Vagrantfile.
- `vagrant init <boxpath>` -- Initialize Vagrant with a specific box. To find a box, go to the [public Vagrant box catalog](https://app.vagrantup.com/boxes/search). When you find one you like, just replace it's name with boxpath. For example, `vagrant init ubuntu/trusty64`.

# Starting a VM
- `vagrant up`                  -- starts vagrant environment (also provisions only on the FIRST vagrant up)
- `vagrant resume`              -- resume a suspended machine (vagrant up works just fine for this as well)
- `vagrant provision`           -- forces reprovisioning of the vagrant machine
- `vagrant reload`              -- restarts vagrant machine, loads new Vagrantfile configuration
- `vagrant reload --provision`  -- restart the virtual machine and force provisioning

# Getting into a VM
- `vagrant ssh`           -- connects to machine via SSH
- `vagrant ssh <boxname>` -- If you give your box a name in your Vagrantfile, you can ssh into it with boxname. Works from any directory.

# Stopping a VM
- `vagrant halt`        -- stops the vagrant machine
- `vagrant suspend`     -- suspends a virtual machine (remembers state)

# Cleaning Up a VM
- `vagrant destroy`     -- stops and deletes all traces of the vagrant machine
- `vagrant destroy -f`   -- same as above, without confirmation

# Boxes
- `vagrant box list`              -- see a list of all installed boxes on your computer
- `vagrant box add <name> <url>`  -- download a box image to your computer
- `vagrant box outdated`          -- check for updates vagrant box update
- `vagrant box remove <name>`   -- deletes a box from the machine
- `vagrant package`               -- packages a running virtualbox env in a reusable box

# Saving Progress
-`vagrant snapshot save [options] [vm-name] <name>` -- vm-name is often `default`. Allows us to save so that we can rollback at a later time

# Tips
- `vagrant -v`                    -- get the vagrant version
- `vagrant status`                -- outputs status of the vagrant machine
- `vagrant global-status`         -- outputs status of all vagrant machines
- `vagrant global-status --prune` -- same as above, but prunes invalid entries
- `vagrant provision --debug`     -- use the debug flag to increase the verbosity of the output
- `vagrant push`                  -- yes, vagrant can be configured to [deploy code](http://docs.vagrantup.com/v2/push/index.html)!
- `vagrant up --provision | tee provision.log`  -- Runs `vagrant up`, forces provisioning and logs all output to a file

# Plugins
- [vagrant-hostsupdater](https://github.com/cogitatio/vagrant-hostsupdater) : `$ vagrant plugin install vagrant-hostsupdater` to update your `/etc/hosts` file automatically each time you start/stop your vagrant box.

# Advanced Vagrant Usage

## Configuring a Vagrantfile

Customize the Vagrantfile to define the VM configuration:

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/trusty64"
  config.vm.network "private_network", type: "dhcp"
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "1024"
  end
end
```

- `config.vm.box` – Defines the base image.
- `config.vm.network` – Sets up networking (e.g., private, public, bridged).
- `config.vm.provider` – Configures the virtualization provider (e.g., VirtualBox, VMware).

## Provisioning with Shell Scripts

Automate VM setup using shell provisioning:

```ruby
config.vm.provision "shell", inline: <<-SHELL
  apt-get update
  apt-get install -y nginx
SHELL
```

This installs Nginx automatically when the VM is started.

## Using Multiple Machines

Define multiple VMs in the Vagrantfile:

```ruby
Vagrant.configure("2") do |config|
  config.vm.define "web" do |web|
    web.vm.box = "ubuntu/trusty64"
  end
  config.vm.define "db" do |db|
    db.vm.box = "ubuntu/trusty64"
  end
end
```

Start them with:

```sh
vagrant up web
vagrant up db
```

## Conclusion

Vagrant is a powerful tool for managing virtualized development environments. By using a Vagrantfile, you can automate and share reproducible environments with ease. With this cheatsheet, you now have a quick reference for the most commonly used Vagrant commands and configurations.

For more details, check out the [official Vagrant documentation](https://www.vagrantup.com/docs).



# Notes
- If you are using [VVV](https://github.com/varying-vagrant-vagrants/vvv/), you can enable xdebug by running `vagrant ssh` and then `xdebug_on` from the virtual machine's CLI.
