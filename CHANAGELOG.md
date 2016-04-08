# Ruby 2 (`ruby2`) - Change log
 
All notable changes to this role will be documented in this file.
This role adheres to [Semantic Versioning](http://semver.org/spec/v2.0.0.html).
 
Remember: Make sure to update `ruby2_barc_role_version` variable when a new version is released.
 
## [Unreleased][unreleased]

## 0.2.0 - 08/04/2016

### Added

* Continuous Integration testing

### Fixed

* Task names - minor inconsistencies
* Variable comments - very minor inconsistencies
* Testing role dependencies should always use latest versions
* README typos

### Changed

* Refactored package installation tests
* Migrating from new Ansible Galaxy namespace, from 'BARC' to 'bas-ansible-roles-collection'
* Migrating from old Ansible Galaxy 'categories' to new 'tags' meta-data
* Refactored role to use Pristine base project - BARC flavour version 0.2.0
* Upgrading to Ruby 2.3 on Ubuntu systems using non-systems sources, from 2.2

## 0.1.0 - 07/12/2015

### Added

* Initial version - with support for fixing old default ruby version on Ubuntu when using system package
