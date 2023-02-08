# Ansible role: create RHEL custom iso file

This role has been tested with three different versions of RHEL, this is 8.5, 8.6 and 9. You need to download the original iso file from Red Hat and store it in your target host, remember to define all the mandatory variables, including the full path to the original Red Hat iso file.

You only need to give values to all of the variables (some of these are optional):

```
#Directories and files
my_original_iso_full_path: /ISO-files/rhel-8.5-x86_64-dvd.iso
my_custom_iso: /ISO-files/my_custom.iso
my_working_dir: /tmp/working-dir
my_mnt_dir: /tmp/mnt-dir

#General iso configuration
my_system_language: en_US
my_keyboard_layout: es
my_time_zone: Europe/Paris
my_root_pass: redhat

#Networking options for iso file
my_network_bootprotocol: dhcp #Mandatory variable, options are "dhcp" or "static"
my_hostname: testhostname #Optional
my_netdevice: eht1 #Optional, only to be used if my_network_bootprotocol is "static"
my_ip: 192.168.1.23 #Mandatory when my_network_bootprotocol is "static"
my_netmask: 24 #Optional, only to be used if my_network_bootprotocol is "static"
my_gateway: 4.5.6.7 #Optional, only to be used if my_network_bootprotocol is "static"
my_dns_server: 8.8.8.8 #Optional, only to be used if my_network_bootprotocol is "static"

#Optional Red Hat CDN user and password for registering with Insights
my_rhn_user: myuser
my_rhn_pass: mypassword

#Optional, users to be created. You can add as many users as you want by growing the dictionary with the same structure
#Every entry on the dictionary must have "name" and "password" fields, optionally you can define additional groups for the user with the "groups" field (see example below)
my_users:
  - name: ansible
    password: redhat
    groups: wheel
  - name: preciado
    password: redhat
    groups: wheel
  - name: pablo
    password: redhat
```

Once you know all the values for your variables just write a simple playbook calling the role and defining the variables, such as the one you can find in the "example.yml" file.

```
---
- name: Example playbook for generating new custom iso
  hosts: target1
  become: true
  
  vars:
    #Directories and files
    my_original_iso_full_path: /ISO-files/rhel-8.5-x86_64-dvd.iso
    my_custom_iso: /ISO-files/my_custom.iso
    my_working_dir: /tmp/working-dir
    my_mnt_dir: /tmp/mnt-dir
    
    #General iso configuration
    my_system_language: en_US
    my_keyboard_layout: es
    my_time_zone: Europe/Paris
    my_root_pass: redhat
    
    #Networking options for iso file
    my_network_bootprotocol: dhcp #Mandatory variable, options are "dhcp" or "static"
    my_hostname: testhostname #Optional
    my_netdevice: eht1 #Optional, only to be used if my_network_bootprotocol is "static"
    my_ip: 192.168.1.23 #Mandatory when my_network_bootprotocol is "static"
    my_netmask: 24 #Optional, only to be used if my_network_bootprotocol is "static"
    my_gateway: 4.5.6.7 #Optional, only to be used if my_network_bootprotocol is "static"
    my_dns_server: 8.8.8.8 #Optional, only to be used if my_network_bootprotocol is "static"
    
    #Optional Red Hat CDN user and password for registering with Insights
    my_rhn_user: myuser
    my_rhn_pass: mypassword
    
    #Optional, users to be created. You can add as many users as you want by growing the dictionary with the same structure
    #Every entry on the dictionary must have "name" and "password" fields, optionally you can define additional groups for the user with the "groups" field (see example below)
    my_users:
      - name: ansible
        password: redhat
        groups: wheel
      - name: preciado
        password: redhat
        groups: wheel
      - name: pablo
        password: redhat
  
  roles:
     - custom-iso-role
```


Then run the ansible-navigator command for executing your playbook:

```
ansible-navigator run example.yml -m stdout
```

References:

   [Red Hat kickstart generator](https://access.redhat.com/labs/kickstartconfig/)

   [How to create a modified Red Hat Enterprise Linux ISO with kickstart file or modified installation media?](https://access.redhat.com/solutions/60959)