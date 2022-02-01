redhat.jws.jws
==================================================
This role contains the ansible playbook to setup JWS.

Dependency
--------------
- middleware_automation.redhat_csp_download
    - This collection is required to download resources from RedHat Customer Portal.
    - Documentation to collection can be found at <https://github.com/ansible-middleware/redhat-csp-download>

Role Variables
--------------

- local_zipfiles: Keeping zip files local to install. This is the default methord.
- rhn_zipfiles: get zip files from redhat customer portal.
- rpm: RPM instalation
- tomcat_install_dir: Tomcat instalation directory
- rhn_username: Redhat Customer portal username
- rhn_passwrd: Redhat Customer portal password.
- override_tomcat_user: Default value is 'tomcat'
- override_tomcat_group: Default value is 'tomcat'
- tomcat_home: Default value is '/opt/jws-5.4/tomcat'
- tomcat_catalina_base: Default value is lookup('env','CATALINA_BASE')
- override_tomcat_conf_properties: Default value is './conf/catalina.properties'
- override_tomcat_conf_policyL: Default value is './conf/catalina.policy'
- override_tomcat_conf_loggging: Default value is './conf/logging.properties'
- override_tomcat_conf_context: Default value is './conf/context.xml'
- override_tomcat_conf_server: Default value is './conf/server.xml'
- override_tomcat_conf_web: Default value is './conf/web.xml'
- override_tomcat_conf_templates_context: Default value is 'templates/context.xml.j2'
- override_tomcat_conf_templates_server: Default value is 'templates/server.xml.j2'
- override_tomcat_conf_templates_web: Default value is 'templates/web.xml.j2'
- override_tomcat_shutdown_port: Default value is '8005'
- override_tomcat_listen_http_port: Default value is '8080'
- override_tomcat_listen_http_bind_address: Default address is 'localhost'
- override_tomcat_listen_http_enabled: Default value is 'yes'
- override_tomcat_listen_ajp_enabled: Default value is'False'
- override_tomcat_listen_ajp_address: Default value is '::1'
- override_tomcat_listen_ajp_port: Default value is '8009'
- override_tomcat_listen_ajp_secretRequired: Default value is 'True'
- override_tomcat_listen_ajp_secre: Default value is 'secret'
- override_tomcat_vault_name: Default value is vault.keystore'
- override_tomcat_vault_enable: Default value is 'False')
- override_tomcat_vault_alias: Default value is 'my_vault'
- override_tomcat_vault_storepass: Default value is '123456'
- override_tomcat_vault_iteration: Default value is '44'
- override_tomcat_vault_salt: Default value is '1234abcd'
- override_tomcat_vault_properties: Default value is '/conf/vault.properties'
- override_tomcat_modcluster_enable: Default value is 'False'
- override_tomcat_modcluster_ip: Default value is '127.0.0.1'
- override_tomcat_modcluster_port: Default value is '6666'
- override_tomcat_modcluster_connector_port: Default value is '6666'
- override_tomcat_modcluster_advertise: Default value is 'false'
- override_tomcat_modcluster_stickySession: Default value is 'true'
- override_tomcat_modcluster_stickySessionForce: Default value is 'false'
- override_tomcat_modcluster_stickySessionRemove: Default value is 'true'
- tomcat_systemd_enabled: Default value is 'False'
- tomcat_service_name: Default value is 'jws5-tomcat.service'
- override_tomcat_service_systemd: Default value is '/usr/lib/systemd/system/tomcat.service'

Example Playbooks
--------------
'''
---
- hosts: all
  tasks:
    - name: "Include tomcat"
      include_role:
        name: "jws"
'''

