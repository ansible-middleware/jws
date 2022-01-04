# JWS Collection for Ansible

This repository contains the ansible playbook to setup JWS.

## Ansible version compatibility

This collection has been tested against following Ansible versions: **>=2.9.10**.

Plugins and modules within a collection may be tested with only specific Ansible versions. A collection may contain metadata that identifies these versions.
<!--end requires_ansible-->
## Included content

Click on the name of a plugin or module to view that content's documentation:

### Collections

- redhat.redhat_csp_download
    - This collection is required to download resources from RedHat Customer Portal.
    - Documentation to collection can be found at <https://github.com/ansible-middleware/redhat-csp-download>

### Roles

- The tomcat role which:
    - Installs JAVA
    - Installs basic packages required
    - Adds tomcat user and group
    - Downloads tomcat and install
    - Makes sure that tomcat belongs to the tomcat user and group
    - Deploys server.xml, web.xml and context.xml

## Installation and Usage

### How to use this playbook


The use of the playbook will vary on the installation method you choose. There is currently
three options:

- local zipfile
- rpm
- using your RHN credentials to retrieve the JWS zipfiles provided by Red Hat.

### Prerequisite


You can the playbook directly from this folder for demostration purpose, however, the proper way to install the collection is to build it and install it :

    $ ansible-galaxy collection build .
    $ ansible-galaxy collection install redhat-jws-*.tar.gz

### Using local zipfiles


Download the requires zipfiles from your Red Hat account:

- Red Hat JBoss Web Server 5.4.0 Application Server (the server itself)
- Red Hat JBoss Web Server 5.4.0 Application Server for RHEL 8 x86_64 (the native bits)

Provides the path to those zipfiles:

    vars:
      ...
      tomcat_zipfile: jws-5.4.0-application-server.zip
      native_zipfile: jws-5.4.0-application-server-RHEL8-x86_64.zip

Note that if you respect the naming convention above for the file name, you can just provide the JWS version instead of those two paths:

    vars:
      ...
      jws_version: 5.4.0


### Using RPM


Change the default install method to RPM and provide the appropriate Tomcat HOME in the playbooks:

    vars:
      ...
      tomcat_home: /opt/rh/jws5/root/usr/share/tomcat/
      tomcat_install_method: rpm

### Using RHN


To use the install method RHN zipfiles, simply set the method :

    vars:
       ...
       tomcat_install_method: rhn_zipfiles

And provide your RHN credentials (as argument or in file):

    rhn_username: alice@wonderland.org
    rhn_password: eatmeiamacookie

### Running the play books


1) Configure the install method as described above!

2) Update your inventory, e.g.:
    ```
        [tomcat]
        192.168.0.1      # Remote user to act on
    ```
3) Update variables in playbook file, the variables are as follow:
    - jws_version (which version of jws to install)
    - tomcat_install_java (where to install java)
    - tomcat_java_version (which version of java to install)
    - tomcat.listen.http.port and tomcat.listen.https.port (which http/https ports to listen on)
    - tomcat.listen.ajp (Where to use ajp and if yes configure the address, port, where to use a secret etc.)

Note: If you are using a non root remote user, then set username and enable sudo:
    ```
        become: yes
        become_method: sudo
    ```


## Mod Cluster Listener

### What does the mod cluster listener do:
Allows communication between Apache Tomcat and the Apache HTTP Server's mod_proxy_cluster module. This allows the Apache HTTP Server to be used as a load balancer for JBoss Web Server. For information on the configuration of mod_cluster, or for information on the installation and configuration of the alternative load balancers mod_jk and mod_proxy, see the [HTTP Connectors and Load Balancing Guide](https://access.redhat.com/documentation/en-us/red_hat_jboss_core_services/2.4.37/html-single/apache_http_server_connectors_and_load_balancing_guide/ "HTTP Connectors and Load Balancing Guide").


### How to enable mod cluster listener:

All that you have to do to enable a mod_cluster listener for jws is to edit the mod_cluster variables in the playbook:
- mod_cluster.enable (Set to True to enable the listener)
- mod_cluster.ip (Set the ip of the mod_cluster instance)
- mod_cluster.port (Set the port of the mod_cluster instance)


## Running Playbook


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


## Support

jws collection v1.0.0 is a Beta release and for [Technical Preview](https://access.redhat.com/support/offerings/techpreview). If you have any issues or questions related to collection, please don't hesitate to contact us on <Ansible-middleware-core@redhat.com> or open an issue on https://github.com/ansible-middleware/jws-ansible-playbook/issues

## License

Apache License v2.0 or later

See [LICENCE](LICENSE) to view the full text.
