middleware_automation.jws.jws
==================================================

This role contains the ansible playbook to set up JWS.

Dependencies

------------

The roles depends on:

* [middleware_automation.common](https://github.com/ansible-middleware/common)
* [ansible.posix](https://docs.ansible.com/ansible/latest/collections/ansible/posix/index.html)


Versions
--------

| JWS VERSION | Release Date      | Tomcat Version | Native Version | Notes                                                                                                                                             |
|:------------|:------------------|:---------------|:---------------|:--------------------------------------------------------------------------------------------------------------------------------------------------|
| `6.0.0`     | October 31, 2023  | `10.1.8`       | `1.2.36`       | [Release Notes](https://access.redhat.com/documentation/en-us/red_hat_jboss_web_server/6.0/html/red_hat_jboss_web_server_6.0_release_notes/index) |
| `5.8.0`     | May 7, 2024       | `9.0.87`       | `1.2.31`       | [Release Notes](https://access.redhat.com/documentation/en-us/red_hat_jboss_web_server/5.8/html/red_hat_jboss_web_server_5.8_release_notes/index) |
| `5.7.0`     | November 2, 2022  | `9.0.62`       | `1.2.31`       | [Release Notes](https://access.redhat.com/documentation/en-us/red_hat_jboss_web_server/5.7/html/red_hat_jboss_web_server_5.7_release_notes/index) |
| `5.6.0`     | November 30, 2021 | `9.0.50`       | `1.2.30`       | [Release Notes](https://access.redhat.com/documentation/en-us/red_hat_jboss_web_server/5.6/html/red_hat_jboss_web_server_5.6_release_notes/index) |
| `5.5.0`     | June 29, 2021     | `9.0.43`       | `1.2.26`       | [Release Notes](https://access.redhat.com/documentation/en-us/red_hat_jboss_web_server/5.5/html/red_hat_jboss_web_server_5.5_release_notes/index) |
| `5.4.0`     | November 23, 2020 | `9.0.36`       | `1.2.25`       | [Release Notes](https://access.redhat.com/documentation/en-us/red_hat_jboss_web_server/5.4/html/red_hat_jboss_web_server_5.4_release_notes/index) |
| `5.3.0`     | April 21, 2020    | `9.0.30`       | `1.2.23`       | [Release Notes](https://access.redhat.com/documentation/en-us/red_hat_jboss_web_server/5.3/html/red_hat_jboss_web_server_5.3_release_notes/index) |
| `5.2.0`     | November 20, 2019 | `9.0.21`       | `1.2.21`       | [Release Notes](https://access.redhat.com/documentation/en-us/red_hat_jboss_web_server/5.2/html/red_hat_jboss_web_server_5.2_release_notes/index) |
| `5.1.0`     | May 08, 2019      | `9.0.7`        | `1.2.17`       | [Release Notes](https://access.redhat.com/documentation/en-us/red_hat_jboss_web_server/5.1/html/red_hat_jboss_web_server_5.1_release_notes/index) |

For further information: [JBoss Web Server Component Details](https://access.redhat.com/articles/111723)


<!--start argument_specs-->
Role Defaults
-------------

### Download and install parameters

| Variable                   | Description                                                                                                      | Default                                                                                            |
|:---------------------------|:-----------------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------|
| `jws_install_method`       | Installation method, allowed values: `['zipfiles','rpm']`                                                        | `zipfiles`                                                                                         |
| `jws_install_dir`          | Installation path for JWS/tomcat                                                                                 | `/opt`                                                                                             |
| `jws_rpm`                  | Installation RPM version                                                                                         | `jws6`                                                                                             |
| `jws_version`              | Version of JWS to install                                                                                        | `6.0.0`                                                                                            |
| `jws_apply_patches`        | Install JWS most recent cumulative patch for requested version                                                   | `False`                                                                                            |
| `jws_selinux_enabled`      | Enable selinux policy enforcement for JWS                                                                        | `True`                                                                                             |
| `jws_home`                 | Default installation path for JWS archives                                                                       | `{{ jws_install_dir }}/jws-{{ jws_version.split('.')[0] }}.{{ jws_version.split('.')[1] }}/tomcat` |
| `jws_user`                 | posix user account for service                                                                                   | `tomcat`                                                                                           |
| `jws_uid`                  | posix UID user account for service                                                                               | `tomcat`                                                                                           |
| `jws_group`                | posix group for service                                                                                          | `tomcat`                                                                                           |
| `jws_gid`                  | posix GID user account for service                                                                               | `tomcat`                                                                                           |
| `jws_native`               | Install native bits; provide a zipfile path below with tomcat, while on JWS it will be interpolated from version | `True`                                                                                             |
| `jws_native_zipfile`       | Tomcat native binaries archive filename                                                                          | `''`                                                                                               |
| `jws_force_install`        | Whether to stop any running tomcat process and continue installation                                             | `false`                                                                                            |
| `jws_archive_repository`   | Path local to controller for offline/download archive files                                                      | `{{ lookup('env', 'PWD') | default('/opt') }}`                                                |
| `jws_offline_install`      | Whether to perform a completely offline install                                                                  |`false`                                                                                             |
| `jws_url_download_retries` | Number of retries in case a download fails                                                                       | `5`                                                                                                |
| `jws_url_download_delay`   | Delay among two consequent download retries                                                                      | `10`                                                                                               |

### Service configuration

| Variable                                 | Description                              | Default                                       |
|:-----------------------------------------|:-----------------------------------------|:----------------------------------------------|
| `jws_apps_to_remove`                     | Comma separated list of apps to undeploy | `docs,ROOT,examples`                          |
| `jws_catalina_base`                      | Tomcat catalina base env variable        | `{{ lookup('env','CATALINA_BASE') }}`         |
| `jws_conf_properties`                    | Path for tomcat configuration            | `./conf/catalina.properties`                  |
| `jws_conf_policy`                        | Path for tomcat policy configuration     | `./conf/catalina.policy`                      |
| `jws_conf_logging`                       | Path for logging configuration           | `./conf/logging.properties`                   |
| `jws_conf_context`                       | Relative path to context.xml             | `./conf/context.xml`                          |
| `jws_conf_server`                        | Relative path to server.xml              | `./conf/server.xml`                           |
| `jws_conf_web`                           | Relative path to web.xml                 | `./conf/web.xml`                              |
| `jws_conf_templates_context`             | Template to use for context.xml          | `templates/context.xml.j2`                    |
| `jws_conf_templates_server`              | Template to use for server.xml           | `templates/server.xml.j2`                     |
| `jws_conf_templates_web`                 | Template to use for web.xml              | `templates/web.xml.j2`                        |
| `jws_conf_templates_catalina_properties` | Template to use for catalina.properties  | `templates/catalina.properties.j2`            |
| `jws_shutdown_port`                      | Tomcat shutdown port                     | `8005`                                        |
| `jws_listen_http_port`                   | Tomcat http listen port                  | `8080`                                        |
| `jws_listen_http_bind_address`           | Service bind address                     | `localhost`                                   |
| `jws_listen_http_enabled`                | Enable listening on http port            | `True`                                         |
| `jws_listen_https_port`                  | Enable listening on https port           | `8443`                                        |
| `jws_listen_https_bind_address`          | Bind address for https                   | `::1`                                         |
| `jws_listen_https_enabled`               | Enable listening on https port           | `false`                                       |
| `jws_listen_ajp_enabled`                 | Enable listening on ajp port             | `False`                                       |
| `jws_listen_ajp_address`                 | Bind address for ajp                     | `::1`                                         |
| `jws_listen_ajp_port`                    | Tomcat ajp listen port                   | `8009`                                        |
| `jws_listen_ajp_secret_required`         | Enable loading secret from vault         | `True`                                        |
| `jws_listen_ajp_secret`                  | Passphrase for vault secret              | `secret`                                      |
| `jws_systemd_enabled`                    | Enable tomcat systemd unit               | `False`                                       |
| `jws_systemd_script_interpreter`         | Interpreter for systemd unit             | `bash`                                        |
| `jws_systemd_script_shebang`             | Customize sysVinit script shebang        | `#!/bin/{{ jws_systemd_script_interpreter }}` |
| `jws_service_name`                       | Name for the systemd unit                | `tomcat`                                      |
| `jws_service_conf`                       | Absolute path to tomcat.conf             | `{{ jws_home }}/conf/tomcat.conf`             |
| `jws_service_script`                     | Tomcat sysVinit script                   | `{{ jws_home }}/bin/systemd-service.sh`       |
| `jws_service_systemd`                    | Tomcat systemd unit                      | `/usr/lib/systemd/system/tomcat.service`      |
| `jws_service_pidfile`                    | Absolute path to tomcat PIDfile          | `{{ jws_home }}/tomcat.pidfile`               |
| `jws_service_systemd_type`               | Systemd unit type                        | `simple`                                      |

### tomcat-vault configuration

| Variable                      | Description                                                              | Default            |
|:------------------------------|:-------------------------------------------------------------------------|:-------------------|
| `jws_tomcat_vault_keystore`   | vault keystore filename, made available in playbook files lookup paths   | `vault.keystore`   |
| `jws_tomcat_vault_enabled`    | Enable value                                                             | `False`            |
| `jws_tomcat_vault_alias`      | Alias for loading from vault                                             | `my_vault`         |
| `jws_tomcat_vault_storepass`  | Tomcat keystore password                                                 | `123456`           |
| `jws_tomcat_vault_iteration`  | Number of iteration for vault encryption                                 | `44`               |
| `jws_tomcat_vault_salt`       | Salt for encrypting tomcat vault                                         | `1234abcd`         |
| `jws_tomcat_vault_properties` | vault.properties filename, made available in playbook files lookup paths | `vault.properties` |
| `jws_tomcat_vault_data`       | vault.data filename, made available in playbook files lookup paths       | `VAULT.dat`        |

### mod_cluster configuration

| Variable                               | Description                        | Default     |
|:---------------------------------------|:-----------------------------------|:------------|
| `jws_modcluster_enable`                | Enable mod_cluster module          | `False`     |
| `jws_modcluster_ip`                    | Bind address for mod_cluster       | `127.0.0.1` |
| `jws_modcluster_port`                  | mod_cluster port                   | `6666`      |
| `jws_modcluster_connector_port`        | mod_cluster connector port         | `8080`      |
| `jws_modcluster_advertise`             | Enable mod_cluster advertising     | `false`     |
| `jws_modcluster_sticky_session`        | Enable mod_cluster sticky sessions | `true`      |
| `jws_modcluster_sticky_session_force`  | Force use of sticky sessions       | `false`     |
| `jws_modcluster_sticky_session_remove` | Remove sticky session from cookies | `true`      |

### HTTPS configuration

| Variable                              | Description                                   | Default                 |
|:--------------------------------------|:----------------------------------------------|:------------------------|
| `jws_listen_https_enabled`            | Enable HTTPS connector                        | `False`                 |
| `jws_listen_https_port`               | Tomcat HTTPS listen port                      | `'8443'`                |
| `jws_listen_https_bind_address`       | Service HTTPS bind address                    | `'localhost'`           |
| `jws_listen_https_servername`         | Servername associated to the HTTPS connector  | `'My Server'`           |
| `jws_listen_https_threads_max`        | Max number of threads for the HTTPS connector | `150`                   |
| `jws_listen_https_connection_timeout` | HTTPS connector timeout                       | `6000`                  |
| `jws_listen_https_headers_size`       | HTTPS connector HTTPS header size             | `8192`                  |
| `jws_listen_https_keystore_file`      | Path to keystore file used by HTTPS connector | `/etc/ssl/keystore.jks` |
| `jws_listen_https_keystore_password`  | Password to keystore file                     | `changeit`              |
| `jws_listen_https_client_auth`        | Request certificate from client               | `false`                 |

Role Variables
--------------

| Variable           | Description                                                                        |
|:-------------------|:-----------------------------------------------------------------------------------|
| `jws_java_version` | Version of java openjdk RPM to install for Tomcat, by default nothing is installed |
| `jws_java_home`    | Path to the JAVA\_HOME to be used by the server                                    |
<!--end argument_specs-->

**NOTE:** You need to provide either `jws_java_version` or `jws_java_home` value. `jws_java_version` value can be 11, 17 or 21 (for RHEL 10).


Example Playbook
----------------

### Basic Installation
```yaml
---
- name: Install JWS with basic configuration
  hosts: all
  vars:
    jws_java_version: 21
    jws_listen_http_bind_address: 127.0.0.1
    jws_systemd_enabled: True
    jws_service_systemd_type: forking
    jws_selinux_enabled: False
  roles:
    - middleware_automation.jws.jws
```

### RPM Installation
```yaml
---
- name: Install JWS using RPM method
  hosts: webservers
  vars:
    jws_install_method: rpm
    jws_java_version: 21
    jws_systemd_enabled: True
  roles:
    - middleware_automation.jws.jws
```

### With HTTPS Enabled
```yaml
---
- name: Configure JWS with HTTPS
  hosts: jws_servers
  vars:
    jws_java_version: 21
    jws_listen_https_enabled: True
    jws_listen_https_port: 8443
    jws_systemd_enabled: True
  roles:
    - middleware_automation.jws.jws
```

### With mod_cluster
```yaml
---
- name: Configure JWS with mod_cluster
  hosts: jws_cluster_nodes
  vars:
    jws_java_version: 21
    jws_modcluster_enable: True
    jws_modcluster_port: 6666
    jws_systemd_enabled: True
  roles:
    - middleware_automation.jws.jws
```

### Offline Installation
```yaml
---
- name: Offline JWS installation
  hosts: all
  vars:
    jws_offline_install: True
    jws_archive_repository: "/opt/jws-archives"
    jws_java_home: "/usr/lib/jvm/java-11-openjdk"
  roles:
    - middleware_automation.jws.jws
```

### Advanced Configuration with Tomcat Vault
```yaml
---
- name: JWS with Tomcat Vault and HTTPS
  hosts: secure_servers
  vars:
    jws_java_version: 21
    jws_listen_https_enabled: True
    jws_tomcat_vault_enabled: True
    jws_tomcat_vault_storepass: "my_secure_password"
    jws_systemd_enabled: True
  roles:
    - middleware_automation.jws.jws
```