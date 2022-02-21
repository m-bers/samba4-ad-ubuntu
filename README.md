# samba4-ad-ubuntu

Adapted from https://www.tecmint.com/install-samba4-active-directory-ubuntu/

## Step 1: Initial Configuration for Samba4

1. Before proceeding your Samba4 AD DC installation first let’s run a few pre-required steps. First make sure the system is up to date with the last security features, kernels and packages by issuing the below command:
```
sudo apt-get update 
sudo apt-get upgrade
sudo apt-get dist-upgrade
```
2. Next, open machine /etc/fstab file and assure that your partitions file system has ACLs enabled as illustrated below.

Usually, common modern Linux file systems such as ext3, ext4, xfs or btrfs support and have ACLs enabled by default. If that’s not the case with your file system just open /etc/fstab file for editing and add acl string at the end of third column and reboot the machine in order to apply changes.
```
# /etc/fstab: static file system information.
#
# Use 'blkid' to print the universally unique identifier for a
# device; this may be used with UUID= as a more robust way to name devices
# that works even if disks are added and removed. See fstab(5).
#
# <file system> <mount point>   <type>  <options>       <dump>  <pass>
# / was on /dev/vda2 during curtin installation
/dev/disk/by-uuid/16b85517-5ca6-410f-b091-6465899af51b / ext4 defaults,==acl== 0 1
/swap.img	none	swap	sw	0	0
```
