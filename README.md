Ansible role votum.magento2
===========================

Ansible role to install Magento2 e-commerce system.

Requirements
------------

This role depends on the Magerun2 cli tool being installed on the remote server. See https://github.com/netz98/n98-magerun2 for install instructions.

Role Variables
--------------

Available variables with their default values are listed below. (see also `defaults/main.yml`)

    magento2_instance_name: "magento2"

The instance name is used as a label mainly for the cronjobs. Usefull in case u want to install more than one magento instance on the same server with this role.

    magento2_version_to_install: "magento-ce-2.0.7"

Magento version string of the version to install. Version strings are taken from the magerun2 install command.
Possible values are:

 * _magento-ce-2.1.0_
 * _magento-ce-2.0.7_
 * _magento-ce-2.0.6_
 * _magento-ce-2.0.5_
 * _magento-ce-2.0.4_
 * _magento-ce-2.0.2_
 * _magento-ce-2.0.1_
 * _magento-ce-2.0.0_
 * _..._
 * _(you should've got the idea)_
 
    magento2_install_path: "/var/www"

Installation path of the Magento root. *Note:* This is not the web root of the _vhost_. The _vhost_ should point to `{{magento2_install_path}}/pub`.

    magento2_magerun_bin_path: "/usr/local/bin/n98-magerun2.phar"

Path to the Magerun2 binary. The install process rests upon the Magerun2 CLI tool. See http://magerun.net/tag/n98-magerun2/ and https://github.com/netz98/n98-magerun2 for more information and ways to install it.

    magento2_install_sample_data: true

Whether to install sample data or not. Default is true.

    magento2_enable_crons: true
    
Wheter to activate the Magento2 cron jobs. Default is true.

    magento2_auth_public_key: "xxxxxxxxxxxxxxxxxxxxxx"
    magento2_auth_private_key: "xxxxxxxxxxxxxxxxxxxxxx"

To install Magento2 via composer you need a developers account with Magento. Put your public and private key here to enable unattended installation via your credentials. See http://devdocs.magento.com/guides/v2.0/install-gde/prereq/connect-auth.html for further information about Magento2 authentication keys.  

    magento2_db_host: "127.0.0.1"
    magento2_db_name: "magento2"
    magento2_db_user: "root"
    magento2_db_password: ""
    magento2_db_prefix: ""

Set Magento2's database config with this variables. Pretty self explanatory. *Note:* If your database runs on a different port you kann pass it to the `magento2_db_host` variable using the `127.0.0.1:3306` notation.

    magento2_language: "en_US"
    magento2_currency: "USD"
    magento2_timezone: "Europe/Berlin"

Additional Magento2 default install params for language, currency and timezone. For possible values see `./bin/magento info:language:list`, `./bin/magento info:currency:list` and `./bin/magento info:timezone:list`.

    magento2_admin_firstname: "John"
    magento2_admin_lastname: "Doe"
    magento2_admin_email: "mageadmin@example.com"
    magento2_admin_user: "admin"
    magento2_admin_password: "admin123"

Admin login details.

    magento2_backend_frontname: "admin"

URL path to admin backend.

    magento2_base_url: "{{ '{{base_url}}' }}"
    magento2_base_url_secure: ""

The base urls (secure for HTTPS protected areas like customer account and checkout). *Note:* URLs have to be provided including protocolls and trailing slashes. Defaults to {{URL}} which should read the URL from `vhost` config. As of now this is not always working as expected.

    magento2_use_rewrites: "1"

Use web server rewrites for generated links in the storefront and Admin.

    magento2_use_secure: "1"

Use secure URLs. Enable this option only if SSL is available. 

    magento2_use_secure_admin: "1"

Use SSL to access the Magento Admin. Make sure your web server supports SSL before you select this option.

    magento2_use_security_key: "1"
    
Whether to use a "security key" feature in Magento Admin URLs and forms.

    magento2_session_save: "files"

Session save handler (default: "files").

    magento2_cleanup_database: "1"
    
Cleanup the database before installation

    magento2_key: ""

If you have one, specify a key to encrypt sensitive data in the Magento2 database. If you don't have one, leave it empty and Magento2 generates one for you.

    magento2_sales_order_increment_prefix: ""

Specify a string value to use as a prefix for sales orders. Typically, this is used to guarantee unique order numbers for payment processors.


Dependencies
------------

None.

Example Playbook
----------------

    ---
    - name: setup demo installation magento-ce-2.0
      hosts: app
    
      vars_files:
        - group_vars/main.yml
        - group_vars/magento2-ce20-demo.yml
    
      pre_tasks: []
    
      roles:
        - { role: votum.magerun2 }
        - { role: votum.magento2, ansible_become: yes, ansible_become_user: www-data }
    
      post_tasks: []

License
-------

MIT

Author Information
------------------

Copyright VOTUM GmbH (info@votum.de)
