Role Name
=========

Installs mariadb mysql https://mariadb.org/
######Configurable options (clustering and cacti monitoring ready)

Requirements
------------

You should create a minimum of 3 servers to be part of the cluster. This will ensure you do not experience a split brain cluster.

Role Variables
--------------

````
cacti_db_password: cactiuser  #defines the password for cacti db monitoring...define here or globally in group_vars/all/accounts
cacti_db_user: cactiuser  #defines the user for cacti db monitoring...define here or globally in group_vars/all/accounts
deb_db_password: []  #defines debian db password...generate using echo password | mkpasswd -s -m sha-512 ...define here or globally in group_vars/all/accounts
debian_mariadb_repository: 'deb http://ftp.utexas.edu/mariadb/repo/10.0/{{ ansible_distribution|lower() }} {{ ansible_distribution_release }} main'
email_notifications: 'notifications@{{ smtp_domain_name }}'  #defines email address to receive notifications...define here or in group_vars/group
enable_cacti_monitoring: false  #defines if cacti monitoring should be enabled for mysql
enable_galera_monitoring_script: false
galera_cluster_name: [] # Define the name of the cluster...define here or in group_vars/group
galera_cluster_nodes: [] # Define the IP addresses of the nodes which will be part of the cluster...define here or in group_vars/group
galera_monitor_script_name: galeranotify.py
galera_monitor_script_path: /etc/mysql
mysql_master: false  #defines a node in the cluster as master...define as true for one host in host_vars/host
mysql_root_password: [] #defines mysql root password...generate using echo password | mkpasswd -s -m sha-512 ...define here or globally in group_vars/all/accounts
notify_mail_from: 'galeranotify@{{ smtp_domain_name }}'  #defines email address that cluster notifications will be sent from
notify_mail_to: '{{ email_notifications }}'
notify_smtp_server: '{{ smtp_server }}'  #defines smtp server to send notifications through
reconfigure_galera: false  #defines if the cluster should be reconfigured
smtp_domain_name: '{{ pri_domain_name }}' #defines smtp domain for email...define here or globally in group_vars/all/email
smtp_server: 'smtp.{{ pri_domain_name }}'  #defines smtp server to send email through...define here or globally in group_vars/all/servers
````

Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: mrlesmithjr.mariab-galera-cluster }

License
-------

BSD

Author Information
------------------

Larry Smith Jr.
- @mrlesmithjr
- http://everythingshouldbevirtual.com
- mrlesmithjr [at] gmail.com
