# samba4-ad-ubuntu

Adapted for Ubuntu 20.04 from https://www.tecmint.com/install-samba4-active-directory-ubuntu/

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
/dev/disk/by-uuid/<SOME_UUID> / ext4 defaults,==acl== 0 1
/swap.img	none	swap	sw	0	0
```
3. Finally setup your machine hostname with a descriptive name, such as adc1 used in this example, by editing /etc/hostname file or by issuing.
```
sudo hostnamectl set-hostname dc1
```
## Step 2: Install Required Packages for Samba4 AD DC
4. In order to transform your server into an Active Directory Domain Controller, install Samba and all the required packages on your machine by issuing the below command with root privileges in a console.
```
apt-get install acl attr samba samba-dsdb-modules samba-vfs-modules winbind libpam-winbind libnss-winbind libpam-krb5 krb5-config krb5-user dnsutils ntp ntpdate
```
5. While the installation is running a series of questions will be asked by the installer in order to configure the domain controller.

On the first screen you will need to add a name for Kerberos default REALM in uppercase. Enter the name you will be using for your domain in uppercase and hit Enter to continue..

6. Next, enter the hostname of Kerberos server for your domain. Use the same name as for your domain, with lowercases this time and hit Enter to continue.

## Step 3: Provision Samba AD DC for Your Domain
8. Before starting to configure Samba for your domain, first run the below commands in order to stop and disable all samba daemons.
```
sudo systemctl stop samba-ad-dc.service smbd.service nmbd.service winbind.service
sudo systemctl disable samba-ad-dc.service smbd.service nmbd.service winbind.service
```
9. Next, rename or remove samba original configuration. This step is absolutely required before provisioning Samba AD because at the provision time Samba will create a new configuration file from scratch and will throw up some errors in case it finds an old smb.conf file.

```
sudo mv /etc/samba/smb.conf /etc/samba/smb.conf.initial
```
10. Now, start the domain provisioning interactively by issuing the below command with root privileges and accept the default options that Samba provides you.

Also, make sure you supply the IP address for a DNS forwarder at your premises (or external) and choose a strong password for Administrator account. If you choose a week password for Administrator account the domain provision will fail.

```
sudo samba-tool domain provision --use-rfc2307 --interactive
```
