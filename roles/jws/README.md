middleware_automation.jws.jws
==================================================

This role contains the ansible playbook to setup JWS.


Dependency
--------------
- middleware_automation.redhat_csp_download
    - This collection is required to download resources from RedHat Customer Portal.
    - Documentation to collection can be found at <https://github.com/ansible-middleware/redhat-csp-download>

<!--start argument_specs-->
Role Defaults
-------------


Role Variables
--------------


<!--end argument_specs-->


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

