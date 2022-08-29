#Ansible role: create RHEL custom iso file

This role has been tested with three different versions of RHEL, this is 8.5, 8.6 and 9. You need to download the original iso file from Red Hat and store it in your target host, remember to define all the mandatory variables, including the full path to the original Red Hat iso file.

First step is updating the "hosts" file which you'll use an inventory. Then you need to create yaml file (I recommend to protect the file using ansible-vault) in which you'll store all the needed variable values:

```
ansible-vault create variables.yml
```

Define all the variables in that file. Remember that if not defined the role will use default values, some of them don't have valid default value so those ones are mandatory.

```
###########################
# General variables
###########################

#Mandatory: Specify where to find the original ISO file (the standard RHEL ISO)

original_iso_full_path: "full-path-to-your-original-iso-file"

#Optional: define the name and path for your output ISO file:

#cust_iso_file: "custom-iso-file.iso"
#cust_iso: "~/{{ cust_iso_file }}"

#Optional: define your working directory:

#wrk_dir:  /tmp/rhel_cdrom

###########################
# Kickstart file variables
###########################

#Mandatory: define root password (it will be encrypted in the ks.cfg file):

rt_pass: "your-root-password"

#Mandatory: choose "dhcp" or "static", default is "dhcp"

network_bootproto: "dhcp"

#Mandatory if static: define ip and netmask

#machine_ip: "10.10.10.10"
#machine_netmask: "255.255.255.0"

#Optional if static: define DNS server and/or gateway

#dns_server: 8.8.8.8
#net_gateway: 10.10.10.1

#Optional: define a hostname

#machine_hostname: test.local

#Optional: you can create another user

#additional_user_name: ansible
#additional_user_pass: redhat

#Optional: add the additional user to a group different of the default one (use wheel for sudoer)

#additional_user_group: wheel

#Optional: give Red Hat network username and password

#rhn_user: "your-rh-cdn-username"
#rhn_pass: "your-rh-cdn-password"

#Mandatory: other variables you need to define

keyboard_layout: "es"
system_language: "en_US.UTF-8"
time_zone: "Europe/Madrid"
```

Prepare your environment so it's able to run ansible-navigator:

```
podman login registry.redhat.io
```

Then run the ansible-navigator command for executing the playbook using the variables on the vault file:

```
ansible-navigator run -m stdout --pae false test-role.yml -e @variables.yml -i ./hosts --ask-vault-pass
```
