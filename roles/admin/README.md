# Admin Role

This role manages system administration tasks, monitoring, and secure access for validator nodes.

## Features

- UFW firewall configuration
- Monit system monitoring
- Grafana Agent for metrics collection
- Promtail for log aggregation
- Node Exporter for system metrics
- Teleport for secure SSH access
- Binary signature verification

## Requirements

- Ansible 2.9+
- Ubuntu 20.04+ / Debian 11+
- Python 3.8+

## Role Variables

### UFW Configuration
```yaml
ufw:
  enabled: true
  default_incoming_policy: "deny"
  default_outgoing_policy: "allow"
  rules:
    - { port: 22, proto: "tcp", rule: "allow" }
    - { port: 3022, proto: "tcp", rule: "allow" }
```

### Monit Configuration
```yaml
monit:
  enabled: true
  config_dir: "/etc/monit"
  config_file: "/etc/monit/monitrc"
  include_dir: "/etc/monit/conf.d"
  check_interval: 30
  start_delay: 120
```

### Promtail Configuration
```yaml
promtail:
  version: "2.9.2"
  enabled: true
  install_dir: "/usr/local/bin"
  config_dir: "/etc/promtail"
  positions_dir: "/var/lib/promtail"
  loki_url: "http://loki:3100/loki/api/v1/push"
```

### Grafana Agent Configuration
```yaml
grafana_agent:
  version: "0.39.1"
  enabled: true
  remote_write:
    - url: "http://prometheus:9090/api/v1/write"
```

### Node Exporter Configuration
```yaml
node_exporter:
  version: "1.7.0"
  enabled: true
  port: 9100
```

### Teleport Configuration
```yaml
teleport:
  enabled: true
  version: "13.3.2"
  auth_token: ""  # Required
  auth_server: "" # Required
```

## Dependencies

Required Ansible collections:
- community.general
- ansible.posix

## Example Playbook

```yaml
- hosts: validators
  roles:
    - role: admin
      vars:
        teleport:
          auth_token: "your-auth-token"
          auth_server: "auth.example.com:3025"
        monit:
          checks:
            system:
              enabled: true
        promtail:
          enabled: true
          loki_url: "http://loki.example.com:3100"
```

## Security Features

- Binary signature verification for all downloaded binaries
- UFW firewall with restrictive default policies
- Secure service configurations with minimal privileges
- Dedicated system users for each service
- Systemd service hardening

## Monitoring Stack

### Monit
- System resource monitoring
- Process monitoring
- Custom service checks

### Grafana Agent
- Metrics collection
- Remote write to Prometheus
- Service discovery

### Promtail
- Log aggregation
- Label management
- Loki integration

### Node Exporter
- System metrics collection
- Hardware monitoring
- Resource utilization tracking

## Testing

This role includes Molecule tests for verifying functionality:

```bash
# Install test dependencies
pip install molecule molecule-docker ansible-lint

# Run tests
cd roles/admin
molecule test
```

The tests verify:
- Package installation
- Service configuration
- Port availability
- Binary installation
- Configuration files
- System users
- UFW rules

## License

MIT

## Author Information

Created by [Your Name]
