# Polkadot Validator Infrastructure Deployment

## Overview

This Ansible collection provides a comprehensive, secure, and automated solution for deploying and managing Polkadot validator nodes. Designed with a focus on security, observability, and reliability, the collection offers robust infrastructure-as-code capabilities.

## Architecture

```mermaid
graph TD
    subgraph Validator Infrastructure
        A[Polkadot Validator Node] --> |Metrics| B[Monitoring Stack]
        A --> |Logging| B
        A --> |Service Management| C[Systemd]
        
        subgraph Security Layer
            D[UFW Firewall]
            E[SSH Hardening]
            F[Binary Signature Verification]
        end
        
        subgraph Monitoring Components
            G[Prometheus]
            H[Grafana Agent]
            I[Node Exporter]
            J[Promtail]
            K[Monit]
        end
    end

    B --> G
    B --> H
    B --> I
    B --> J
    B --> K

    D --> A
    E --> A
    F --> A
```

## Services Interaction

```mermaid
graph LR
    subgraph Validator Node
        A[Polkadot Service]
        B[Systemd]
        C[UFW Firewall]
    end

    subgraph Monitoring
        D[Prometheus]
        E[Grafana Agent]
        F[Node Exporter]
        G[Promtail]
        H[Monit]
    end

    subgraph Security
        I[SSH Hardening]
        J[Binary Verification]
    end

    A -->|Exposes Metrics| D
    A -->|Logs| G
    A -->|Managed By| B
    A -->|Protected By| C

    B -->|Monitors| H
    B -->|Controls| A

    C -->|Restricts Access| A

    D -->|Scrapes Metrics| E
    D -->|Collects System Metrics| F

    G -->|Forwards Logs| D

    I -->|Secures Access| A
    J -->|Validates Binaries| A
```

## Security and Hardening Features

### System Hardening
- **Shared Memory Protection**
  - Secure mounting of `/dev/shm`
  - Prevents code execution, setuid, and device access
  - Configurable mount options

- **Fail2ban SSH Protection**
  - Configurable brute-force attack mitigation
  - Customizable ban times and retry attempts
  - Multi-service support (SSH)

### Monitoring and Alerting
- Comprehensive system monitoring
- Centralized log management
- Proactive alerting
- **OpsGenie Heartbeats**
  - Configurable service health monitoring
  - Multiple heartbeat endpoints
  - Flexible interval settings

## Quick Configuration Examples

### Hardening Configuration
```yaml
hardening:
  shared_memory:
    enabled: true
    mount_options: 
      - noexec
      - nosuid
      - nodev
  
  fail2ban:
    enabled: true
    ssh:
      max_retry: 5
      ban_time: 7200  # 2 hours
      find_time: 1200  # 20 minutes
    services:
      - name: sshd
        enabled: true
```

### OpsGenie Heartbeat Configuration
```yaml
opsgenie:
  heartbeats:
    enabled: true
    api_key: "your_opsgenie_api_key"
    heartbeats:
      - name: node_health
        interval: 5  # minutes
        enabled: true
      - name: validator_status
        interval: 5  # minutes
        enabled: true
```

## Features

### 🔒 Security
- SSH hardening with best practices
- Firewall configuration (UFW)
- Binary signature verification
- Minimal privilege execution
- Secure service configurations

### 🖥️ Monitoring
- Prometheus metrics collection
- Grafana Agent integration
- Monit service monitoring
- Node Exporter system metrics
- Promtail log aggregation

### 🚀 Validator Management
- Automated Polkadot binary deployment
- Systemd service management
- Version control
- Resource restriction
- Health checks

## Roles

### Admin Role
- System-level configurations
- Security hardening
- Monitoring stack setup
- User and group management

### Polkadot Role
- Validator node deployment
- Binary management
- Service configuration
- Monitoring integration

## Prerequisites

- Ansible 2.9+
- Ubuntu 20.04/22.04 LTS
- Python 3.8+

## Installation

1. Install Ansible collections:
```bash
ansible-galaxy install -r requirements.yml
```

2. Configure your inventory and variables

## Configuration

Customize deployment through role variables in `group_vars` or `host_vars`:

- `polkadot_version`: Specify Polkadot binary version
- `validator_name`: Set your validator's name
- `monitoring_enabled`: Enable/disable monitoring components

## Testing

Uses Molecule for automated testing:
- Docker-based test environments
- Comprehensive role verification
- Scenario-based testing

## Security Considerations

- Regular updates to binary signatures
- Periodic security audits
- Minimal exposed ports
- Strict authentication mechanisms

## Contributing

1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Push and create a Pull Request

## License

Apache-2.0 License - See [LICENSE](LICENSE) file for details.

## Documentation

For detailed documentation on each role, please refer to:
- [Admin Role Documentation](roles/admin/README.md)
- [Polkadot Role Documentation](roles/polkadot/README.md)

## Contact

Maintained by the Polkadot Validator Infrastructure Team
