# test-role-create-custom-iso-file

First step is updating the "hosts" file which you'll use an inventory. Then you need to create yaml file (I recommend to protect the file using ansible-vault) in which you'll store all the needed variable values:

```
ansible-vault create variables.yml
```

Define all the variables in that file. Remember that if not defined the role will use default values, some of them don't have valid default value so those ones need to be specified.

```
#Define RHEL version you're working with (options are "8.5,"8.6" and "9")

rhel_version: "8.5"

#Define root password (it will be encrypted in the ks.cfg file):

rt_pass: "your-root-password"

#Specify where to find the original ISO file (the standard RHEL ISO)

original_iso_full_path: "full-path-to-your-original-iso-file"

#Optionally, define the name and path for your output ISO file:

cust_iso_file: "custom-iso-file.iso"
cust_iso: "~/{{ cust_iso_file }}"

#Optionally, you can define your working directory:

wrk_dir:  /tmp/rhel_cdrom

#Define the configuration of your kickstart file

#Choose "dhcp" or "static", default is "dhcp"
network_bootproto: "dhcp"

#If static, it's mandatory to define ip and netmask
machine_ip: "1.1.1.1"
machine_netmask: "255.255.255.0"

#Other variables you need to define
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