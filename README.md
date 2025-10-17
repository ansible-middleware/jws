# Ansible Collection - middleware_automation.jws
<!--start build_status -->
[![Build Status](https://github.com/ansible-middleware/jws/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/ansible-middleware/jws/actions/workflows/ci.yml)
<!--end build_status -->

This repository contains the Ansible roles and playbooks to set up an automated installation of [Red Hat JBoss Web Server (JWS)](https://www.redhat.com/en/technologies/jboss-middleware/web-server).


## Ansible version compatibility

<!--start ansible_version -->
This collection has been tested against Ansible versions 2.14.0 or later.

The plug-ins and modules that are within a collection might be tested with specific Ansible versions only. A collection can contain metadata that identifies these Ansible versions.
<!--end ansible_version -->


## Content included in this collection

### Roles
<!--start role_content -->
- The [jws](roles/jws/README.md) role contains the Ansible playbook and handles the following automated tasks:
    - Ensures that a Java Development Kit (JDK) is installed on your target hosts
    - Installs the basic packages that a JBoss Web Server installation requires
    - Creates a JBoss Web Server user account and group
    - Installs JBoss Web Server from product archive files or RPM packages
    - Assigns ownership of the JBoss Web Server directories to the appropriate user account and group
    - Deploys the `server.xml`, `web.xml`, and `context.xml` files
- The [jws_validation](roles/jws/README.md) role contains a set of utility playbooks and tasks to validate that the server was properly installed on the target and is functionnal.
<!--end role_content -->

## Collection setup

<!--start galaxy_download -->

For demonstration purposes, you can run the collection directly from this folder. However, the proper setup is to install the collection by using Ansible Galaxy:

    $ ansible-galaxy collection install middleware_automation.jws

For **development purposes**, if you want to test changes to the collection, you can build and install the collection by using the following commands:

    $ ansible-galaxy collection build .
    $ ansible-galaxy collection install middleware_automation-jws-*.tar.gz
<!--end galaxy_download -->


## Collection usage for installing JBoss Web Server

You can enable the collection to use any of the following installation methods when performing an automated installation of JBoss Web Server:

- Local archive files
- RPM packages
- Custom URL for downloading the archive files

<!--start rhn_credentials -->
<!--end rhn_credentials -->

### Using local archive files

To enable the collection to install JBoss Web Server from local archive files:

1. If copies of the archive files are not already on your system, download the appropriate archive files from the Red Hat Customer Portal:

    - Red Hat JBoss Web Server _X_._Y_.0 Application Server *(the application server)*
    - Red Hat JBoss Web Server _X_._Y_.0 Application Server for RHEL 8 x86_64 *(the native components)*

   In the preceding file names, replace _X_._Y_.0 with the JBoss Web Server version that you want to install (for example, 5.7.0 or 6.0.0).

2. Copy the archive files to your Ansible control node.

3. On your Ansible control node, set the following variables, as appropriate:

        vars:
          ...
          jws_install_method: zipfiles
          jws_version: 6.0.0
          jws_native: True
          zipfile_name: <application_server_filename>.zip
          native_zipfile: <native_filename>.zip

    Consider the following guidelines:

   | Variable               | Details                                                                                             |
   |------------------------|-----------------------------------------------------------------------------------------------------|
   | `jws_install_method`   | Specifies the installation method (by default, `zipfiles`)                                          |
   | `jws_version`          | Specifies the version of JBoss Web Server that you want to install (for example, `5.7.0` or `6.0.0`)|
   | `jws_native`           | Indicates whether you also want to install the native archive file (by default, `False`)            |
   | `zipfile_name`         | Specifies the name of the application server archive file on your control node                      |
   | `native_zipfile`       | Specifies the name of the native archive file on your control node                                  |
   | `jws_offline_install`  | Indicates whether to execute a completely offline install                                           |

<!--start no_native_upstream -->

**Note:** By default, the collection installs the main application server archive only. If you also want to install the native archive, ensure that you copy the native archive file to your control node and set the `jws_native` variable to `True`.

<!--end no_native_upstream -->

**Note:** If you did not change the archive file names, you do not need to set the `zipfile_name` and `native_zipfile` variables. The collection uses the JBoss Web Server version to determine the default file names automatically.

4. If you also want to install the latest cumulative patches for the appropriate JBoss Web Server version, copy the archive files for the latest patch updates to your Ansible control node. Then set the `jws_apply_patches` variable to `True`:

        vars:
          ...
          jws_apply_patches: True

> **Note:** Even when local file are present on the controller, the role tries to contact the download server to verify is new cumulative patches are available. To completely turn off
any remote access, set the parameter `jws_offline_install: True`


### Using RPM packages

If you want the collection to install JBoss Web Server from RPM packages, you must first ensure that your system complies with the following prerequisites:

- Your system is compliant with [Red Hat Enterprise Linux package requirements](https://access.redhat.com/documentation/en-us/red_hat_jboss_web_server/6.0/html-single/installation_guide/index#rhel_requirements_rpm).

- You have [attached subscriptions to Red Hat Enterprise Linux](https://access.redhat.com/documentation/en-us/red_hat_jboss_web_server/6.0/html-single/installation_guide/index#attach_subscriptions).

- You have a working internet connection that the collection can use to obtain the RPM packages from Red Hat.

> **Note:** When you enable the RPM installation method, the collection always installs the latest available RPM packages for the latest JBoss Web Server version, including any patch updates.

To enable the collection to install JBoss Web Server from RPM packages, set the `jws_install_method` variable to `rpm` on your Ansible control node:

    vars:
      ...
      jws_install_method: rpm

> **Note:** By default, the collection installs JBoss Web Server in the `/opt/rh/jws6/root/usr/share/tomcat/` directory. If you want to use a different installation directory, you can manually create a symbolic link to `/opt/rh/jws6/root/usr/share/tomcat/`.


### Using a custom URL to download the archive files

To enable the collection to download and install the JBoss Web Server archive files from a custom URL, set the following variables on your Ansible control node:

    vars:
       ...
       jws_install_method: zipfiles
       zipfile_name: <archive_file_name>.zip
       zipfile_name_url: <URL_path/archive_file_name>.zip

In the preceding example, ensure that the `zipfile_name` and `zipfile_name_url` variables specify the correct archive file name and URL path, respectively.


### Running the playbook

To run the playbook:

1. Set the `jws_install_method` variable to the appropriate installation method, as described in the preceding sections.

2. Update the inventory for your target hosts. For example:

~~~
[jws]
192.168.0.1      # Remote host to act on
~~~

3. If you want the collection to install a supported OpenJDK version on your target hosts, set the `jws_java_version` variable to the appropriate value (for example, `1.8.0`, `11`,`17` or `21` (for RHEL 10)). The collection is not configured to install a JDK by default.

4. Set the `jws_listen_http_port` and `jws_listen_https_port` variables to specify which HTTP and HTTPS ports you want JBoss Web Server to listen on. The default HTTP port is 8080. The default HTTPS port is 8443.

5. Run the playbook. For more information, see [Running the Playbook](#running-the-playbook).

> **Note:** If you are using a remote user account that is not the root user, set the username and enable sudo privileges:

~~~
become: True
become_method: sudo
~~~

## Using the collection to configure the `mod_cluster` listener

The `mod_cluster` listener enables communication between JBoss Web Server and the `mod_proxy_cluster` module on the Apache HTTP Server. The `mod_proxy_cluster` module enables use of the Apache HTTP Server as an intelligent load-balancing solution for sending requests to JBoss Web Server. For information about configuring `mod_proxy_cluster` and alternative load balancers such as `mod_jk` and `mod_proxy`, see the [Apache HTTP Server Connectors and Load Balancing Guide](https://access.redhat.com/documentation/en-us/red_hat_jboss_core_services/2.4.51/html-single/apache_http_server_connectors_and_load_balancing_guide/ "Apache HTTP Server Connectors and Load Balancing Guide").

To enable the collection to configure the `mod_cluster` listener, set the following variables on your Ansible control node:

    vars:
      ...
      jws_modcluster_enabled: True
      jws_modcluster_ip: <ip_address>
      jws_modcluster_port: <port>

Consider the following guidelines:

| Variable                 | Details                                                                                                      |
|--------------------------|--------------------------------------------------------------------------------------------------------------|
| `jws_modcluster_enabled` | Indicates whether you want to enable `mod_cluster` (by default, `False`)                                     |
| `jws_modcluster_ip`      | Specifies the bind address for the `mod_cluster` instance on each target host (by default, `127.0.0.1`)      |
| `jws_modcluster_port`    | Specifies the port that the `mod_cluster` instance uses to listen for incoming requests (by default, `6666`) |

The following [Molecule scenario](https://github.com/ansible-middleware/jws/tree/main/molecule/ajp_or_https) supports the validation and testing of this feature.

## Using the collection to configure the password vault for JBoss Web Server

You can use the password vault for JBoss Web Server, which is named `tomcat-vault`, to mask passwords and other sensitive strings, and to store sensitive information in an encrypted Java keystore. When you use the password vault, you can stop storing clear-text passwords in your JBoss Web Server configuration files. JBoss Web Server can use the password vault to search for passwords and other sensitive strings from a keystore.

> **Note:** If you want to use the password vault feature, you must first create the required `vault.keystore`, `VAULT.dat`, and `vault.properties` files as a prerequisite. For more information about creating these files, see the [Red Hat JBoss Web Server Installation Guide: Using a password vault with Red Hat JBoss Web Server](https://access.redhat.com/documentation/en-us/red_hat_jboss_web_server/6.0/html-single/installation_guide/index#vault_for_jws).

To enable the collection to configure the password vault, set the following variables on your Ansible control node:

    vars:
      ...
      jws_vault_name: ./vault_files/vault.keystore
      jws_vault_data: ./vault_files/VAULT.dat
      jws_vault_properties: ./vault_files/vault.properties
      jws_tomcat_vault_enabled: True
      jws_tomcat_vault_alias: <keystore_alias>
      jws_tomcat_vault_storepass: <keystore_password>
      jws_tomcat_vault_iteration: <iteration_count>
      jws_tomcat_vault_salt: <salt>

Consider the following guidelines:

| Variable                     | Details                                                                              |
|------------------------------|--------------------------------------------------------------------------------------|
| `jws_vault_name`             | Specifies the path to the `vault.keystore` file                                      |
| `jws_vault_data`             | Specifies the path to the `VAULT.dat` file                                           |
| `jws_vault_properties`       | Specifies the path to the `vault.properties` file                                    |
| `jws_tomcat_vault_enabled`   | Indicates whether you want to enable the password vault (by default, `False`)        |
| `jws_tomcat_vault_alias`     | Specifies the keystore alias that you configured when creating the required files    |
| `jws_tomcat_vault_storepass` | Specifies the keystore password that you configured when creating the required files |
| `jws_tomcat_vault_iteration` | Specifies the iteration count that you configured when creating the required files   |
| `jws_tomcat_vault_salt`      | Specifies the salt value that you configured when creating the required files        |


## Using the collection to enable HTTPS support

The collection provides a default template for the `server.xml` file that already includes the required configuration to use HTTPS. To enable HTTPS support, you only need to set the appropriate variables. However, the collection does not build or provide the required Java Keystore. In this situation, you must ensure that a Java keystore already exists on each target host.

> **Note:** To automate the creation of Java keystore files, you can use other collections and modules, such as the [Ansible OpenSSH Keypair collection](https://docs.ansible.com/ansible/latest/collections/community/crypto/openssh_keypair_module.html), the [Ansible Collection Community Crypto](https://docs.ansible.com/ansible/latest/collections/community/crypto/index.html) and the [Java Keystore module](https://docs.ansible.com/ansible/latest/collections/community/general/java_keystore_module.html). For more information about automating the creation of a Java keystore, refer to the available documentation for these collections or modules.

To enable the collection to configure HTTPS support, set the following variables on your Ansible control node, as appropriate:

    vars:
      ...
      jws_listen_https_enabled: True
      jws_listen_https_port: <port>
      jws_listen_https_bind_address: <ip_address>
      jws_listen_https_keystore_file: <keystore_path>
      jws_listen_https_keystore_password: <keystore_password>

Consider the following guidelines:

| Variable                             | Details                                                                                           |
|--------------------------------------|---------------------------------------------------------------------------------------------------|
| `jws_listen_https_enabled`           | Indicates whether you want to enable HTTPS support (by default, `False`)                          |
| `jws_listen_https_port`              | Specifies the port that JBoss Web Server uses to listen for HTTPS requests (by default, `8443`)   |
| `jws_listen_https_bind_address`      | Specifies the bind address for HTTPS requests on each target host (by default, `localhost`)       |
| `jws_listen_https_keystore_file`     | Specifies the path to the Java keystore on each target host (by default, `/etc/ssl/keystore.jks`) |
| `jws_listen_https_keystore_password` | Specifies the Java keystore password on each target host (by default, `changeit`)                 |


Refer to the [Apache Tomcat documentation](https://tomcat.apache.org/tomcat-9.0-doc/ssl-howto.html#Quick_Start) for more information about setting up and configuring HTTPS support.

The following [Molecule scenario](https://github.com/ansible-middleware/jws/tree/main/molecule/ajp_or_https) supports the validation and testing of this feature.

## Using the collection to override the default template for `server.xml`

The collection provides a default `server.xml.j2` template that covers the most basic server configuration only. To ensure a more fine-tuned configuration that suits your requirements, you can override the default template with your own customized template.

To override the default template, set the following variable on your Ansible control node:

    vars:
      ...
      jws_conf_templates_server: <path_to_custom_template>.j2

The following [Molecule scenario](https://github.com/ansible-middleware/jws/tree/main/molecule/override_server_xml) supports the validation and testing of this feature.

## Using the collection to deploy web applications

Ansible provides various modules and features to facilitate the deployment of web applications on your target hosts.

For example:

- To deploy an application by downloading the `.war` file from a repository, use the [get_url:](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/get_url_module.html) module:

        - name: Download App
          get_url:
            url: https://repo1.maven.org/maven2/org/jolokia/jolokia-war/1.7.1/jolokia-war-1.7.1.war
            dest: "{{ jws_home }}/tomcat/webapps/"

- To deploy an application by copying the `.war` file from your control node to the target host, use the [copy:](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/copy_module.html) module:

        - ansible.builtin.copy:
           src: files/jolokia-war-1.7.1.war
           dest: "{{ jws_home }}/tomcat/webapps/"

- To deploy an application when the `.war` file already exists on the target host, use the [copy:](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/copy_module.html) module with the `remote_src` parameter:

        - ansible.builtin.copy:
           src: files/jolokia-war-1.7.1.war
           dest: "{{ jws_home }}/tomcat/webapps/"
           remote_src: True

- To deploy an application by using a symbolic link or hard link to the `.war` file, which avoids duplicating the file, use the [file:](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/file_module.html) module:

        - ansible.builtin.file:
            src: /apps/jolokia-war-1.7.1.war
            dest: "{{ jws_home }}/tomcat/webapps/jolokia-war-1.7.1.war"
            state: link

## Running the playbook

After you define the appropriate variables and settings, you can run the playbook on your Ansible control node to begin the automated installation process. Ansible supports various ways to run the playbook.

For example:

- To run the playbook as the root user with a secure shell (SSH) key:

    ```
    $ ansible-playbook -i hosts playbooks/playbook.yml
    ```

- To run the playbook as the root user with a password:

    ```
    $ ansible-playbook -i hosts playbooks/playbook.yml --ask-pass
    ```

- To run the playbook as a user with sudo privileges and a password:

    ```
    $ ansible-playbook -i hosts playbooks/playbook.yml --ask-pass --ask-become-pass
    ```

- To run the playbook as a user with sudo privileges, an SSH key, and a sudo password:

    ```
    $ ansible-playbook -i hosts playbooks/playbook.yml --ask-become-pass
    ```

- To run the playbook as a user with sudo privileges and an SSH key but without a sudo password:

    ```
    $ ansible-playbook -i hosts playbooks/playbook.yml --ask-become-pass
    ```

Ensure that the playbook runs successfully without any errors.


## Support

<!--start support -->
For Red Hat customers, this collection is released as the [Red Hat Ansible certified content collection for JBoss Web Server](https://console.redhat.com/ansible/automation-hub/repo/published/redhat/jws) with [Production Support](https://access.redhat.com/support/offerings/production).

If you have any issues or questions related to this collection, please contact <Ansible-middleware-core@redhat.com> or open an issue at https://github.com/ansible-middleware/jws/issues.

For more information about using this collection, see the [Product Documentation for Red Hat JBoss Web Server](https://access.redhat.com/documentation/en-us/red_hat_jboss_web_server/6.0).
<!--end support -->


## License

Apache License v2.0 or later

See [LICENSE](LICENSE) to view the full text.
