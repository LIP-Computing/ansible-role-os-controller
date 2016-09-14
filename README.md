Role os-controller
==================

This ansible role provide the installation and configuration of the controllers of the main services of openstack:

* keystone
* glance
* nova
* neutron
* horizon
* cinder

The role install in the same host the network controller and network node with local driver configured. It uses a 

On the other hand, it installs and configures the required the software dependencies, and configure the firewall.
It have been tested with Openstack Liberty on Centos 7

Requirements
------------

* The role depends from the role geerlingguy.mysql.
* Disable SELinux (reboot is required).
* Ensure the system has configured swap memory


Role Variables
--------------
The variables are related to the Openstack version, endpoints, IPs, passwords: 

    openstack_hard_nofile_limits: 102400
    
    openstack_controller_ip: 127.0.0.1
    openstack_cidr: 127.0.0.0/8
    openstack_memcached_servers: localhost:11211
    
    #Administration endpoints
    openstack_admin_vip: "{{ openstack_controller_host }}"
    openstack_internal_vip: "{{ openstack_controller_host }}"
    openstack_public_vip: "{{ openstack_controller_host }}"
    openstack_os_url: "http://{{ openstack_controller_host }}:35357/v3"
    openstack_os_identity_api_version: "3"
    openstack_ath_uri: "http://{{ openstack_admin_vip }}:5000"
    openstack_ath_url: "http://{{ openstack_admin_vip }}:35357"
    openstack_endpoint_admin_url: "http://{{ openstack_admin_vip }}:35357/v3"
    openstack_endpoint_internal_url: "http://{{ openstack_internal_vip }}:5000/v3"
    openstack_endpoint_public_url: "http://{{ openstack_public_vip }}:5000/v3"
    openstack_database_host: "{{ openstack_controller_host }}"
    openstack_http_port: 80
    
    # authorization
    openstack_admin_token: "openstack"
    openstack_admin_user: "admin"
    openstack_admin_project: "admin"
    openstack_admin_password: "openstack"
    openstack_admin_role: "admin"
    
    #Rabbitmq variables
    openstack_rabbit_host: "{{ openstack_controller_host }}"
    openstack_rabbit_user: openstack
    openstack_rabbit_pass: 12345
    
    # Keystone
    openstack_keystone_db_name: "keystone"
    openstack_keystone_db_user: "keystone"
    openstack_keystone_db_password: "keystone"
    openstack_keystone_config_default_verbose: False
    openstack_keystone_config_token_provider: "uuid"
    openstack_keystone_config_token_driver: "sql"
    openstack_keystone_config_revoke_driver: "sql"
    
    openstack_keystone_auth_region: RegionOne
    openstack_user_name: "demo"
    openstack_user_project: "demo"
    openstack_user_password: "userpass"
    openstack_user_role: "user"
    
    
    # glance
    openstack_glance_keystone_password: "openstack"
    openstack_glance_db_name: "glance"
    openstack_glance_db_user: "glance"
    openstack_glance_db_password: "glance"
    openstack_glance_adminurl: "http://{{ openstack_admin_vip }}:9292"
    openstack_glance_internalurl: "http://{{ openstack_internal_vip }}:9292"
    openstack_glance_publicurl: "http://{{ openstack_public_vip }}:9292"
    
    # nova controller
    openstack_nova_keystone_password: "openstack"
    openstack_nova_db_name: "nova"
    openstack_nova_db_user: "nova"
    openstack_nova_db_password: "nova"
    openstack_nova_adminurl: "http://{{ openstack_admin_vip }}:8774/v2/%\\(tenant_id\\)s"
    openstack_nova_intelnalurl: "http://{{ openstack_internal_vip }}:8774/v2/%\\(tenant_id\\)s"
    openstack_nova_publicurl: "http://{{ openstack_public_vip }}:8774/v2/%\\(tenant_id\\)s"
    
    
    # Neutron controller:
    openstack_neutron_keystone_password: "openstack"
    openstack_neutron_db_name: "neutron"
    openstack_neutron_db_user: "neutron"
    openstack_neutron_db_password: "neutron"
    openstack_neutron_adminurl: "http://{{ openstack_admin_vip }}:9696"
    openstack_neutron_intelnalurl: "http://{{ openstack_internal_vip }}:9696"
    openstack_neutron_publicurl: "http://{{ openstack_public_vip }}:9696"
    openstack_neutron_metadata_secret: "openstack"
    
    # Cinder:
    openstack_cinder_user_password: "openstack"
    openstack_cinder_db_name: "cinder"
    openstack_cinder_db_user: "cinder"
    openstack_cinder_db_password: "cinder"
    openstack_cinder_adminurl_v1: "http://{{ openstack_admin_vip }}:8776/v1/%\\(tenant_id\\)s"
    openstack_cinder_internalurl_v1: "http://{{ openstack_internal_vip }}:8776/v1/%\\(tenant_id\\)s"
    openstack_cinder_publicurl_v1: "http://{{ openstack_public_vip }}:8776/v1/%\\(tenant_id\\)s"
    openstack_cinder_adminurl_v2: "http://{{ openstack_admin_vip }}:8776/v2/%\\(tenant_id\\)s"
    openstack_cinder_intelnalurl_v2: "http://{{ openstack_internal_vip }}:8776/v2/%\\(tenant_id\\)s"
    openstack_cinder_publicurl_v2: "http://{{ openstack_public_vip }}:8776/v2/%\\(tenant_id\\)s"
    
    ## auth env
    token_auth_env:
      OS_TOKEN: "{{ openstack_admin_token }}"
      OS_URL: "{{ openstack_os_url }}"
      OS_IDENTITY_API_VERSION: "{{ openstack_os_identity_api_version }}"
    
    admin_auth_env:
      OS_PROJECT_DOMAIN_ID: default
      OS_USER_DOMAIN_ID: default
      OS_PROJECT_NAME: admin
      OS_TENANT_NAME: admin
      OS_USERNAME: admin
      OS_PASSWORD: "{{ openstack_admin_password }}"
      OS_AUTH_URL: "{{ openstack_os_url }}"
      OS_IDENTITY_API_VERSION: "{{ openstack_os_identity_api_version }}"
    
    openstack_neutron_bridge_interface: "br-ex"



Dependencies
------------
The role uses a mysql server, we recommend to use:
 * geerlingguy.mysql


Example Playbook
----------------

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

License
-------

Apache 2.0

