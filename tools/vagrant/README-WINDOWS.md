# Setting up a Vagrant / VirtualBox VM on Windows 10

## Pre-requisites

- Install VirtualBox - follow instructions
- Install Vagrant - follow instructions
## Get a base image: CentOS 6

In the CMD terminal window:

```
cd C:\HashiCorp\Vagrant\bin
vagrant box add centos/6
```

Choose: 3 (virtualbox)

## Go to directory where the box lives

```
cd C:\Users\as56\.vagrant.d\boxes\centos-VAGRANTSLASH-6\1802.01\virtualbox
```

Edit the local: `VagrantFile` so it looks something like:

```
Vagrant.configure("2") do |config|
  config.vm.base_mac = "525400e0ddcb"
  config.vm.synced_folder "C:/vagrant-shares/centos6", "/vagrant-share", type: "nfs"
  config.vm.network :forwarded_port, guest: 22, host: 2225
  config.vm.box = "centos/6"
  config.vm.network "forwarded_port", guest: 80, host: 8083
  config.vm.network "forwarded_port", guest: 8000, host: 8003, host_ip: "127.0.0.1"  
end
```

NOTE: the `config.vm.base_mac` will be different.

## Test it brings up the VM

```
vagrant up
```

This may show errors but you should still be able to login.

## Login

```
vagrant ssh
```

## Install other useful stuff on the VM

Now get extra stuff installed:

```
sudo su 
yum update -y
yum install -y kernel-devel gcc make

cat > /etc/selinux/config <<-SELINUXCONF
# This file controls the state of SELinux on the system.
# SELINUX= can take one of these three values:
#       enforcing - SELinux security policy is enforced.
#       permissive - SELinux prints warnings instead of enforcing.
#       disabled - SELinux is fully disabled.
SELINUX=disabled
# SELINUXTYPE= type of policy in use. Possible values are:
#       targeted - Only targeted network daemons are protected.
#       strict - Full SELinux protection.
SELINUXTYPE=targeted
SELINUXCONF
```

Exit the server with Ctrl^D

## Reload the VM

```
vagrant reload
```

Login again and install guest additions:

```
vagrant ssh
```

```
sudo su 
yum install -y wget
VN=$(curl -s http://download.virtualbox.org/virtualbox/LATEST.TXT)
wget -c http://download.virtualbox.org/virtualbox/$VN/VBoxGuestAdditions_$VN.iso  \
     -O VBoxGuestAdditions.iso -q

mount VBoxGuestAdditions.iso -o loop /mnt
/mnt/VBoxLinuxAdditions.run --nox11


usermod -aG vboxsf vagrant
umount /mnt
rm -f VBoxGuestAdditions.iso

yum history undo -y 

yum install -y epel-release 
rpm -Uvh http://dist.ceda.ac.uk/yumrepo/RPMS/ceda-yumrepo-0.1-1.ceda.el6.noarch.rpm
yum remove python-nose
yum remove python-setuptools
```

Ctrl^D to exit VM.

## Check shared mounts are set up

```
mkdir C:\vagrant-shares\centos6
```

Update the `VagrantFile`, uncomment the line:

`  config.vm.synced_folder "C:/vagrant-shares/centos6", "/vagrant-share", type: "nfs"`

Try bringing up VM again:

```
vagrant reload
```

## To login in with MobaXTerm

Copy SSH private key to my keys area for MobaXTerm

```
copy C:\Users\as56\.vagrant.d\boxes\centos-VAGRANTSLASH-6\1802.01\virtualbox\.vagrant\machines\default\virtualbox\private_key C:\Users\as56\Documents\MobaXterm\home\.ssh\vagrant_centos6
```

```
exec ssh-agent $SHELL
ssh-add ~/.ssh/vagrant_private_key
ssh -p 2225 vagrant@127.0.0.1
```

## To package up the VM image as a base box that can be re-used

Package up with a new name:

```
mkdir C:\Users\as56\.vagrant.d\templates
vagrant package --output C:\Users\as56\.vagrant.d\templates\centos6-template.box
```


## Create a new VM based on the baesd image you have created

```
cd C:\My VMs\
mkdir ukcp-test
cd ukcp-test\
vagrant box add ukcp-test C:\Users\as56\.vagrant.d\templates\centos6-template.box
```

Which has just created:

```
C:\Users\as56\.vagrant.d\boxes\ukcp-test\0\virtualbox
```

Now create local VagrantFile:

```
vagrant init ukcp-test
```

Now we have a `VagrantFile` instance, add in the following to sync the folder
and provide a distinct SSH port:

```
  config.vm.synced_folder "C:/vagrant-shares/centos6", "/vagrant-share", type: "nfs"
  config.vm.network :forwarded_port, guest: 22, host: 2226
```

Now we login with same private key file:

```
ssh -p 2226 vagrant@localhost
```

And the same vagrant-shares are working!!!

## Set up a static IP on the local network

Halt the VM, then change VagrantFile to include:

```
  config.vm.box = "ukcp-test"
  config.vm.synced_folder "C:/vagrant-shares/centos6", "/vagrant-share", type: "nfs"
  config.vm.network "private_network", ip: "192.168.50.4"
```

Up the VM:

```
vagrant up
```

NOTE: On windows this will ask you to confirm the changes as administrator.

Then try:

```
ssh vagrant@192.168.50.4
```

DONE!!!

## But where are they being stored?

```
C:\Users\as56\VirtualBox VMs\
```


## Now, put multiple hosts into a single file


```
  config.vm.box = "ukcp-test"
  config.vm.synced_folder "C:/vagrant-shares/centos6", "/vagrant-share", type: "nfs"
 
  config.vm.define "test1" do |test1|
    test1.vm.hostname = "test1"
    test1.vm.network "private_network", ip: "192.168.50.4"
  end
  
  config.vm.define "test2" do |test2|
    test2.vm.hostname = "test2"
    test2.vm.network "private_network", ip: "192.168.50.5"
  end  
```

## Host server can access private network addresses

E.g.:

```
python manage.py runserver 0.0.0.0:8000
```

Shows up on host in browser at:

http://192.168.50.50:8000/

## Get X working

```
yum install -y xorg-x11-xauth
```

## Resetting a VM to its base state

Try:

```
vagrant halt
vagrant destroy -f
vagrant up
```

## Limiting the disk size on a VM

In the `Vagrantfile`:

```
    config.disksize.size = "20GB"
```

And install the plugin:

```
$ vagrant plugin install vagrant-disksize
```

Then reload an provision:

```
vagrant reload --provision
```
