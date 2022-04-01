middleware_automation.jws.jws_validation
========================================

This role provides validation for deployment and setup of JWS.


<!--start argument_specs-->
Role Defaults
-------------

| Variable | Description | Default |
|:---------|:------------|:--------|
|`tomcat_user`| posix user account for service | `tomcat` |
|`tomcat_uid`| posix UID user account for service | `tomcat` |
|`tomcat_group`| posix group for service | `tomcat` |
|`tomcat_gid`| posix GID user account for service | `tomcat` |
|`tomcat_service_name`| Name for the systemd unit | `tomcat` |
|`tomcat_listen_https_port`| Enable listening on https port | `8443` |
|`tomcat_listen_https_bind_address`| Bind address for https | `::1` |
|`tomcat_listen_ajp_enabled`| Enable listening on ajp port | `False` |
|`tomcat_listen_ajp_address`| Bind address for ajp | `::1` |
|`tomcat_listen_ajp_port`| Tomcat ajp listen port | `8009` |
|`tomcat_catalina_logfile`| Path to tomcat logfile | `/opt/apache-tomcat-9.0.50/logs/catalina.out` |

Role Variables
--------------

* No required variables.
<!--end argument_specs-->

