Role os-controller
==================

This ansible role provides the installation and configuration of the main openstack services controllers and
the required software dependencies and firewall. 

Services provided are the following:

* keystone
* glance
* nova
* neutron
* horizon
* cinder

In addition a network node is installed in the same host. The network configuration is based in openvswitch.

It has been tested with Openstack Liberty on Centos 7.

### Related roles
* In case you want to install a compute node in the same host, you can use the role LIP-Computing.nova-compute-node.
* In case you want to link the floating ips to external network, you can use the role LIP-Computing.os-all-in-network.


Requirements
------------

* Disable SELinux (reboot is required).
* Ensure the system has configured swap memory


Role Variables
--------------
The variables are related to the Openstack version, endpoints, IPs, passwords: 
       
    openstack_controller_host: hostname for controller ( for example "controller")
    openstack_controller_ip: controller host ip (127.0.0.1 by default)
    openstack_cidr: controller host CIDR (127.0.0.1/8 by default)
    openstack_memcached_servers: memcached server (localhost:11211 by default)
    
    #Administration endpoints
    openstack_admin_vip: administration IP ("{{ openstack_controller_host }}" by default)
    openstack_internal_vip: internal IP  ("{{ openstack_controller_host }}" by default)
    openstack_public_vip: public IP  ("{{ openstack_controller_host }}" by default)
    openstack_os_url: authentication url ("http://{{ openstack_controller_host }}:35357/v3" by default)
    openstack_os_identity_api_version: identity api version (3 by default)
    openstack_ath_uri: auth_uri, authorization uri ("http://{{ openstack_admin_vip }}:5000" by default)
    openstack_ath_url: auth_url, authorization url ("http://{{ openstack_admin_vip }}:35357" by default)
    openstack_endpoint_admin_url: adminurl ("http://{{ openstack_admin_vip }}:35357/v3" by default)
    openstack_endpoint_internal_url: internalurl "http://{{ openstack_internal_vip }}:5000/v3" by default)
    openstack_endpoint_public_url: publicurl ("http://{{ openstack_public_vip }}:5000/v3" by default)
    openstack_database_host: database host ("{{ openstack_controller_host }}" by default)
    openstack_http_port: http port (80 by default)
    
    # authorization
    openstack_admin_token: administration tocken ("openstack" by default)
    openstack_admin_user: administration user ("admin" by default)
    openstack_admin_project: administration project name ("admin" by default)
    openstack_admin_password: administration password ("openstack" by default)
    openstack_admin_role: administration role name ("admin" by default)
    
    #Rabbitmq variables
    openstack_rabbit_host: rabbit_host ("{{ openstack_controller_host }}" by default)
    openstack_rabbit_user: rabbit_user (openstack by default)
    openstack_rabbit_pass: rabbit password ("{{ openstack_admin_password }}" by default)
    
    # Keystone
    openstack_keystone_db_name: database name ("keystone" by default)
    openstack_keystone_db_user: database user ("keystone" by default)
    openstack_keystone_db_password: dabase password ("{{ openstack_admin_password }}" by default)
    openstack_keystone_config_default_verbose: verbose (False by default)
    openstack_keystone_config_token_provider: token provider ("uuid" by default)
    openstack_keystone_config_token_driver: token driver ("sql" by default)
    openstack_keystone_config_revoke_driver: revoke driver ("sql" by default)
    
    openstack_keystone_auth_region: region_name (RegionOne by default)
    openstack_user_name: demo user name ("demo" by default)
    openstack_user_project: demo project name ("demo" by default)
    openstack_user_password: demo password ("{{ openstack_admin_password }}" by default)
    openstack_user_role: demo role name ("user" by default)
    
    
    # glance
    openstack_glance_keystone_password: glance keystone password ("{{ openstack_admin_password }}" by default)
    openstack_glance_db_name: glance database name ("glance" by default)
    openstack_glance_db_user: glance database user name ("glance" by default)
    openstack_glance_db_password: glance database name ("{{ openstack_admin_password }}" by default)
    openstack_glance_adminurl: glance admin url ("http://{{ openstack_admin_vip }}:9292" by default)
    openstack_glance_internalurl: glance interla url ("http://{{ openstack_internal_vip }}:9292" by default)
    openstack_glance_publicurl: glance public url ("http://{{ openstack_public_vip }}:9292" by default)
    
    # nova controller
    openstack_nova_keystone_password: nova keystone password ("{{ openstack_admin_password }}" by default)
    openstack_nova_db_name: nova database name ("nova" by default)
    openstack_nova_db_user: nova database user name ("nova" by default)
    openstack_nova_db_password: nova database password ("{{ openstack_admin_password }}" by default)
    openstack_nova_adminurl: nova admin url ("http://{{ openstack_admin_vip }}:8774/v2/%\\(tenant_id\\)s" by default)
    openstack_nova_intelnalurl: nova internal url ("http://{{ openstack_internal_vip }}:8774/v2/%\\(tenant_id\\)s" by default)
    openstack_nova_publicurl: nova public url ("http://{{ openstack_public_vip }}:8774/v2/%\\(tenant_id\\)s" by default)
    
    
    # Neutron controller:
    openstack_neutron_keystone_password: neutron keystone password ("{{ openstack_admin_password }}" by default)
    openstack_neutron_db_name: neutron database name ("neutron" by default)
    openstack_neutron_db_user: neutron database user ("neutron" by default)
    openstack_neutron_db_password: neutron dabase password ("{{ openstack_admin_password }}" by default)
    openstack_neutron_adminurl: neutron admin url ("http://{{ openstack_admin_vip }}:9696" by default)
    openstack_neutron_intelnalurl: neutron internal url ("http://{{ openstack_internal_vip }}:9696" by default)
    openstack_neutron_publicurl: neutron public url ("http://{{ openstack_public_vip }}:9696" by default)
    openstack_neutron_metadata_secret: metadata_proxy_shared_secret  ("openstack" by default)
    
    # Cinder:
    openstack_cinder_user_password: cinder keystone password"{{ openstack_admin_password }}" by default)
    openstack_cinder_db_name: cinder database name ("cinder" by default)
    openstack_cinder_db_user: cinder database user name ("cinder" by default)
    openstack_cinder_db_password: cinder database pasword  ("{{ openstack_admin_password }}" by default)
    openstack_cinder_adminurl_v1: cinder admin url ("http://{{ openstack_admin_vip }}:8776/v1/%\\(tenant_id\\)s" by default)
    openstack_cinder_internalurl_v1: cinder internal url ("http://{{ openstack_internal_vip }}:8776/v1/%\\(tenant_id\\)s" by default)
    openstack_cinder_publicurl_v1: "cinder public url (http://{{ openstack_public_vip }}:8776/v1/%\\(tenant_id\\)s" by default)
    openstack_cinder_adminurl_v2: cinder admin url("http://{{ openstack_admin_vip }}:8776/v2/%\\(tenant_id\\)s" by default)
    openstack_cinder_intelnalurl_v2: cinder internal url ("http://{{ openstack_internal_vip }}:8776/v2/%\\(tenant_id\\)s" by default)
    openstack_cinder_publicurl_v2: cinder public url ("http://{{ openstack_public_vip }}:8776/v2/%\\(tenant_id\\)s" by default)
    
    # token auth env, use in the openstack admin commands
    token_auth_env:
      OS_TOKEN: admin token ("{{ openstack_admin_token }}" by default)
      OS_URL: administration  url ("{{ openstack_os_url }}" by default)
      OS_IDENTITY_API_VERSION: identity version ("{{ openstack_os_identity_api_version }}" by default)
      
    # admin auth env: use in the openstack commands
    admin_auth_env:
      OS_PROJECT_DOMAIN_ID: ("default" by default)
      OS_USER_DOMAIN_ID: ("default" by default)
      OS_PROJECT_NAME: (admin by default)
      OS_TENANT_NAME: (admin by default)
      OS_USERNAME: (admin by default)
      OS_PASSWORD: "{{ openstack_admin_password }}" by default)
      OS_AUTH_URL: "{{ openstack_os_url }}" by default)
      OS_IDENTITY_API_VERSION: "{{ openstack_os_identity_api_version }}" by default)
    
    # Networks:
    
    openstack_neutron_bridge_interface: external bridge of openvswitch ("br-ex" by default)
    
    # Create inital networks
    openstack_create_inital_networks: Create public and private initial network True|False (False by default)
    openstack_external_net_name: external network name ("public" by default)
    openstack_external_subnet_name: external subnetwork name ("public" by default)
    openstack_external_subnet_gateway: external subnetwork gateway ("192.168.1.254" by default)
    openstack_external_subnet_cird: external subnetwork gateway ("192.168.1.0/24" by default)
    openstack_external_subnet_pool_start: ip to start the external subnet pool("192.168.1.1" by default)
    openstack_external_subnet_pool_end: ip to end the external subnet pool("192.168.1.128" by default)
    
    
    openstack_demo_net_name: private network name ("demo_net" by default)
    openstack_demo_subnet_name: private subnetwork name ("demo_net" by default)
    openstack_demo_subnet_gateway: private subnet gateway ("192.168.200.254" by default)
    openstack_demo_subnet_cidr: private subnet cidr ("192.168.200.0/24" by default)
    openstack_demo_dns_servers: private dns servers ("'8.8.8.8 8.8.8.4'" by default)
    
    openstack_demo_router: "router1"
    



Dependencies
------------
The role uses a mysql server, we recommend to use:
 * geerlingguy.mysql


Playbook Examples
----------------
### Standard playbook
    # controller_playbook.yml
    - hosts: localhost
      vars:
        openstack_version: "liberty"
        openstack_controller_host: "localhost"
        openstack_admin_token: "admin_token"
        openstack_admin_password: "admin_password"
        openstack_database_host: localhost
        openstack_database_root: "root"
        openstack_database_root_password: "root"
        openstack_keystone_config_revoke_driver: "sql"
        mysql_root_username: "{{ openstack_database_root }}"
        mysql_root_password: "{{ openstack_database_root_password }}"
        openstack_controller_ip: 127.0.0.1
        openstack_cidr: 127.0.0.0/24
        openstack_memcached_servers: localhost:11211
        openstack_libvirt_secret_uuid: "libvirt_secret"
    
      roles:
        - geerlingguy.mysql
        - LIP-Computing.os-controller
        
### All in playbook    
This playbook installs the controller and the compute node in the same host (all-in installation):
    
    # all_in_playbook.yml
    - hosts: localhost
      vars:
        openstack_version: "liberty"
        openstack_all_in: True
        openstack_controller_host: "localhost"
        openstack_admin_token: "admin_token"
        openstack_admin_password: "admin_password"
        openstack_database_host: localhost
        openstack_database_root: "root"
        openstack_database_root_password: "root"
        openstack_keystone_config_revoke_driver: "sql"
        mysql_root_username: "{{ openstack_database_root }}"
        mysql_root_password: "{{ openstack_database_root_password }}"
        openstack_controller_ip: 127.0.0.1
        openstack_cidr: 127.0.0.0/24
        openstack_memcached_servers: localhost:11211
        openstack_libvirt_secret_uuid: "libvirt_secret"
        openstack_compute_node_ip: 127.0.0.1
        openstack_cidr: 127.0.0.0/24
        openstack_neutron_public_interface: "eth0"
        openstack_create_inital_networks: True
    
      roles:
        - geerlingguy.mysql
        - LIP-Computing.os-controller
        - LIP-Computing.nova-compute-node



Execute
------------

The ansible roles are executed by the command:
ansible-playbook <-i host_list> controller_playbook.yml 


License
-------

Apache 2.0


FAQ
------
* Services does not start after rebooting the virtual machine (VM)
 * If you have install the controller in a VM provided by openstack, you have to set the hostname every time that the VM restars.
 * Rabbit depends of the hostname. If the hostname changes, rabbit needs to be reinstalled.


Author Information
------------------

Contact: jorgesece@lip.pt
