# Changelog

All notable changes to this project will be documented in this file.

## [0.1.0] - 2025-11-14

### Added

- **DNS Stack Deployment:** Deploys Unbound and AdGuard Home as containers managed by `podman-compose`.
- **Security and Privacy-Focused Configuration:** Includes Unbound hardening, DNSSEC validation, and the ability to operate in recursive mode or forward queries via DNS-over-TLS.
- **System Integration:** Creates and manages a systemd user service with Quadlet for automated startup and management of the DNS stack.
- **Variable Prefixing:** All variables have been prefixed with `mesh_17711_stack_do__` to avoid conflicts with other roles.

### Changed

- Made ownership of all created files and directories consistent, using the `ops` user and group.
- Introduced `mesh_17711_stack_do__dns_stack_ops_group` variable for group ownership to improve maintainability.

### Fixed

- Corrected a file permission error that occurred when deploying Unbound configuration files by ensuring parent directories are created with the proper ownership.
