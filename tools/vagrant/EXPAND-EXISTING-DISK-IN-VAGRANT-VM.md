# Expanding a disk on a Vagrant VM (without destroying)

## Firstly, work from outside the VM

```
vagrant halt
vagrant plugin install vagrant-disksize
```

Edit the `Vagrantfile` to set new size, e.g.:

```
  config.disksize.size = "75GB"
```

Restart

```
vagrant up  # which might take a few minutes...
```

## Secondly, on the VM itself

Instructions modified slightly from here:

https://gist.github.com/christopher-hopper/9755310

```
# List disk partitions
fdisk -l
df -h

# Run fdisk and create a new entry
fdisk /dev/sda
```

Interactively do the following to create `/dev/sda4/`:

```
Press p to print the partition table to identify the number of partitions. By default there are two - sda1 and sda2.
Press n to create a new primary partition.
Press p for primary.
Press 4 for the partition number, depending the output of the partition table print.
Press Enter two times to accept the default First and Last cylinder.
Press t to change the system's partition ID
Press 4 to select the newly creation partition
Type 8e to change the Hex Code of the partition for Linux LVM
Press w to write the changes to the partition table.
```

Reboot:

```
reboot
```

Login again and set up disk:

```
ssh root@vagrantbox...

# Create a new physical volume using the new primary partition just created.
pvcreate /dev/sda4

# Find out the name of the Volume Group that the Logical Volume mapping belongs to (ie. VolGroup00).
vgdisplay

# Extend the Volume Group to use the newly created physical volume.
vgextend VolGroup00 /dev/sda4

# Extend the logical volume to use more of the Volume Group size now available to it. You can either 
# tell it to add a set amount of space in Megabytes, Gigabytes or Terabytes, 
# and control the growth of the Disk:
lvextend -L+33G /dev/mapper/VolGroup00-LogVol00

# Resize the file-system to use up the space made available in the Logical Volume:
resize2fs /dev/mapper/VolGroup00-LogVol00

# Verify that there is now more space available
df -h
```
