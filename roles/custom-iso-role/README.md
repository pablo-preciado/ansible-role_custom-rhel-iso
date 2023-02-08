Role Name
=========

This role takes a original RHEL iso file from Red Hat and automatically creates a custom iso file from that one. The customized iso file can be configured via variables so as soon as your system boots your desired configuration is applied.

Role Variables
--------------

The role need a original Red Hat provided iso file and also a name for the custom iso that will generate. You can define these values with this variables:
        my_original_iso_full_path: ~/rhel9.iso
        my_custom_iso: ~/custom-rhel9.iso

Also, the role will mount the Red Hat original iso file into a directory and will copy all of its content into a working directory so it can then create a new iso file from all the original information. You can define the mount and working directories with the following variables:
        my_working_dir: /tmp/working-dir
        my_mnt_dir: /tmp/mnt-dir
        
Then, there are quite some variables you can use for customizing your iso image. Take a look at the following values:
        #General iso configuration
        my_system_language: en_US
        my_keyboard_layout: es
        my_time_zone: Europe/Paris
        my_root_pass: redhat
        
        #Networking options for iso file
        my_network_bootprotocol: static #Mandatory variable, options are "dhcp" or "static"
        my_hostname: test #Optional
        my_netdevice: eht1 #Optional, only to be used if my_network_bootprotocol is "static"
        my_ip: 192.168.1.23 #Mandatory when my_network_bootprotocol is "static"
        my_netmask: 24 #Optional, only to be used if my_network_bootprotocol is "static"
        my_gateway: 4.5.6.7 #Optional, only to be used if my_network_bootprotocol is "static"
        my_dns_server: 8.8.8.8 #Optional, only to be used if my_network_bootprotocol is "static"
        
        #Optional Red Hat CDN user and password for registering with Insights
        my_rhn_user: ppreciad
        my_rhn_pass: mypassword
        
        #Optional, users to be created. You can add as many users as you want by growing the dictionary with the same structure
        #Every entry on the dictionary must have "name" and "password", optionally you can define additional group for the user (see example below)
        my_users:
          - name: ansible
            password: redhat
            groups: wheel
          - name: preciado
            password: redhat
            groups: wheel
          - name: pablo
            password: redhat

Example Playbook
----------------

The most simple example would be the following:

    - hosts: servers
      become: true
      
      vars
        #Directories and files
        my_original_iso_full_path: ~/rhel9.iso
        my_custom_iso: ~/custom-rhel9.iso
        my_working_dir: /tmp/working-dir
        my_mnt_dir: /tmp/mnt-dir
        
        #General iso configuration
        my_system_language: en_US
        my_keyboard_layout: es
        my_time_zone: Europe/Paris
        my_root_pass: redhat
        
        #Networking options for iso file
        my_network_bootprotocol: static #Mandatory variable, options are "dhcp" or "static"
        my_hostname: test #Optional
        my_netdevice: eht1 #Optional, only to be used if my_network_bootprotocol is "static"
        my_ip: 192.168.1.23 #Mandatory when my_network_bootprotocol is "static"
        my_netmask: 24 #Optional, only to be used if my_network_bootprotocol is "static"
        my_gateway: 4.5.6.7 #Optional, only to be used if my_network_bootprotocol is "static"
        my_dns_server: 8.8.8.8 #Optional, only to be used if my_network_bootprotocol is "static"
        
        #Optional Red Hat CDN user and password for registering with Insights
        my_rhn_user: ppreciad
        my_rhn_pass: mypassword
        
        #Optional, users to be created. You can add as many users as you want by growing the dictionary with the same structure
        #Every entry on the dictionary must have "name" and "password", optionally you can define additional group for the user (see example below)
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


Author Information
------------------

Pablo Preciado.
