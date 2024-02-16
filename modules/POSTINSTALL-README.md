# secureblue

After rebasing to secureblue, the following steps are recommended.

## Set a GRUB password

Setting a GRUB password helps protect the device from physical tampering and mitigates various attack vectors, such as booting from malicious media devices and changing boot or kernel parameters.

To set a GRUB password, use the following command. By default, the password will be required when modifying boot entries, but not when booting existing entries.

```sudo grub2-setpassword```

GRUB will prompt for a username and password. The default username is root.

If you wish to password-protect booting existing entries, you can add the `grub_users root` entry in the specific configuration file located in the `/boot/loader/entries` directory.

## Create a separate wheel account for admin purposes

Creating a dedicated wheel user and removing wheel from your primary user helps prevent certain attack vectors:

https://www.kicksecure.com/wiki/Dev/Strong_Linux_User_Account_Isolation#LD_PRELOAD
https://www.kicksecure.com/wiki/Root#Prevent_Malware_from_Sniffing_the_Root_Password

1. ```adduser admin```
2. ```usermod -aG wheel admin```
3. ```gpasswd -d {your username here} wheel```
4. reboot
