# Ansible Collection - middleware_automation.jws

[![Build Status](https://github.com/ansible-middleware/jws-ansible-playbook/workflows/CI/badge.svg?branch=main)](https://github.com/ansible-middleware/jws-ansible-playbook/actions/workflows/ci.yml)

This repository contains the ansible roles and playbooks to setup [Red Hat JBoss Web Server](https://www.redhat.com/en/technologies/jboss-middleware/web-server).


## Ansible version compatibility

This collection has been tested against following Ansible versions: **>=2.9.10**.

Plugins and modules within a collection may be tested with only specific Ansible versions. A collection may contain metadata that identifies these versions.
<!--end requires_ansible-->


### Dependencies

- `middleware_automation.redhat_csp_download`
    - This collection is required to download resources from Red Hat Customer Portal.
    - Documentation for the collection can be found at <https://github.com/ansible-middleware/redhat-csp-download>


## Included content

### Roles

- The [jws](roles/jws/README.md) role which:
    - Installs JAVA
    - Installs basic packages required
    - Adds tomcat user and group
    - Downloads tomcat or jws and installs
    - Makes sure that tomcat belongs to the tomcat user and group
    - Deploys server.xml, web.xml and context.xml


## Installation and Usage

### How to use this playbook

The use of the playbook will vary on the installation method you choose. The following options are available:

- local zipfile
- rpm
- using your RHN credentials to retrieve the JWS zipfiles provided by Red Hat
- setting a custom URL for downloading the tomcat/JWS archives


### Prerequisite

You can the playbook directly from this folder for demostration purpose, however, the proper way to install the collection is to build it and install it :

    $ ansible-galaxy collection build .
    $ ansible-galaxy collection install middleware_automation-jws-*.tar.gz


### Install tomcat community version

After adding the following variables, the playbook will download tomcat from github and install the requested version of tomcat:


```
    vars:
      jws_install_method: zipfiles
      tomcat_version: 9.0.50
      tomcat_home: "{{ tomcat_install_dir }}/apache-tomcat-{{ tomcat_version }}"
```


### Using JWS from Red Hat Customer Portal

To use JWS, set the install method to rhn_zipfiles:

    vars:
       ...
       jws_install_method: rhn_zipfiles

To automatically download the JWS archives from Red Hat Customer Portal, provide your RHN credentials (as extra argument, in variable file, or better loaded from vault):

    rhn_username: alice@wonderland.org
    rhn_password: eatmeiamacookie


### Using JWS local zipfiles

It is possible to skip the download (ie. for offline installations) by making the downloaded archives available on the playbook directory on the controller.

Download the requires zipfiles from your Red Hat account:

- Red Hat JBoss Web Server 5.6.0 Application Server (the server itself)
- Red Hat JBoss Web Server 5.6.0 Application Server for RHEL 8 x86_64 (the native bits)

Provide the path to those zipfiles:

    vars:
      ...
      jws_install_method: rhn_zipfiles
      jws_version: 5.6.0
      tomcat_zipfile: jws-5.6.0-application-server.zip
      tomcat_native_zipfile: jws-5.6.0-application-server-RHEL8-x86_64.zip

Note that if you respect the naming convention above for the file name, which is the default filename as set by the RHN download, you can just provide the JWS version instead of those two paths:

    vars:
      ...
      jws_version: 5.6.0


Note: if you provide the `jws_version` and set `tomcat_native` to True, then the collection will compute the value of tomcat_native_zipfile for you.


### JWS Versions

| JWS VERSION | Release Date      | Tomcat Version | Native Version | Notes           |
|:------------|:------------------|:---------------|:---------------|:----------------|
|`5.6.0`      |November 30, 2021  |`9.0.50`        | `1.2.30`       |[Release Notes](https://access.redhat.com/documentation/en-us/red_hat_jboss_web_server/5.6/html/red_hat_jboss_web_server_5.6_release_notes/index)|
|`5.5.0`      |June 29, 2021      |`9.0.43`        | `1.2.26`       |[Release Notes](https://access.redhat.com/documentation/en-us/red_hat_jboss_web_server/5.5/html/red_hat_jboss_web_server_5.5_release_notes/index)|
|`5.4.0`      |November 23, 2020  |`9.0.36`        | `1.2.25`       |[Release Notes](https://access.redhat.com/documentation/en-us/red_hat_jboss_web_server/5.4/html/red_hat_jboss_web_server_5.4_release_notes/index)|
|`5.3.0`      |April 21, 2020     |`9.0.30`        | `1.2.23`       |[Release Notes](https://access.redhat.com/documentation/en-us/red_hat_jboss_web_server/5.3/html/red_hat_jboss_web_server_5.3_release_notes/index)|
|`5.2.0`      |November 20, 2019  |`9.0.21`        | `1.2.21`       |[Release Notes](https://access.redhat.com/documentation/en-us/red_hat_jboss_web_server/5.2/html/red_hat_jboss_web_server_5.2_release_notes/index)|
|`5.1.0`      |May 08, 2019       |`9.0.7`         | `1.2.17`       |[Release Notes](https://access.redhat.com/documentation/en-us/red_hat_jboss_web_server/5.1/html/red_hat_jboss_web_server_5.1_release_notes/index)|


### JWS Patching

When variable `jws_apply_patches` is `True` (default: `False`), the role will automatically apply the latest cumulative patch for the selected base version.

| JWS VERSION | Release Date      | JWS LATEST CP | Notes           |
|:------------|:------------------|:--------------|:----------------|
|`5.6.0`      |April 29, 2022     |`5.6.2`        |[Release Notes](https://access.redhat.com/documentation/en-us/red_hat_jboss_web_server/5.6/html/red_hat_jboss_web_server_5.6_service_pack_2_release_notes/index)|
|`5.5.0`      |October 06, 2021   |`5.5.1`        |[Release Notes](https://access.redhat.com/documentation/en-us/red_hat_jboss_web_server/5.5/html/red_hat_jboss_web_server_5.5_service_pack_1_release_notes/index)|
|`5.4.0`      |April 14, 2021     |`5.4.2`        |[Release Notes](https://access.redhat.com/documentation/en-us/red_hat_jboss_web_server/5.4/html/red_hat_jboss_web_server_5.4_service_pack_2_release_notes/index)|
|`5.3.0`      |August 04, 2020    |`5.3.2`        |[Release Notes](https://access.redhat.com/documentation/en-us/red_hat_jboss_web_server/5.3/html/red_hat_jboss_web_server_5.3_service_pack_2_release_notes/index)|


Refer to [JBoss Web Server Component Details](https://access.redhat.com/articles/111723) for additional details.


### Using RPM

Change the default install method to RPM and provide the appropriate Tomcat HOME in the playbooks:

    vars:
      ...
      tomcat_home: /opt/rh/jws5/root/usr/share/tomcat/
      jws_install_method: rpm


### Using a custom download URL

To use the install method zipfiles, downloading from a custom URL, set :

    vars:
       ...
       jws_install_method: zipfiles
       tomcat_zipfile: tomcat-x.y.z.zip
       tomcat_zipfile_url: https://binary.repository.internal.company/tomcat-x.y.z.zip


### Running the play books

1) Configure the install method as described above!

2) Update your inventory, e.g.:
    ```
        [tomcat]
        192.168.0.1      # Remote user to act on
    ```
3) Update variables in playbook file, the variables are as follow:
    - `jws_version` or `tomcat_version` (which version of jws or tomcat to install)
    - `jws_java_version` (which version of java to install, ie. name of the JVM rpm package)
    - `tomcat_listen_http_port` and `tomcat_listen_https_port` (which http/https ports to listen on)
    - `tomcat_listen_ajp_enabled` and `tomcat_listen_ajp_*` (Whether to use ajp and if yes configure the address, port, where to use a secret etc.)

Note: If you are using a non root remote user, then set username and enable sudo:
    ```
        become: yes
        become_method: sudo
    ```


## Mod Cluster Listener

### What does the mod_cluster listener do

Allows communication between Apache Tomcat and the Apache HTTP Server's mod_proxy_cluster module. This allows the Apache HTTP Server to be used as a load balancer for JBoss Web Server. For information on the configuration of mod_cluster, or for information on the installation and configuration of the alternative load balancers mod_jk and mod_proxy, see the [HTTP Connectors and Load Balancing Guide](https://access.redhat.com/documentation/en-us/red_hat_jboss_core_services/2.4.37/html-single/apache_http_server_connectors_and_load_balancing_guide/ "HTTP Connectors and Load Balancing Guide").


### How to enable mod_cluster listener

All that you have to do to enable a mod_cluster listener for jws is to edit the mod_cluster variables in the playbook:
- `tomcat_modcluster_enabled` (Set to True to enable the listener)
- `tomcat_modcluster_ip` (Set the ip of the mod_cluster instance)
- `tomcat_modcluster_port` (Set the port of the mod_cluster instance)

(This feature is validated and tested by the following [Molecule scenario](https://github.com/ansible-middleware/jws-ansible-playbook/tree/main/molecule/ajp) )

## Enable HTTPS

The default template for server.xml provided with this Ansible collection already includes the required configuration to use HTTPS. It just need to be activated. However, the collection does not build, nor provide the requires SSH keys and Java Keystore. It expects it to be already installed and available.

    tomcat_listen_https_enabled: True
    # uncomment the following line to change the default value for the java keystore path
    #tomcat_listen_https_keystore_file: /etc/ssl/keystore.jks

Please refers to the [server documentation](https://tomcat.apache.org/tomcat-9.0-doc/ssl-howto.html#Quick_Start) for more details on the setup and configuration of this feature.

Note: There other collections and modules available to automate the creation of those files (such as [Ansible OpenSSH Keypair collection](https://docs.ansible.com/ansible/latest/collections/community/crypto/openssh_keypair_module.html), [Ansible Collection Community Crytpo](https://docs.ansible.com/ansible/latest/collections/community/crypto/index.html) and the [Java Keystore module](https://docs.ansible.com/ansible/latest/collections/community/general/java_keystore_module.html)). Please refers to those in order to automate this part.

(This feature is validated and tested by the following [Molecule scenario](https://github.com/ansible-middleware/jws-ansible-playbook/tree/main/molecule/https) )

## Overriding the default template for server.xml

The provided template for the server.xml.j2 covers the most basic use case of the server. It's most likely that a user will need to replace this template by its own, it order to deploy a fine-grained configuration, suiting one's use case. To do so, just change of this default variable:

    tomcat_conf_templates_server: path/to/my_template_for_server_xml.j2

(This feature is validated and tested by the following [Molecule scenario](https://github.com/ansible-middleware/jws-ansible-playbook/tree/main/molecule/override_server_xml) )

## How to deploy webapps?

Simply use Ansible existing module! For instance, you can use the [get_url:](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/get_url_module.html) module to deploy a webapp downloaded from a repository:

    - name: Download App
      get_url:
        url: https://repo1.maven.org/maven2/org/jolokia/jolokia-war/1.7.1/jolokia-war-1.7.1.war
        dest: "{{ tomcat_home }}/webapps/"

Another option is to use the [copy:](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/copy_module.html) module that allow to deploy a file from the Ansible controller to the target:

    - ansible.builtin.copy:
       src: files/jolokia-war-1.7.1.war
       dest: "{{ tomcat_home }}/webapps/"

This module can also be used if the file already exists on the target host:

    - ansible.builtin.copy:
       src: files/jolokia-war-1.7.1.war
       dest: "{{ tomcat_home }}/webapps/"
       remote_src: yes

However, to avoid duplicating the files, a symlink or hardlink can also be used instead using the module [file:](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/file_module.html):

    - ansible.builtin.file:
        src: /apps/jolokia-war-1.7.1.war
        dest: "{{ tomcat_home }}/webapps/jolokia-war-1.7.1.war"
        state: link

Bottom line: Ansible has many features to help deploy webapps into the appropriate directory for the server!

## Running Playbook

Once all values are updated, you can then run the playbook against your nodes.

Playbook executed as root user - with ssh key:

```
$ ansible-playbook -i hosts playbooks/playbook.yml
```

Playbook executed as root user - with password:

```
$ ansible-playbook -i hosts playbooks/playbook.yml --ask-pass
```

Playbook executed as sudo user - with password:

```
$ ansible-playbook -i hosts playbooks/playbook.yml --ask-pass --ask-become-pass
```

Playbook executed as sudo user - with ssh key and sudo password:

```
$ ansible-playbook -i hosts playbooks/playbook.yml --ask-become-pass
```

Playbook executed as sudo user - with ssh key and passwordless sudo:

```
$ ansible-playbook -i hosts playbooks/playbook.yml --ask-become-pass
```

Execution should be successful without errors


## Support

jws collection is a Beta release and for [Technical Preview](https://access.redhat.com/support/offerings/techpreview). If you have any issues or questions related to collection, please don't hesitate to contact us on <Ansible-middleware-core@redhat.com> or open an issue on https://github.com/ansible-middleware/jws-ansible-playbook/issues


## License

Apache License v2.0 or later

See [LICENCE](LICENSE) to view the full text.
