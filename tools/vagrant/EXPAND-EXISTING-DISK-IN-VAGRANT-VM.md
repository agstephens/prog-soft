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
Run this:

```
cfdisk /dev/sda
```

Which opens up this editor for the partitions:
![[Pasted image 20240718101134.png]]

Navigate to the new one, and select `Delete`. It should then list free space.

Navigate to the main partition (in this case `/dev/sda5`) and select `Resize` and put in the new size.

Finally, before quitting, select `Write`, then `Quit`.

Now run this command to *grow the disk to its allotted size:

```
xfs_growfs /dev/sda5
```

And now `df -H` should show you it has grown.
