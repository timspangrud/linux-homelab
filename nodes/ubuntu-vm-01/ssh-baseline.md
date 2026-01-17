# Homelab Node — SSH Hardening & Port Management

## Infrastructure & Linux
- Hardened OpenSSH configuration to enforce key-only authentication, user allow-listing, and disabled root SSH access.
- Migrated SSH policy configuration to drop-in files under `/etc/ssh/sshd_config.d/` for update-safe, modular management.
- Validated daemon configuration changes using `sshd -t` and inspected effective settings with `sshd -T`.

## Web & Networking
- Reconfigured SSH to listen on a non-standard port (2222) while maintaining fallback access during testing.
- Investigated and resolved discrepancies between configured ports and active listeners by identifying systemd socket activation (`ssh.socket`) as the binding mechanism.
- Standardized on daemon-managed ports by disabling socket activation and returning port ownership to `sshd_config`.

## Security & Observability
- Monitored SSH lifecycle and access attempts using `journalctl`.
- Detected and corrected a UFW firewall rule set that silently blocked inbound traffic on port 2222.
- Verified successful external connectivity and enforced access controls after updating firewall policy.

## Configuration Notes (Source of Truth)
- **Socket activation:** Disabled to avoid split-brain port management.

## Configuration model: Drop-in file under /etc/ssh/sshd_config.d/

- 99-custom-port.conf → network binding
- (e.g., Port 22, Port 2222)

##Rationale: 
- Separate policy from network exposure to simplify troubleshooting, reduce configuration drift, and keep changes update-safe.
