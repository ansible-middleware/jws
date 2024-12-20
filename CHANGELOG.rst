==============
Release Notes
==============

.. contents:: Topics

This changelog describes changes after version 0.0.3.

v2.1.0
======

Major Changes
-------------

- AMW-196 Usage of become inside the jws role `#272 <https://github.com/ansible-middleware/jws/pull/272>`_
- Update default for ``jws_native`` `#269 <https://github.com/ansible-middleware/jws/pull/269>`_
- fix typo in variable name `#271 <https://github.com/ansible-middleware/jws/pull/271>`_

Minor Changes
-------------

- Add jws_enable var for consistency with other runtime collection `#277 <https://github.com/ansible-middleware/jws/pull/277>`_
- Make playbook ``become`` unnecessary `#274 <https://github.com/ansible-middleware/jws/pull/274>`_
- Update collection to use Ansible 2.16 as a minimum requirement `#276 <https://github.com/ansible-middleware/jws/pull/276>`_

v2.0.0
======

Major Changes
-------------

- Bump collection major version, set default JWS version to 6.0 `#265 <https://github.com/ansible-middleware/jws/pull/265>`_

Minor Changes
-------------

- Allow automatic recognition of Java `#259 <https://github.com/ansible-middleware/jws/pull/259>`_

Bugfixes
--------

- Fix "undefined patch_bundle" when ``jws_offline_install`` and ``jws_apply_cp`` `#263 <https://github.com/ansible-middleware/jws/pull/263>`_
- Fix RPM installations have home directory incorrectly set `#266 <https://github.com/ansible-middleware/jws/pull/266>`_
- Fix incorrect jws tomcat_vault rpm version `#267 <https://github.com/ansible-middleware/jws/pull/267>`_
- Fix placeholder in selinux policy file contexts `#268 <https://github.com/ansible-middleware/jws/pull/268>`_
- Fix usage of bare variables in assert conditions `#260 <https://github.com/ansible-middleware/jws/pull/260>`_
- Make system ``tomcat`` account only be in ``tomcat`` group `#264 <https://github.com/ansible-middleware/jws/pull/264>`_
- Set minimum version of ansible-core >=2.14 `#261 <https://github.com/ansible-middleware/jws/pull/261>`_

v1.2.5
======

Minor Changes
-------------

- Handle result of allowing port if not already authorized `#250 <https://github.com/ansible-middleware/jws/pull/250>`_
- Several enhacements on the rpm install `#249 <https://github.com/ansible-middleware/jws/pull/249>`_
- Update templates for JWS5/JWS6 `#254 <https://github.com/ansible-middleware/jws/pull/254>`_
- add offline_install parameters, move file checks up when offline `#248 <https://github.com/ansible-middleware/jws/pull/248>`_

Bugfixes
--------

- Update selinux tasks for jws6 `#253 <https://github.com/ansible-middleware/jws/pull/253>`_

v1.2.4
======

Minor Changes
-------------

- Allow controller down without sudo and rm deps to seport `#237 <https://github.com/ansible-middleware/jws/pull/237>`_
- Download install files via JBossNetwork API  `#229 <https://github.com/ansible-middleware/jws/pull/229>`_
- Remove unused deps to community.general `#227 <https://github.com/ansible-middleware/jws/pull/227>`_
- Tc10 `#232 <https://github.com/ansible-middleware/jws/pull/232>`_
- Update RHN Ids to add latest CP `#220 <https://github.com/ansible-middleware/jws/pull/220>`_
- Update documentation default to 5.7 `#234 <https://github.com/ansible-middleware/jws/pull/234>`_
- jws: add firewalld configuration `#226 <https://github.com/ansible-middleware/jws/pull/226>`_

Bugfixes
--------

- Set correct exit code in systemd unit `#240 <https://github.com/ansible-middleware/jws/pull/240>`_
- Use alternatives, not rpm, to determine java_home `#241 <https://github.com/ansible-middleware/jws/pull/241>`_
- jws_validation: fix typo in rpm validation `#218 <https://github.com/ansible-middleware/jws/pull/218>`_
- workaround java11 buzilla #2224411 `#233 <https://github.com/ansible-middleware/jws/pull/233>`_

v1.2.3
======

Minor Changes
-------------

- Clarify and separate dev setup from installation `#212 <https://github.com/ansible-middleware/jws/pull/212>`_

Bugfixes
--------

- Preinstalled Java `#208 <https://github.com/ansible-middleware/jws/pull/208>`_
- Reorganize selinux http port labeling task `#211 <https://github.com/ansible-middleware/jws/pull/211>`_
- Revert selinux policy postinstall filenames `#210 <https://github.com/ansible-middleware/jws/pull/210>`_

v1.2.2
======

Minor Changes
-------------

- Add 5.7 release to rhn_ids `#205 <https://github.com/ansible-middleware/jws/pull/205>`_
- Add variable to config offline/download path on controller `#191 <https://github.com/ansible-middleware/jws/pull/191>`_
- jws: ensure default server.xml.j2 uses the recommended https config `#196 <https://github.com/ansible-middleware/jws/pull/196>`_

v1.2.0
======

Major Changes
-------------

- Reduce install methods to either 'zipfiles' or 'rpm' `#172 <https://github.com/ansible-middleware/jws/pull/172>`_
- Refactor and cleanup around tomcat_vault feature `#179 <https://github.com/ansible-middleware/jws/pull/179>`_
- Rename vars `#154 <https://github.com/ansible-middleware/jws/pull/154>`_

Minor Changes
-------------

- Conditionally install openssl and apr, only when tomcat-native is installed `#159 <https://github.com/ansible-middleware/jws/pull/159>`_
- Name all tasks `#133 <https://github.com/ansible-middleware/jws/pull/133>`_
- Remove native tests and ensure that native zipfile existence is only checked if native is on `#146 <https://github.com/ansible-middleware/jws/pull/146>`_
- Update modcluster connector port default `#169 <https://github.com/ansible-middleware/jws/pull/169>`_
- Use explicit include_role: in playbooks `#148 <https://github.com/ansible-middleware/jws/pull/148>`_

Bugfixes
--------

- Fix JWS native archive installation `#132 <https://github.com/ansible-middleware/jws/pull/132>`_

v1.1.1
======

Minor Changes
-------------

- Fix string mismatch with groupinstall `#173 <https://github.com/ansible-middleware/jws/pull/173>`_
- jws: only removes examples webapps by default. `#175 <https://github.com/ansible-middleware/jws/pull/175>`_

Bugfixes
--------

- Ensure tc_vault pkgs are installed if install_method is rpm `#178 <https://github.com/ansible-middleware/jws/pull/178>`_
- jws: set 0640 instead of 0600 for configuration files `#181 <https://github.com/ansible-middleware/jws/pull/181>`_

v1.1.0
======

Major Changes
-------------

- Provide uninstall feature `#68 <https://github.com/ansible-middleware/jws/pull/68>`_

Minor Changes
-------------

- Add custom url download and selinux for jws `#43 <https://github.com/ansible-middleware/jws/pull/43>`_
- Allow overriding tomcat user uid and gid `#52 <https://github.com/ansible-middleware/jws/pull/52>`_
- Apply latest JWS cumulative patch when ``jws_apply_patches`` is True `#94 <https://github.com/ansible-middleware/jws/pull/94>`_
- Fix the name of the tarball generated by the collection build step `#76 <https://github.com/ansible-middleware/jws/pull/76>`_
- If another tomcat is found running, fail fast or allow to continue with ``jws_force_install`` `#80 <https://github.com/ansible-middleware/jws/pull/80>`_
- Populate JWS version/patch metadata, update docs `#110 <https://github.com/ansible-middleware/jws/pull/110>`_
- Replace RHN url defaults with base URL and mapped version-productID `#77 <https://github.com/ansible-middleware/jws/pull/77>`_
- Update playbook to utilize variable defaults `#89 <https://github.com/ansible-middleware/jws/pull/89>`_

Breaking Changes / Porting Guide
--------------------------------

- Rename variables to be consistent `#117 <https://github.com/ansible-middleware/jws/pull/117>`_

Bugfixes
--------

- Adjustments to the apply_cp.yml logic `#106 <https://github.com/ansible-middleware/jws/pull/106>`_
- Avoid failure when ``jws_apply_patches`` is true and no CP is available `#118 <https://github.com/ansible-middleware/jws/pull/118>`_
- Ensure JAVA_HOME is set to installed JVM rpm, or allow to override `#101 <https://github.com/ansible-middleware/jws/pull/101>`_
- Ensure tomcat native installs and starts correctly `#120 <https://github.com/ansible-middleware/jws/pull/120>`_
- JWS-2417: Remove undefined executor `#54 <https://github.com/ansible-middleware/jws/pull/54>`_
- Make selinux policy for JWS optional like the zip installation docs suggest it is `#112 <https://github.com/ansible-middleware/jws/pull/112>`_
- Missing required variables to enable HTTPS `#49 <https://github.com/ansible-middleware/jws/pull/49>`_
- The JWS installation option should allow you to exclude natives `#97 <https://github.com/ansible-middleware/jws/pull/97>`_
- ``jws_apply_patches`` also installs native CP when available `#121 <https://github.com/ansible-middleware/jws/pull/121>`_
- fix: tomcat.user owning existing directories `#100 <https://github.com/ansible-middleware/jws/pull/100>`_
- selinux policy allows tomcat to listen to non-default ports `#119 <https://github.com/ansible-middleware/jws/pull/119>`_

v1.0.0
======

Release Summary
---------------

This is the first stable release of the ``middleware_automation.jws`` collection.
