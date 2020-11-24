ReadMe
====

This Ansible project contains to roles, one to install Tomcat Apache and the other one to verify that the setup respects all the Common Criteria security recommendations. This later role can be used on any existing installation of Tomcat. The only information needed is that the env var CATALINA_HOME is properly set (also CATALINA_BASE is required).

By default, the installation role will set up Apache Tomcat in the /opt directory.

How to run the playbook?
----

    $ export CATALINA_HOME=/opt/apache-tomcat-9.0.40/
    $ ansible-playbook playbook.yml


