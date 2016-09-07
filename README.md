Role Name
=========

This ansible role provide the installation and configuration of the controllers of the main services of openstack:

* keystone
* glance
* nova
* neutron
* horizon
* cinder

In addition, it installs and configures the required the software dependencies, and configure the firewall.
It have been tested with Openstack Liberty on Centos 7

Requirements
------------

* The role depends from the role geerlingguy.mysql.
* Disable SELinux (reboot is required).

Role Variables
--------------
The variables are related to the Openstack version, IPs, passwords.
Among all the configurable variable, we have used those for testing: 

    hosts: localhost
    vars:
      openstack_version: "liberty"
      openstack_controller_host: localhost
      openstack_admin_token: "openstack"
      openstack_admin_password: "openstack"
      openstack_database_root: "root"
      openstack_database_root_password: "root"
      openstack_keystone_config_revoke_driver: "sql"
      mysql_root_username: "{{ openstack_database_root }}"
      mysql_root_password: "{{ openstack_database_root_password }}"
      openstack_controller_ip: 10.0.0.11
      openstack_memcached_servers: localhost:11211
      openstack_libvirt_secret_uuid: "secret"


Dependencies
------------
A complete mysql role: geerlingguy.mysql


Example Playbook
----------------

    hosts: localhost
      vars:
        openstack_version: "liberty"
        openstack_controller_host: localhost
        openstack_admin_token: "openstack"
        openstack_admin_password: "openstack"
        openstack_database_root: "root"
        openstack_database_root_password: "root"
        openstack_keystone_config_revoke_driver: "sql"
        mysql_root_username: "{{ openstack_database_root }}"
        mysql_root_password: "{{ openstack_database_root_password }}"
        openstack_controller_ip: 10.0.0.11
        openstack_memcached_servers: localhost:11211
        openstack_libvirt_secret_uuid: "secret"
    
      roles:
        - geerlingguy.mysql
        - ansible-role-os-controller

License
-------

Apache2

