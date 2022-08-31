# Xen Project

###Creating a VM###

xen-create-image is a xen-tools program for provisioning a virtual machine.
It handles everything from creating the disks, applying the base filesystem, installing necessary packages and configuring networking.
**We deploy Ubuntu 14.04** images by default from debian debootstrap via xen-tools.

* Hostname and IP are important, see [Hostlist](https://github.com/ACMLug/hostlist/blob/master/hostlist.txt).
* Keep disk size under 40g unless required.
* Begin with 2 vcpus, see if you need more later by editing `/etc/xen/${hostname}.cfg`

We typically create a new VM with this command and these options
```
xen-create-image --hostname=test --size=20G --memory=1gb --vcpus=2 --ip=192.17.239.35
```

The command will generate the a config file for the VM which can be edited with new settings applied on its boot:
```
/etc/xen/${hostname}.cfg
```

An example of more detailed options:
```
xen-create-image --hostname=test --size=20G --memory=1g --vcpus=2 --lvm=XenVG --fs=ext4 --bridge=xenbr0 --ip=192.17.239.35 --netmask=255.255.255.128 --gateway=192.17.239.1 --pygrub --dist=trusty --role=repo,packages,nagios,puppet
```

###Start the VM###

Once the VM is created we must start it.
```
xl create /etc/xen/test.cfg
```

Start VM with debug mode and attach to console. Good for troubleshooting
```
xl create /etc/xen/<config>.cfg -d -c
```

List machines
```
xl vm-list
```

To stop the VM use
```
xl destroy test
```

###Delete VM###

First stop the VM and then remove it the images
```
xl destroy test-guest
xen-delete-image test-guest --lvm XenVG
```

###Attach to Console###

**Note:** Exit the console with `Ctrl+]`

To attach to the console of a VM use the console command from the Xen Project host.
If you do not see the login screen then press enter and it will refresh the screen.
```
xl console test
<Enter>
```

The root password of the new guest is automatically generated via xen-tools scripts and is stored in VMs xen-tools log file.
```
fgrep Pass /var/log/xen-tools/test.log
Root Password   :  uBRPkj7V88l8szX
```

###Manual LVM###

####Grow VM Disk####

From the host, grow the disk LVM of the VM by 40GB.
Shutdown the guest first after finding the name of the disk.
```
lvs | grep test
test-disk  XenVG -wi-ao---  40.00g
test-swap  XenVG -wi-ao--- 128.00m
xl destroy test
lvextend -L+40G /dev/XenVG/test-disk
```

Start the VM and resize the partition in the VM:
```
xl create /etc/xen/test.cfg
root@test:~# resize2fs /dev/xvda2
resize2fs 1.42.9 (4-Feb-2014)
Filesystem at /dev/xvda2 is mounted on /; on-line resizing required
old_desc_blocks = 3, new_desc_blocks = 5
The filesystem on /dev/xvda2 is now 20971520 blocks long.

root@test:~# df -h | grep xvda
/dev/xvda2       79G  3.1G   72G   5% /
```

####Manual LVM Creation####

Manually create disk and filesystem for new VM.
```
lvcreate -L 4G -n test-guest XenVG
lvdisplay
mkfs.ext4 /dev/XenVG/test-guest
mkdir -p /xen/test-guest
mount /dev/XenVG/test-guest /xen/test-guest
```
