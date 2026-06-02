# Changelog

All notable changes to this project will be documented in this file.

## [1.3.0] - 2026-06-02

### Role - oss

- Added support for Ubuntu 26.04.

## [1.2.0] - 2025-10-08

### Role - oss

- Added support for Debian 13.
- Added support for EL 10.

## [1.1.4] - 2025-06-11

### Collection

- Changed repository URL to use GitHub Organization.
- Corrected missing or extra dependencies.

## [1.1.3] - 2025-05-15

### Role - oss

- Changed OS validation.

### Role - oss_reverse_proxy

- Changed OS validation.

## [1.1.2] - 2025-01-08

### Collection

- Added Changelog.
- Updated collection README documentation.

## [1.1.1] - 2024-08-09

### Collection

- Minimum Ansible version changed from `2.14` to `2.15` due to EOL status.

## [1.1.0] - 2024-08-03

### Role - oss

- Added validation where possble for all config directives.

## [1.0.9] - 2024-07-08

### Collection

- Updated manifest file to ensure that molecule tests are not included in releases.

## [1.0.8] - 2024-07-01

### Role - oss

- Removed config validation task because of false positive.

## [1.0.7] - 2024-07-01

### Role - oss

- Reordered tasks to limit false positives from config validation.
- Ensured consistent formatting in the validation task messages.

### Rose - oss_reverse_proxy

- Fixed a mistake in the variables sent to the *oss* role.

## [1.0.6] - 2024-07-01

### Role - oss

- Fixed various indentation problems within the templates.

## [1.0.5] - 2024-07-01

### Role - oss

- Changed the `server_name` directive to accept a list of strings instead of a string in both the templates and the argument spec.

## [1.0.4] - 2024-07-01

### Role - oss

- Added missing `index` and `random_index` directives to the templates and argument spec, where appropriate.

## [1.0.3] - 2024-07-01

### Role - oss

- Added missing `access_log` directive to role argument spec for the `location` blocks.

## [1.0.2] - 2024-06-30

### Role - oss

- Added more description to various variables in the role argument spec.

## [1.0.1] - 2024-06-29

### Role - oss

- Renamed the `nginx_allow_firewalld_http` and `nginx_allow_firewalld_https` variables to `nginx_allow_http` and `nginx_allow_https` variables to be more host firewall agnostic.
- Changed references to Red Hat Enterprise Linux (RHEL) to more accurately reference Enterprise Linux (EL) to convey the intention to support derivatives (Rocky/AlmaLinux/etc.)
- Refactored some validation tasks for better maintainability.

## [1.0.0] - 2024-06-24

### Collection

- Initial release.
- *oss* role added.
- *oss_reverse_proxy* role added.
