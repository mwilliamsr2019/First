# SSSD and xRDP Ansible Playbook
Current Version: v1.0

This Ansible playbook installs and configures SSSD (System Security Services Daemon) and xRDP on Ubuntu systems for centralized authentication and remote desktop access.

## Features

- **SSSD Configuration**: LDAP/Active Directory integration
- **xRDP Installation**: Remote desktop protocol server
- **XFCE Desktop**: Lightweight desktop environment
- **Security**: TLS encryption and secure authentication
- **Firewall**: UFW configuration for xRDP access

## Requirements

- Ansible 2.9+
- Ubuntu target systems (18.04, 20.04, 22.04)
- SSH access to target systems
- Sudo privileges on target systems

## Quick Start

1. **Clone and configure**:
   ```bash
   cd sssd-xrdp-playbook
   ```

2. **Update inventory**:
   Edit `inventory/hosts.yml` with your target hosts

3. **Configure variables**:
   Edit `group_vars/ubuntu_servers.yml` with your LDAP settings

4. **Create encrypted vault** (for LDAP password):
   ```bash
   ansible-vault create group_vars/ubuntu_servers/vault.yml
   ```
   Add: `vault_sssd_ldap_bind_password: "your-secure-password"`

5. **Run playbook**:
   ```bash
   ansible-playbook -i inventory/hosts.yml site.yml --ask-vault-pass
   ```

## Configuration

### SSSD Settings

| Variable | Description | Default |
|----------|-------------|---------|
| `sssd_domain` | Domain name for SSSD | `company.local` |
| `sssd_realm` | Kerberos realm | `COMPANY.LOCAL` |
| `sssd_ldap_uri` | LDAP server URI | `ldaps://dc.company.local:636` |
| `sssd_ldap_search_base` | LDAP search base | `dc=company,dc=local` |

### xRDP Settings

| Variable | Description | Default |
|----------|-------------|---------|
| `xrdp_port` | RDP listening port | `3389` |
| `xrdp_max_bpp` | Maximum color depth | `24` |
| `xrdp_crypt_level` | Encryption level | `high` |

## Usage

### Connect via RDP

After deployment, connect using any RDP client:
- **Server**: Target Ubuntu IP address
- **Port**: 3389 (default)
- **Credentials**: Use your LDAP/AD credentials

### Testing

1. **Verify SSSD**:
   ```bash
   sudo systemctl status sssd
   getent passwd username@domain
   ```

2. **Verify xRDP**:
   ```bash
   sudo systemctl status xrdp
   netstat -tlnp | grep 3389
   ```

## Troubleshooting

### Common Issues

1. **LDAP Connection Failed**:
   - Check LDAP server connectivity
   - Verify certificate trust
   - Check bind credentials

2. **RDP Connection Failed**:
   - Verify firewall rules
   - Check xrdp service status
   - Ensure desktop environment is installed

3. **Authentication Issues**:
   - Check SSSD configuration
   - Verify user exists in LDAP
   - Check PAM configuration

### Debug Commands

```bash
# SSSD debug
sudo sssctl domain-status company.local
sudo tail -f /var/log/sssd/sssd_company.local.log

# xRDP debug
sudo tail -f /var/log/xrdp.log
sudo tail -f /var/log/xrdp-sesman.log
```

## Security Notes

- Change default passwords
- Use TLS/SSL certificates
- Configure proper firewall rules
- Regular security updates
- Monitor authentication logs

## License

MIT License - see LICENSE file for details