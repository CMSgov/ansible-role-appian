# Ansible Role: Appian

Helps with Appian installation on RHEL/CentOS servers. Any Highly available (HA) and/or Distributed Appian architecture is supported.
This role can be used against any appropriately networked cloud or on-premise infrastructure.

## Requirements

This role uses the [geerlingguy.apache role](https://galaxy.ansible.com/geerlingguy/apache) and requires it to be installed. See the molecule test playbooks for an example of a requirements file, such as the one located within [molecule/default](molecule/default).

You must obtain and provide your own appian installation file (.bin file) and appian license files (two .lic files). This role can only help apply the valid, temporary license files, obtained by you, for new server setup. Once applied, you must follow Appian instructions to request and install permanent license files in your environment.

## Role Variables

Available variables are listed below, along with default values (see [defaults/main.yml](defaults/main.yml)):

    appian_user: appian

... the user that will be created to run Appian services.

    appian_user_home: /home/appian

... the home directory for the Appian user.

    appian_user_nofile_limit: 100000

... hard and soft ulimit for the number of file descriptors available to appian user. Appian's recommendation is 100000.

    appian_host_connection_variable: ansible_hostname

... points to a variable containing connection information, and controls how appian hosts address each other. By default, this assumes appian hosts should communicate with each other over the ansible_hostname as discovered by ansible.

    appian_java_home: "{{ appian_home }}/java"

... java home for appian to use

    appian_installation_dir: /opt/appian

... the parent folder to hold the appian installation, its repo directory, and its backup directory.

    appian_home:  "{{  appian_installation_dir }}/appian"

... the appian home directory

    appian_backup_home: "{{  appian_installation_dir }}/backup"

... the appian backup directory

    appian_repo_home: "{{  appian_installation_dir }}/repo"

... the appian configuration repo Directory

    appian_bin_source: setupLinux64_appian-20.1.47.0.bin

... file name of the appian installation bin on the appian controller; ansible [search path rules](https://docs.ansible.com/ansible/latest/user_guide/playbook_pathing.html) apply.

    appian_env_name: dev

... the name of the appian environment to configure and deploy

    appian_smtp_host: aspmx.l.google.com

... an smtp endpoint to serve outbound mail

    appian_max_exec_engine_load_metric: 120

... This metric limits the amount of memory that a single execution engine will use. On production systems, raise this setting to a value of 120 or higher.

    appian_app_server_min_heap_size: 2048

... The inital heap size in megabytes for the app server. This default value represents Appian's default value.

    appian_app_server_max_heap_size: 4096

... The inital heap size in megabytes for the app server. This default value represents Appian's default value.

    appian_rdbms_drivers:
      mysql: https://repo1.maven.org/maven2/mysql/mysql-connector-java/5.1.48/mysql-connector-java-5.1.48.jar
      oracle: ojdbc7.jar

... a dictionary containing locations of rdbms drivers. These values can be either remote locations (urls) or local paths to the driver(s) on the ansible controller. If using a local file path, ansible [search path rules](https://docs.ansible.com/ansible/latest/user_guide/playbook_pathing.html) apply. Note: only the driver applicable to your installation's given rdbms is used. So, if you are using Oracle and want to use a different download url, you may simply override the Oracle value and leave the MySQL value default as is. Alternatively, if you're going to use MySQL, you need not worry about this default value for oracle, as it won't be used.

    appian_system_data_store_jndi: jdbc/AppianDS

... the JNDI data store name to assign to the system data store

    appian_shared_folder_location: ""

... the location to hold Appian shared folders and files. For HA/D installations, this should be an NFS mount point.

    appian_shared_folders:
      - < see defaults/main.yml for full list >

... a list of folders that must be shared across appian instances in an HA installation. It is very unlikely you should ever change this.

    appian_efs_id

... optional id of an efs to mount. It set, the efs with this id will be mounted to the appian_shared_folder_location.

    appian_k3_license: k3.lic

... file name of the k3.lic file on the appian controller; ansible [search path rules](https://docs.ansible.com/ansible/latest/user_guide/playbook_pathing.html) apply.

    appian_k4_license: k4.lic

... file name of the k4.lic file on the appian controller; ansible [search path rules](https://docs.ansible.com/ansible/latest/user_guide/playbook_pathing.html) apply.

    appian_sync_static_files_to_apache: true

... set to false to skip syncing appian static content from the appian leader to the apache web servers. This exists as a convenience for local docker workflows that can't support rsync. This should likely be left set to true.

    appian_engines:
      - < see defaults/main.yml for full list of defaults >

... a list of engines to run on a given appian engine instance. If an appian engines instance does not have an appian_engines host variable set, the default list will be applied.

    appian_db_engine: '{{ hostvars[groups.appiands[0]]["engine"] }}'

... the engine type of the rdbms. Valid values are mysql and oracle. The default value is pulled from host variables, but any value may be specified.

    appian_db_address: '{{ hostvars[groups.appiands[0]]["endpoint"]["address"] }}'

... the endpoint (ip or fqdn) of the rdbms. This should not include port. The default value is pulled from host variables, but any value may be specified.

    appian_db_port: '{{ hostvars[groups.appiands[0]]["endpoint"]["port"] }}'

... the rdbms' listener port. The default value is pulled from host variables, but any value may be specified.

    appian_db_name: '{{ hostvars[groups.appiands[0]]["db_name"] }}'

... the database on the rdbms to use as appian system data store. The default value is pulled from host variables, but any value may be specified.

    appian_db_username: '{{ hostvars[groups.appiands[0]]["master_username"] }}'

... the username to use in the rdbms connection. The default value is pulled from host variables, but any value may be specified.

    appian_db_password: "password"

... the password to use in the rdbms connection. The default will almost certainly need to be overridden.

    appian_server_url: localhost

... the url at which appian can be reached. This should include the port if a non-standard port (for your scheme) is used. If your scheme is https and your port is 443, or if your scheme is http and your port is 80, you don't need to list a port here.

    appian_server_scheme: http

... the protocol over which Appian is reached. Valid choices are http or https.

    appian_system_manager_password: "password"

... a password for the appian system manager. This is used when starting and stopping the engines service.

    appian_admin_username: "admin"
    appian_admin_first_name: "John"
    appian_admin_last_name: "Smith"
    appian_admin_email: "admin@example.com"
    appian_admin_temporary_password: "badPasswordsOnly"

... information for the user created during initial installation and startup.

    appian_local_plugins_list: {}

... An optional list of local paths to plugin(s) to be installed.

    appian_remote_plugins_list: {}

... An optional list of download urls to plugin(s) to be installed.

## Dependencies

None.

## Example Playbook

    - hosts: appian_instances
      vars:
        appian_server_url: myappian.example.com
        appian_db_password: dbPW123
      roles:
        - role: ansible-role-appian

## Example Inventory

HA and Distributed Appian installations require complex installation, start up, and shut down procedures. As such, this role's default (main.yml) execution order is orchestrated by host group membership in inventory. An example of a static inventory to install Appian on a single host.

    all:
      hosts:
          appian:
              ansible_host: 10.x.x.x
              ansible_user: ec2-user
              ansible_ssh_pivate_key_file: /path/to/my/key.pem
              private_dns_name: localhost
      children:
          appian_instances: # List of all hosts
              hosts:
                  appian
          appian_leader: # A single appian backend host; some non-predictable config files that are required to be the same across a distributed appian installationare pulled from here
              hosts:
                  appian
          appian_engines_instances: # List of engine instances
              hosts:
                  appian
          appian_internal_messaging_instances: # List of Kafka and Zookeeper hosts
              hosts:
                  appian
          appian_data_server_instances: # List of data server hosts
              hosts:
                  appian
          appian_search_server_instances: # List of search server hosts
              hosts:
                  appian
          appian_app_server_instances: # List of Tomcat hosts
              hosts:
                  appian
          appian_web_server_instances: # List of Apache web server hosts
              hosts:
                  appian

A HA or distributed installation is affected by adding hosts and adding them to the desired groups.  
Dynamic inventory may be used (and is encouraged); an example of discovering and organizing appian hosts with ansible's aws_ec2 and aws_rds inventory plugins is shown below. In the below example, tags on EC2 and RDS instances are used extensively to alert ansible to the desired topology.

    ---
    plugin: aws_ec2
    regions:
      - us-east-1
    filters:
      # Discover all EC2s with this tag key/value pair
      # All EC2s that are to be in the appian environment need this tag.
      tag:appian_environment: "ansible-role-appian-ha"
    strict: no
    hostnames: # This is a hierarchy that decides how ansible should address the machines.
      - tag:appian_hostname
      - tag:Name
      - private-ip-address
    groups:
      # Discover all EC2s involved in the appian environment.  All EC2s need this tag, appian_instance:yes.
      appian_instances: "'yes' in (tags.appian_instance)" # Tag should exist on all EC2s in the environment
      # Discover the appian leader with the appian_leader:yes tag.  Exactly one EC2 should be tagged as the leader.
      appian_leader: "'yes' in (tags.appian_leader)"
      # Discover EC2s that will run engines by the appian_engines_instance:yes tag.
      appian_engines_instances: "'yes' in (tags.appian_engines_instance)"
      # Discover EC2s for the messaging service by the appian_internal_messaging_instance:yes tag.
      appian_internal_messaging_instances: "'yes' in (tags.appian_internal_messaging_instance)"
      # Discover EC2s for the data server service by the appian_data_server_instance:yes tag.
      appian_data_server_instances: "'yes' in (tags.appian_data_server_instance)"
      # Discover EC2s for the search service by the appian_search_server_instance:yes tag.
      appian_search_server_instances: "'yes' in (tags.appian_search_server_instance)"
      # Discover EC2s for tomcat with the appian_app_server_instance:yes tag
      appian_app_server_instances: "'yes' in (tags.appian_app_server_instance)"
      # Discover EC2s to run apache with the appian_web_server_instance:yes tag.
      appian_web_server_instances: "'yes' in (tags.appian_web_server_instance)"
    compose:
      # This sets how the ansible controller connects to the machines over ssh.
      ansible_host: public_ip_address

    ---
    plugin: aws_rds
    regions:
      - us-east-1
    groups:
      # Discover the RDS to serve the 'ansible-role-appian-ha' envirnment by tag.
      appiands: "'ansible-role-appian-ha' in (tags.appian_environment)"

## License

[![License](https://img.shields.io/badge/License-CC0--1.0--Universal-blue.svg)](https://creativecommons.org/publicdomain/zero/1.0/legalcode)

See [LICENSE](LICENSE.md) for full details.

```text
As a work of the United States Government, this project is
in the public domain within the United States.

Additionally, we waive copyright and related rights in the
work worldwide through the CC0 1.0 Universal public domain dedication.
```
