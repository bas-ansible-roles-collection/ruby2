# Ruby 2 (`ruby2`)
 
Master: [![Build Status](https://semaphoreci.com/api/v1/bas-ansible-roles-collection/ruby2/branches/master/badge.svg)](https://semaphoreci.com/bas-ansible-roles-collection/ruby2)
Develop: [![Build Status](https://semaphoreci.com/api/v1/bas-ansible-roles-collection/ruby2/branches/develop/badge.svg)](https://semaphoreci.com/bas-ansible-roles-collection/ruby2)
 
Installs Ruby 2 language runtime and development packages
 
**Part of the BAS Ansible Role Collection (BARC)**
 
**This role uses version 0.2.0 of the BARC flavour of the BAS Base Project - Pristine**.
 
## Overview
 
* ...
 
## Quality Assurance
 
This role uses manual and automated testing to ensure its features work as advertised. 
See [here](tests/README.md) for more information.
 
## Dependencies
 
* None
 
More information on role dependencies is available in the 
[BARC General Documentation](https://antarctica.hackpad.com/BARC-Overview-and-Policies-SzcHzHvitkt#:h=Role-dependencies)
 
## Requirements
 
* None
 
More information on role requirements is available in the 
[BARC General Documentation](https://antarctica.hackpad.com/BARC-Overview-and-Policies-SzcHzHvitkt#:h=Role-requirements)
 
## Limitations
 
* Ruby *1.9* is the default Ruby version on Ubuntu Trusty where only system packages are permitted

I.e. If you are using Ubuntu Trusty and set *BARC_use_non_system_package_sources* to `false` you will get Ruby *1.9*,
not *2.0*.

This is caused by a [baffling](https://www.youtube.com/watch?v=iLeyp5QXLnk) set of dependencies for the Ubuntu Trusty 
`ruby2.0` package, and an equally [baffling](https://www.youtube.com/watch?v=iWno0BWOo14) decision not to fix this.

To try and restore some sense of sanity, this role *will* perform additional configuration steps where system packages 
are used on Ubuntu, so that `ruby` and `gem` point to their *2.0* versions (which are installed, just not the system 
default).

*This limitation is considered to be significant. A workaround is in place mitigate this limitation, pending a*
*permanent resolution by Ubuntu. Pull requests addressing this limitation will be considered.*

See [#1310292](https://bugs.launchpad.net/ubuntu/+source/ruby2.0/+bug/1310292) for upstream information.

See [BARC-86](https://jira.ceh.ac.uk/browse/BARC-86) for further details.

* The Ruby version varies depending on which operating system is used and if non-system package sources are permitted

As the package policy varies between system and non-system package sources, and between operating systems, the version 
of Ruby installed is variable between supported operating systems.

For Ubuntu machines, a non-system PPA is installed resulting in more recent versions of Ruby to be installed. On CentOS 
machines, the system default package for Ruby will always be installed as no non-system source is available.

It is a convention of BARC roles to use the latest version of packages. Where a suitable non-system package source is 
available it will be used. Otherwise system packages will be used. Suitable non-system packages require a reputable,
maintainer, typically a company or well respected individual. Where non-system packages are used, the variable 
*BARC_use_non_system_package_sources* can be set to `false` to always use system packages if this is needed.

_This limitation is **NOT** considered to be significant. Solutions will **NOT** be actively pursued._ 
_Pull requests addressing this limitation will be considered.*_

See [BARC-87](https://jira.ceh.ac.uk/browse/BARC-87) for further details.

 
More information on role limitations is available in the 
[BARC General Documentation](https://antarctica.hackpad.com/BARC-Overview-and-Policies-SzcHzHvitkt#:h=Role-limitations)
 
## Usage
 
### BARC Manifest
 
By default, BARC roles will record that they have been applied to a system, this is termed the BAS Manifest.
The specific local facts set for this role are:
 
* `ansible_local.barc_ruby2.general.role_applied` - (boolean) records whether a role has been applied
* `ansible_local.barc_ruby2.general.role_version` - (string) records the version of the role that was applied
 
Note: You **SHOULD** use this feature to determine whether this role has been applied to a system.
If you do not want these facts to be set by this role, you **MUST** skip the **BARC_SET_MANIFEST** tag.
 
More information is available in the 
[BARC General Documentation](https://antarctica.hackpad.com/BARC-Overview-and-Policies-SzcHzHvitkt#:h=Role-Manifest)

### Ruby version

Depending on the operating system used, the version of Ruby installed will different, though it will be at least Ruby 
*2.0*. The table below hopes to clarify the version you can expect:

| Operating System | Non-System Package Sources Permitted | Ruby Version | Notes  |
| ---------------- | ------------------------------------ | ------------ | ------ |
| Ubuntu           | Yes                                  | *2.3*        | -      |
| Ubuntu           | No                                   | *2.0*        | [1]    |
| CentOS           | N/A                                  | *2.0*        | [2]    |

Because the exact version installed cannot be guaranteed by this role, you should be careful if using depending on this 
role in another role or a project that relies on Ruby. If any version of Ruby 2 is acceptable this role is suitable,
however where you depend on some feature added to minor releases (e.g. *2.1*) this role is unsuitable.

This ambiguity, and the fixes needed on Ubuntu without non-system package sources, are both considered limitations.
See the *limitations* section for more information.

[1] With additional fixes to make this version the system default, see the *limitations* section for more information.

[2] There is no non-system source available for CentOS which has a more up to date version of Ruby. Therefore the 
system default package is used in all circumstances.

### Development packages

Unusually for a BARC role this role will install development libraries as well as run-time libraries for Ruby. This is 
to allow Gem's which build native extensions to be installed.

### Typical playbook
 
```yaml
---
 
- name: ...
  hosts: all
  become: yes
  vars: []
  roles:
    - bas-ansible-roles-collection.ruby2
```
 
### Tags
 
BARC roles use standardised tags to control which aspects of an environment are changed by roles.
Tasks in this role will 'tag' themselves with these tags as appropriate, typically in `tasks/main.yml`.
 
More information is available in the
[BARC General Documentation](https://antarctica.hackpad.com/BARC-Overview-and-Policies-SzcHzHvitkt#:h=Appendix-B---BARC-Standardised)
 
### Variables
 
#### *ruby2_barc_role_name*
 
* **MUST NOT** be specified
* Specifies the name of this role within the BAS Ansible Roles Collection (BARC) used for setting local facts
* See the *BARC roles manifest* section for more information
* Example: `ruby2` 
 
#### *ruby2_barc_role_version*
 
* **MUST NOT** be specified
* Specifies the name of this role within the BAS Ansible Roles Collection (BARC) used for setting local facts
* See the *BARC roles manifest* section for more information
* Example: `2.0.0` 
 
### *BARC_use_non_system_package_sources*
 
* **MAY** be specified
* Specifies whether non-system sources can be used to install packages
* Note: This variable is scoped to all other BARC roles which install packages from non-system sources
* Values MUST use one of these options, as determined by Ansible:
    * `true` 
    * `false` 
* Values **SHOULD NOT** be quoted to prevent Ansible coercing values to a string
* Where not specified, a value of true will be assumed
* Default: `true` 
 
## Developing
 
### Issue tracking
 
Issues, bugs, improvements, questions, suggestions and other tasks related to this package are managed through the 
[BAS Ansible Roles Collection](https://jira.ceh.ac.uk/projects/BARC) (BARC) project on Jira.
 
This service is currently only available to BAS or NERC staff, although external collaborators can be added on request.
See our contributing policy for more information.
 
More information is also available in the
[BARC General Documentation](https://antarctica.hackpad.com/BARC-Overview-and-Policies-SzcHzHvitkt#:h=Issue-Tracking)
 
### Source code
 
All changes should be committed, via pull request, to the canonical repository:
 
`ssh://git@stash.ceh.ac.uk:7999/barc/ROLE-NAME.git`
 
A read-only mirror of this repository is maintained on GitHub:
 
`git@github.com:bas-ansible-roles-collection/ROLE-NAME.git`
 
More information is available in the
[BARC General Documentation](https://antarctica.hackpad.com/BARC-Overview-and-Policies-SzcHzHvitkt#:h=Source-Code)
 
### Contributing policy
 
This project welcomes contributions, see `CONTRIBUTING.md` for our general policy.
 
### Release procedure
 
The general release procedure for BARC roles is available in the
[BARC General Documentation](https://antarctica.hackpad.com/BARC-Overview-and-Policies-SzcHzHvitkt#:h=Release-procedures)
 
## Licence
 
Copyright 2016 NERC BAS.
 
Unless stated otherwise, all documentation is licensed under the Open Government License - version 3.
All code is licensed under the MIT license.
 
Copies of these licenses are included within this project.
