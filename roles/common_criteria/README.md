redhat.jws.common_criteria
==================================================

This role will handle common criteria certification.

Role Variables
--------------

Required:

- display_xpath_res_override: default vaule is 'False'
- display_directory_stats_override: default vaule is 'False'
- tomcat_owner_override: default vaule is 'tomcat'
- tomcat_group_override: default vaule is 'tomcat'

Example Playbooks
-----------------

'''
---
- hosts: all
  gather_facts: false
  roles:
    - role: common_criteria
'''