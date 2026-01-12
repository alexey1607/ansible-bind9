# Ansible Bind9 Role - CHANGELOG

## [Develop] - 2026-01-12

### Added
- Multi-platform support (Debian/Ubuntu and RedHat/CentOS)
- OS-specific variables for different distributions
- BIND configuration validation using named-checkconf and named-checkzone
- Support for custom DNS records (A, MX, TXT, SRV)
- Comprehensive logging configuration with customizable log categories
- DNSSEC configuration options
- Rate limiting for DDoS protection
- Molecule tests for automated testing
- GitHub Actions CI/CD pipeline
- yamllint and ansible-lint configuration
- Improved handlers with reload instead of restart

### Changed
- Updated README with comprehensive documentation
- Refactored tasks to use OS-agnostic variables
- Improved zone templates with support for custom records
- Changed handler behavior to use reload for minimal downtime

### Fixed
- Corrected comment for slave node configuration in main.yml
- Fixed SOA records in reverse zone templates
- Fixed NS and PTR records in reverse zone to use forward zone domain
- Removed hardcoded DNS group from all A records generation

## [Master] - Previous
- Initial release with basic master-slave BIND9 setup
- Support for Debian/Ubuntu only
- Basic forward and reverse zone configuration
