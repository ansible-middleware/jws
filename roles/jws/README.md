middleware_automation.jws.jws
==================================================

This role contains the ansible playbook to setup JWS.


Dependency
--------------
- middleware_automation.redhat_csp_download
    - This collection is required to download resources from RedHat Customer Portal.
    - Documentation to collection can be found at <https://github.com/ansible-middleware/redhat-csp-download>


Versions
--------

| JWS VERSION | Release Date      | Tomcat Version | Native Version | Notes           |
|:------------|:------------------|:---------------|:---------------|:----------------|
|`5.6.0`      |November 30, 2021  |`9.0.50`        | `1.2.30`       |[Release Notes](https://access.redhat.com/documentation/en-us/red_hat_jboss_web_server/5.6/html/red_hat_jboss_web_server_5.6_release_notes/index)|
|`5.5.0`      |June 29, 2021      |`9.0.43`        | `1.2.26`       |[Release Notes](https://access.redhat.com/documentation/en-us/red_hat_jboss_web_server/5.5/html/red_hat_jboss_web_server_5.5_release_notes/index)|
|`5.4.0`      |November 23, 2020  |`9.0.36`        | `1.2.25`       |[Release Notes](https://access.redhat.com/documentation/en-us/red_hat_jboss_web_server/5.4/html/red_hat_jboss_web_server_5.4_release_notes/index)|
|`5.3.0`      |April 21, 2020     |`9.0.30`        | `1.2.23`       |[Release Notes](https://access.redhat.com/documentation/en-us/red_hat_jboss_web_server/5.3/html/red_hat_jboss_web_server_5.3_release_notes/index)|
|`5.2.0`      |November 20, 2019  |`9.0.21`        | `1.2.21`       |[Release Notes](https://access.redhat.com/documentation/en-us/red_hat_jboss_web_server/5.2/html/red_hat_jboss_web_server_5.2_release_notes/index)|
|`5.1.0`      |May 08, 2019       |`9.0.7`         | `1.2.17`       |[Release Notes](https://access.redhat.com/documentation/en-us/red_hat_jboss_web_server/5.1/html/red_hat_jboss_web_server_5.1_release_notes/index)|


Patching
--------

When variable `jws_apply_patches` is `True` (default: `False`), the role will automatically apply the latest cumulative patch for the selected base version.

| JWS VERSION | Release Date      | JWS LATEST CP | Notes           |
|:------------|:------------------|:--------------|:----------------|
|`5.6.0`      |April 29, 2022     |`5.6.2`        |[Release Notes](https://access.redhat.com/documentation/en-us/red_hat_jboss_web_server/5.6/html/red_hat_jboss_web_server_5.6_service_pack_2_release_notes/index)|
|`5.5.0`      |October 06, 2021   |`5.5.1`        |[Release Notes](https://access.redhat.com/documentation/en-us/red_hat_jboss_web_server/5.5/html/red_hat_jboss_web_server_5.5_service_pack_1_release_notes/index)|
|`5.4.0`      |April 14, 2021     |`5.4.2`        |[Release Notes](https://access.redhat.com/documentation/en-us/red_hat_jboss_web_server/5.4/html/red_hat_jboss_web_server_5.4_service_pack_2_release_notes/index)|
|`5.3.0`      |August 04, 2020    |`5.3.2`        |[Release Notes](https://access.redhat.com/documentation/en-us/red_hat_jboss_web_server/5.3/html/red_hat_jboss_web_server_5.3_service_pack_2_release_notes/index)|


Refer to [JBoss Web Server Component Details](https://access.redhat.com/articles/111723) for additional details.

<!--start argument_specs-->
Role Defaults
-------------

* Download and install parameters

| Variable | Description | Default |
|:---------|:------------|:--------|
|`jws_install_method`| Installation method, allowed values: `['local_zipfiles','rhn_zipfiles','zipfiles','rpm']` | `local_zipfiles` |
|`tomcat_install_dir`| Installation path for JWS/tomcat | `/opt` |
|`tomcat_rpm`| Installation RPM version | `jws5` |
|`tomcat_zipfile_home`| Default installation path for tomcat archives | `{{ tomcat_install_dir }}/apache-tomcat-{{ tomcat_version }}` |
|`jws_rhn_base_url`| Customer Portal Base URL for archive download | `https://access.redhat.com/jbossnetwork/restricted/softwareDownload.html?softwareId=` |
|`jws_version`| Version of JWS to install | `5.6.0` |
|`jws_apply_patches`| Install JWS most recent cumulative patch for requested version | `False` |
|`jws_selinux_enabled` | Enable selinux policy enforcement for JWS | `True` |
|`jws_home`| Default installation path for JWS archives | `{{ tomcat_install_dir }}/jws-{{ jws_version.split('.')[0] }}.{{ jws_version.split('.')[1] }}/tomcat` |
|`tomcat_home`| Target installation directory, defaulting to tomcat or JWS archive path, or can be overridden | `{{ jws_home if jws_install_method == 'rhn_zipfiles' else tomcat_zipfile_home }}` |
|`tomcat_user`| posix user account for service | `tomcat` |
|`tomcat_uid`| posix UID user account for service | `tomcat` |
|`tomcat_group`| posix group for service | `tomcat` |
|`tomcat_gid`| posix GID user account for service | `tomcat` |
|`tomcat_zipfile`| Tomcat archive filename | `apache-tomcat-{{ tomcat_version }}.zip` |
|`tomcat_zipfile_url`| Alternative download URL | `https://dlcdn.apache.org/tomcat/tomcat-{{ tomcat_version.split(".")[0] }}/v{{ tomcat_version }}/bin/{{ tomcat_zipfile }}` |
|`tomcat_native`| Install native bits; provide a zipfile path below with tomcat, while on JWS it will be interpolated from version | `False` |
|`tomcat_native_zipfile`| Tomcat native binaries archive filename | `''` |
|`tomcat_version`| Tomcat version to download | `9.0.60` |
|`tomcat_force_install`| Whether to stop any running tomcat process and continue installation | `false` |


* Service configuration

| Variable | Description | Default |
|:---------|:------------|:--------|
|`tomcat_apps_to_remove`| Comma separated list of apps to undeploy | `docs,ROOT,examples` |
|`tomcat_catalina_base`| Tomcat catalina base env variable | `{{ lookup('env','CATALINA_BASE') }}` |
|`tomcat_conf_properties`| Path for tomcat configuration | `./conf/catalina.properties` |
|`tomcat_conf_policy`| Path for tomcat policy configuration | `./conf/catalina.policy` |
|`tomcat_conf_loggging`| Path for logging configuration | `./conf/logging.properties` |
|`tomcat_conf_context`| Relative path to context.xml | `./conf/context.xml` |
|`tomcat_conf_server`| Relative path to server.xml | `./conf/server.xml` |
|`tomcat_conf_web`| Relative path to web.xml | `./conf/web.xml` |
|`tomcat_conf_templates_context`| Template to use for context.xml | `templates/context.xml.j2` |
|`tomcat_conf_templates_server`| Template to use for server.xml | `templates/server.xml.j2` |
|`tomcat_conf_templates_web`| Template to use for web.xml | `templates/web.xml.j2` |
|`tomcat_conf_templates_catalina_properties`| Template to use for catalina.properties | `templates/catalina.properties.j2` |
|`tomcat_shutdown_port`| Tomcat shutdown port | `8005` |
|`tomcat_listen_http_port`| Tomcat http listen port | `8080` |
|`tomcat_listen_http_bind_address`| Service bind address | `localhost` |
|`tomcat_listen_http_enabled`| Enable listening on http port | `yes` |
|`tomcat_listen_https_port`| Enable listening on https port | `8443` |
|`tomcat_listen_https_bind_address`| Bind address for https | `::1` |
|`tomcat_listen_https_enabled`| Enable listening on https port | `false` |
|`tomcat_listen_ajp_enabled`| Enable listening on ajp port | `False` |
|`tomcat_listen_ajp_address`| Bind address for ajp | `::1` |
|`tomcat_listen_ajp_port`| Tomcat ajp listen port | `8009` |
|`tomcat_listen_ajp_secret_required`| Enable loading secret from vault | `True` |
|`tomcat_listen_ajp_secret`| Passphrase for vault secret | `secret` |
|`tomcat_vault_name`| vault keystore filename, made available in playbook files lookup paths | `vault.keystore` |
|`tomcat_vault_enabled`| Enable value | `False` |
|`tomcat_vault_alias`| Alias for loading from vault | `my_vault` |
|`tomcat_vault_storepass`| Tomcat keystore password | `123456` |
|`tomcat_vault_iteration`| Number of iteration for vault encryption | `44` |
|`tomcat_vault_salt`| Salt for encrypting tomcat vault | `1234abcd` |
|`tomcat_vault_properties`| vault.properties filename, made available in playbook files lookup paths | `vault.properties` |
|`tomcat_vault_data`| vault.data filename, made available in playbook files lookup paths | `VAULT.dat` |
|`tomcat_modcluster_enable`| Enable mod_cluster module | `False` |
|`tomcat_modcluster_ip`| Bind address for mod_cluster | `127.0.0.1` |
|`tomcat_modcluster_port`| mod_cluster port | `6666` |
|`tomcat_modcluster_connector_port`| mod_cluster connector port | `6666` |
|`tomcat_modcluster_advertise`| Enable mod_cluster advertising | `false` |
|`tomcat_modcluster_sticky_session`| Enable mod_cluster sticky sessions | `true` |
|`tomcat_modcluster_sticky_session_force`| Force use of sticky sessions | `false` |
|`tomcat_modcluster_sticky_session_remove`| Remove sticky session from cookies | `true` |
|`tomcat_systemd_enabled`| Enable tomcat systemd unit | `False` |
|`tomcat_systemd_script_interpreter`| Interpreter for systemd unit | `bash` |
|`tomcat_systemd_script_shebang`| Customize sysVinit script shebang | `#!/bin/{{ tomcat_systemd_script_interpreter }}` |
|`tomcat_service_name`| Name for the systemd unit | `tomcat` |
|`tomcat_service_conf`| Absolute path to tomcat.conf | `{{ tomcat_home }}/conf/tomcat.conf` |
|`tomcat_service_script`| Tomcat sysVinit script | `{{ tomcat_home }}/bin/systemd-service.sh` |
|`tomcat_service_systemd`| Tomcat systemd unit | `/usr/lib/systemd/system/tomcat.service` |
|`tomcat_service_systemd_pidfile`| Absolute path to tomcat PIDfile | `{{ tomcat_home }}/tomcat.pidfile` |
|`tomcat_service_systemd_type`| Systemd unit type | `simple` |


Role Variables
--------------

| Variable | Description | Required |
|:---------|:------------|:---------|
|`tomcat_java_version`| Version of java openjdk RPM to install for Tomcat, by default nothing is installed | `No` |
|`tomcat_java_home`| JAVA_HOME to use for starting Tomcat, inferred from `tomcat_java_version` if used, `/usr/lib/jvm/java` otherwise | `No` |
<!--end argument_specs-->


Example Playbook
----------------
```
---
- hosts: all
  tasks:
    - name: "Include tomcat"
      include_role:
        name: "jws"
```

