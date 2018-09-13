# Create a new VM based on the baesd image you have created

```
cd C:\My VMs\
mkdir one-off
cd  one-off\
vagrant box add  one-off C:\Users\as56\.vagrant.d\templates\centos6-template.box
```

Which has just created:

```
C:\Users\as56\.vagrant.d\boxes\ one-off\0\virtualbox
```

Now create local VagrantFile:

```
# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  config.vm.box = "one-off"
  config.vm.synced_folder "C:/vagrant-shares/centos6-general", "/vagrant-share", type: "nfs"
 
  config.vm.define "one-off" do |one_off|
    one_off.vm.hostname = "one-off"
    one_off.vm.network "private_network", ip: "192.168.50.64"
  end
end
```

Up the VM:

```
vagrant up
```

Login:

```
ssh vagrant@192.168.50.64

Warning: Permanently added '192.168.50.64' (RSA) to the list of known hosts.
X11 forwarding request failed on channel 0
Last login: Wed Mar 14 16:27:57 2018 from 10.0.2.2
[vagrant@one-off ~]$ sudo su
[root@one-off vagrant]# cp -r ~vagrant/.ssh ~/
[root@one-off vagrant]# exit

```

Now you can login as root:

```
ssh root@192.168.50.64
```
