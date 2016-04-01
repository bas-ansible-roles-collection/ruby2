# Ruby 2 (`ruby2`)

Installs Ruby 2 including development packages

**Part of the BAS Ansible Role Collection (BARC)**

## Overview

* Installs Ruby and Ruby development packages

## Quality Assurance

This role uses manual testing to ensure the features offered by this role work as advertised. 
See `tests/README.md` for more information.

## Dependencies

* None

## Requirements

* None

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
machines, the system default package for Ruby will be installed.

It is a convention of BARC roles to use the latest version of packages. Where a suitable non-system package source is 
available it will be used. Otherwise system packages will be used. Suitable non-system packages require a reputable,
maintainer, typically a company or well respected individual. Where non-system packages are used, the variable 
*BARC_use_non_system_package_sources* can be set to `false` to always use system packages if this is needed.

_This limitation is **NOT** considered to be significant. Solutions will **NOT** be actively pursued._ 
_Pull requests addressing this limitation will be considered.*_

See [BARC-87](https://jira.ceh.ac.uk/browse/BARC-87) for further details.

## Usage

### Ruby version

Depending on the operating system used, the version of Ruby installed will different, though it will be at least Ruby 
*2.0*. The table below hopes to clarify the version you can expect:

| Operating System | Non-System Package Sources Permitted | Ruby Version | Notes                                                         |
| ---------------- | ------------------------------------ | ------------ | ------------------------------------------------------------- |
| Ubuntu           | Yes                                  | *2.2*        | -                                                             |
| Ubuntu           | No                                   | *2.0*        | with additional fixes to make this version the system default |
| CentOS           | N/A                                  | *2.0*        | -                                                             |

Because the exact version installed cannot be guaranteed by this role, you should be careful if using depending on this 
role in another role or a project that relies on Ruby. If any version of Ruby 2 is acceptable this role is suitable,
however where you depend on some feature added to minor releases (e.g. *2.1*) this role is unsuitable.

This ambiguity, and the fixes needed on Ubuntu without non-system package sources, are both considered limitations.
See the *limitations* section for more information.

### Typical playbook

```yaml
---

- name: install ruby
  hosts: all
  become: yes
  vars: []
  roles:
    - bas-ansible-roles-collection.ruby2
```

### Tags

BARC roles use standardised tags to control which aspects of an environment are changed by roles. Where relevant, tags
will be applied at a role, or task(s) level, as indicated below.

This role uses the following tags, for various tasks:

* [**BARC_CONFIGURE**](https://antarctica.hackpad.com/BARC-Standardised-Tags-AviQxxiBa3y#:h=BARC_CONFIGURE)
* [**BARC_CONFIGURE_PACKAGES**](https://antarctica.hackpad.com/BARC-Standardised-Tags-AviQxxiBa3y#:h=BARC_CONFIGURE_PACKAGE)
* [**BARC_INSTALL_PACKAGES**](https://antarctica.hackpad.com/BARC-Standardised-Tags-AviQxxiBa3y#:h=BARC_INSTALL_PACKAGE)

### Variables

#### *BARC_use_non_system_package_sources*

* **MAY** be specified
* Specifies whether non-system sources can be used to install packages
* Note: This variable is scoped to all other BARC roles which install packages from non-system sources
* Values MUST use one of these options, as determined by the SSH Daemon configuration file:
    * `true`
    * `false`
* Values **SHOULD NOT** be quoted to prevent Ansible coercing values to a string
* Where not specified, a value of `true` will be assumed
* Default: `true`

## Developing

### Issue tracking

Issues, bugs, improvements, questions, suggestions and other tasks related to this package are managed through the 
[BAS Ansible Role Collection](https://jira.ceh.ac.uk/projects/BARC) (BARC) project on Jira.

This service is currently only available to BAS or NERC staff, although external collaborators can be added on request.
See our contributing policy for more information.

### Source code

All changes should be committed, via pull request, to the canonical repository, which for this project is:

`ssh://git@stash.ceh.ac.uk:7999/barc/ruby2.git`

A mirror of this repository is maintained on GitHub. Changes are automatically pushed from the canonical repository to
this mirror, in a one-way process.

`git@github.com:antarctica/ansible-ruby2.git`

Note: The canonical repository is only accessible within the NERC firewall. External collaborators, please make pull 
requests against the mirrored GitHub repository and these will be merged as appropriate.

### Contributing policy

This project welcomes contributions, see `CONTRIBUTING.md` for our general policy.

The [Git flow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow/) 
workflow is used to manage the development of this project:

* Discrete changes should be made within feature branches, created from and merged back into develop 
(where small changes may be made directly)
* When ready to release a set of features/changes, create a release branch from develop, update documentation as 
required and merge into master with a tagged, semantic version (e.g. v1.2.3)
* After each release, the master branch should be merged with develop to restart the process
* High impact bugs can be addressed in hotfix branches, created from and merged into master (then develop) directly

## Licence

Copyright 2015 NERC BAS.

Unless stated otherwise, all documentation is licensed under the Open Government License - version 3. All code is
licensed under the MIT license.

Copies of these licenses are included within this role.
