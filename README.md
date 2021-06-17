JWS Playbook
====
This repository contains the ansible playbook to setup JWS.

Roles
====
There are two roles in this playbook:
- The tomcat role which:
    - Installs JAVA
    - Installs basic packages required
    - Adds tomcat user and group
    - Downloads tomcat and install
    - Makes sure that tomcat belongs to the tomcat user and group
    - Deploys server.xml, web.xml and context.xml
- The common_criteria_tomcat role which:
    - Installs XML-XPath
    - Checks that root directories are appropriately defined
    - Checks that file's owner, group and mode are in compliance
    - Checks that the server's configuration is in compliance with the tomcat common criteria

How to use this playbook
====
- Update your inventory, e.g.:
    ```
        [tomcat]
        192.168.0.1      # Remote user to act on
    ```
- Update variables in playbook file, the variables are as follow:
    - jws_version (which version of jws to install)
    - tomcat_install_java (where to install java)
    - tomcat_java_version (which version of java to install)
    - tomcat.listen.http.port and tomcat.listen.https.port (which http/https ports to listen on)
    - tomcat.listen.ajp (Where to use ajp and if yes configure the address, port, where to use a secret etc.)

- If you are using a non root remote user, then set username and enable sudo:
    ```
        become: yes
        become_method: sudo
    ```


Mod Cluster Listener
====
## What does the mod cluster listener do:
Allows communication between Apache Tomcat and the Apache HTTP Server’s mod_proxy_cluster module. This allows the Apache HTTP Server to be used as a load balancer for JBoss Web Server. For information on the configuration of mod_cluster, or for information on the installation and configuration of the alternative load balancers mod_jk and mod_proxy, see the [HTTP Connectors and Load Balancing Guide](https://access.redhat.com/documentation/en-us/red_hat_jboss_core_services/2.4.37/html-single/apache_http_server_connectors_and_load_balancing_guide/ "HTTP Connectors and Load Balancing Guide").  


## How to enable mod cluster listener:

All that you have to do to enable a mod_cluster listener for jws is to edit the mod_cluster variables in the playbook:  
- mod_cluster.enable (Set to True to enable the listener)
- mod_cluster.ip (Set the ip of the mod_cluster instance)
- mod_cluster.port (Set the port of the mod_cluster instance)


Running Playbook
====

Once all values are updated, you can then run the playbook against your nodes.
<!-- First of all export CATALINA_HOME:
```
$ export CATALINA_HOME=/opt/apache-tomcat-9.0.40/
``` -->

Playbook executed as root user - with ssh key:

```
$ ansible-playbook -i hosts playbook.yml
```

Playbook executed as root user - with password:

```
$ ansible-playbook -i hosts playbook.yml --ask-pass
```

Playbook executed as sudo user - with password:

```
$ ansible-playbook -i hosts playbook.yml --ask-pass --ask-become-pass
```

Playbook executed as sudo user - with ssh key and sudo password:

```
$ ansible-playbook -i hosts playbook.yml --ask-become-pass
```

Playbook executed as sudo user - with ssh key and passwordless sudo:

```
$ ansible-playbook -i hosts playbook.yml --ask-become-pass
```

Execution should be successful without errors






