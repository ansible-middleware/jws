# Ansible Collection - middleware_automation.jws

[![Build Status](https://github.com/ansible-middleware/jws/workflows/CI/badge.svg?branch=main)](https://github.com/ansible-middleware/jws/actions/workflows/ci.yml)

This repository contains the Ansible roles and playbooks to set up [Red Hat JBoss Web Server (JWS)](https://www.redhat.com/en/technologies/jboss-middleware/web-server).


## Ansible version compatibility

This collection has been tested against following Ansible versions: **>=2.9.10**.

Plugins and modules within a collection may be tested with only specific Ansible versions. A collection may contain metadata that identifies these versions.
<!--end requires_ansible-->


## Included content

### Roles

- The [jws](roles/jws/README.md) role which:
    - Installs JAVA
    - Installs basic packages required for JWS
    - Adds JWS user and group
    - Downloads JWS and installs the application server
    - Makes sure that JWS directory structure belongs to the JWS user and group
    - Deploys server.xml, web.xml, and context.xml


## Installation and Usage

### How to use this collection

The use of the collection will vary on the installation method you choose. The following options are available:

- Local JWS zipfiles
- The JWS RPMs
- Setting a custom URL for downloading the JWS archives


### Prerequisites

You can run the collection directly from this folder for demonstration purpose, however, the proper way is to install the collection using Galaxy:

    $ ansible-galaxy collection install middleware_automation.jws

For **development purpose**, if you want to test changes to the collection itself, you can build and install it using the following commands:

    $ ansible-galaxy collection build .
    $ ansible-galaxy collection install middleware_automation-jws-*.tar.gz

## Installing JWS

### Using local JWS zipfiles

Download the required zipfiles from your Red Hat account, and place them into the directory you execute the ansible-playbook command on the controller:

- Red Hat JBoss Web Server 5.7.0 Application Server (the application server itself)
- Red Hat JBoss Web Server 5.7.0 Application Server for RHEL 8 x86_64 (the native components)

Provide the path to those zipfiles:

    vars:
      ...
      jws_install_method: zipfiles
      jws_version: 5.7.0
      zipfile_name: jws-5.7.0-application-server.zip
      native_zipfile: jws-5.7.0-application-server-RHEL8-x86_64.zip$
      jws_native: True

Note that if you respect the naming convention above for the file name, which is the default filename as set by the RHN download, you can just provide the JWS version instead of those two paths:

    vars:
      ...
      jws_version: 5.7.0


Note: if you provide the `jws_version` and set `jws_native` to `True`, then the collection will compute the value of `jws_native_zipfile` for you.

### Using JWS RPMs

Change the default installation method to RPM and provide the appropriate Tomcat HOME in the playbooks:

    vars:
      ...
      jws_home: /opt/rh/jws5/root/usr/share/tomcat/
      jws_install_method: rpm


### Using a custom URL to download the JWS archives

To use the installation method zipfiles, downloading from a custom URL, set :

    vars:
       ...
       jws_install_method: zipfiles
       zipfile_name: tomcat-x.y.z.zip
       zipfile_name_url: https://binary.repository.internal.company/tomcat-x.y.z.zip


### Running the playbooks

1. Configure the installation method as described above!

2. Update your inventory, e.g.:

    ~~~
    [jws]
    192.168.0.1      # Remote host to act on
    ~~~

3. Update variables in vars.yml file; the variables are as follows:
    - `jws_version` (which version of jws to install)
    - `jws_java_version` (which version of java to install, i.e. name of the JVM rpm package)
    - `jws_listen_http_port` and/or `tomcat_listen_https_port` (which http/https ports to listen on)

4. Run the playbook; see [Running the Playbook](#running-the-playbook) below!

Note: If you are using a non-root remote user, then set username and enable sudo:

~~~
become: yes
become_method: sudo
~~~

## ModCluster Listener

### What does the ModCluster Listener do

Allows communication between Apache Tomcat and the Apache HTTP Server's mod_proxy_cluster module. This allows the Apache HTTP Server to be used as a load balancer for JBoss Web Server. For information on the configuration of mod_cluster, or for information on the installation and configuration of the alternative load balancers mod_jk and mod_proxy, see the [HTTP Connectors and Load Balancing Guide](https://access.redhat.com/documentation/en-us/red_hat_jboss_core_services/2.4.37/html-single/apache_http_server_connectors_and_load_balancing_guide/ "HTTP Connectors and Load Balancing Guide").


### How to enable ModCluster Listener

All that you have to do to enable a mod_cluster listener for jws is to edit the mod_cluster variables in the vars.yml:

- `jws_modcluster_enabled` (Set to True to enable the listener)
- `jws_modcluster_ip` (Set the ip of the mod_cluster instance)
- `jws_modcluster_port` (Set the port of the mod_cluster instance)

(This feature is validated and tested by the following [Molecule scenario](https://github.com/ansible-middleware/jws/tree/main/molecule/ajp_or_https) )

## Vault for JWS

### What does tomcat-vault do

Allows users to mask passwords and other sensitive strings, and store them in an encrypted Java keystore. Using the vault enables you to stop storing clear-text passwords in your Tomcat configuration files, because Tomcat can look up passwords and other sensitive strings from a keystore using the vault.

### How to enable tomcat-vault

Before you can enable the tomcat-vault feature, you must follow our documentation on how to create the required files for the feature to function. Please visit the [JWS Installation Guide, Chapter 6](https://access.redhat.com/documentation/en-us/red_hat_jboss_web_server/5.7/html-single/installation_guide/index#vault_for_jws) for next steps, and return here once you've generated your vault files.

Once you have your vault files (`vault.keystore`, `VAULT.dat` and `vault.properties`), you'll need to set the following variables to point to each file:

    ~~~
    ...
    jws_vault_name: ./vault_files/vault.keystore
    jws_vault_data: ./vault_files/VAULT.dat
    jws_vault_properties: ./vault_files/vault.properties
    ...
    ~~~

With this configuration done, you can turn on the vault feature by setting `jws_tomcat_vault_enabled` to `True` in your `vars.yml` file.

In addition to that, you need to provide several other bits of information from the tomcat-vault configuration step in Chapter 6. You'll need to set the following variables to match the values used in your tomcat-vault configuration:

* `jws_tomcat_vault_alias`
* `jws_tomcat_vault_storepass`
* `jws_tomcat_vault_iteration`
* `jws_tomcat_vault_salt`

## Enable HTTPS

The default template for `server.xml` provided with this Ansible collection already includes the required configuration to use HTTPS. It just needs to be activated. However, the collection does not build, nor provide the required Java Keystore. It expects it to be already installed and available.

    jws_listen_https_enabled: True
    # add the following variable to change default port (default: 8443)
    jws_listen_https_port: '8443'
    # add the following variable to change default bind address (default: localhost)
    jws_listen_https_bind_address: 'localhost'
    # add the following variable to change the default path to the keystore file (default: /etc/ssl/keystore.jks)
    jws_listen_https_keystore_file: /etc/ssl/keystore.jks
    # add the following variable to change the default password to the keystore (default: changeit)
    jws_listen_https_keystore_password: changeit

Please refer to the [server documentation](https://tomcat.apache.org/tomcat-9.0-doc/ssl-howto.html#Quick_Start) for more details on the setup and configuration of this feature.

Note: There are other collections and modules available to automate the creation of those files (such as [Ansible OpenSSH Keypair collection](https://docs.ansible.com/ansible/latest/collections/community/crypto/openssh_keypair_module.html), [Ansible Collection Community Crypto](https://docs.ansible.com/ansible/latest/collections/community/crypto/index.html) and the [Java Keystore module](https://docs.ansible.com/ansible/latest/collections/community/general/java_keystore_module.html)). Please refer to those in order to automate this part.

(This feature is validated and tested by the following [Molecule scenario](https://github.com/ansible-middleware/jws/tree/main/molecule/ajp_or_https) )

## Overriding the default template for server.xml

The provided template for the `server.xml.j2` covers the most basic use case of the server. It's most likely that a user will need to replace this template by its own, in order to deploy a fine-grained configuration, suiting one's use case. To do so, just change of this default variable:

    jws_conf_templates_server: path/to/my_template_for_server_xml.j2

(This feature is validated and tested by the following [Molecule scenario](https://github.com/ansible-middleware/jws/tree/main/molecule/override_server_xml) )

## How to deploy webapps?

Simply use Ansible existing module! For instance, you can use the [get_url:](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/get_url_module.html) module to deploy a webapp downloaded from a repository:

    - name: Download App
      get_url:
        url: https://repo1.maven.org/maven2/org/jolokia/jolokia-war/1.7.1/jolokia-war-1.7.1.war
        dest: "{{ jws_home }}/tomcat/webapps/"

Another option is to use the [copy:](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/copy_module.html) module that allows to deploy a file from the Ansible controller to the target:

    - ansible.builtin.copy:
       src: files/jolokia-war-1.7.1.war
       dest: "{{ jws_home }}/tomcat/webapps/"

This module can also be used if the file already exists on the target host:

    - ansible.builtin.copy:
       src: files/jolokia-war-1.7.1.war
       dest: "{{ jws_home }}/tomcat/webapps/"
       remote_src: yes

However, to avoid duplicating the files, a symlink or hardlink can also be used instead, using the module [file:](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/file_module.html):

    - ansible.builtin.file:
        src: /apps/jolokia-war-1.7.1.war
        dest: "{{ jws_home }}/tomcat/webapps/jolokia-war-1.7.1.war"
        state: link

Bottom line: Ansible has many features to help deploy webapps into the appropriate directory for the server!

## Running the Playbook

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

This collection is released as [Technical Preview](https://access.redhat.com/support/offerings/techpreview) for Red Hat Customer [JWS Ansible Collection](https://console.redhat.com/ansible/automation-hub/repo/published/redhat/jws). If you have any issues or questions related to collection, please don't hesitate to contact us on <Ansible-middleware-core@redhat.com> or open an issue on https://github.com/ansible-middleware/jws/issues


## License

Apache License v2.0 or later

See [LICENSE](LICENSE) to view the full text.
