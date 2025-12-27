# Split-Tunnel VPN System

A flexible and robust split-tunnel VPN implementation that allows users to route selected traffic through a VPN while keeping other traffic on the local network. This system is designed for enhanced privacy, security, and performance optimization.

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [System Requirements](#system-requirements)
- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)
- [Architecture](#architecture)
- [API Documentation](#api-documentation)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [License](#license)

## Overview

Split-tunnel VPN technology enables users to simultaneously route different types of traffic through different network paths. This system implements a sophisticated split-tunnel VPN that allows granular control over which applications, domains, or IP addresses are routed through the VPN connection.

### Key Benefits

- **Improved Performance**: Local traffic stays local, reducing latency for non-sensitive operations
- **Enhanced Privacy**: Route sensitive traffic through encrypted VPN tunnels
- **Bandwidth Efficiency**: Optimize bandwidth usage by excluding streaming services and large downloads from VPN tunneling
- **Flexible Routing**: Fine-grained control over application and domain-based routing policies

## Features

### Core Capabilities

- ✅ Application-based traffic routing
- ✅ Domain-based routing policies
- ✅ IP address-based filtering
- ✅ Real-time traffic monitoring
- ✅ Automatic connection management
- ✅ Multi-protocol support (OpenVPN, WireGuard, IPSec)
- ✅ Cross-platform compatibility (Windows, macOS, Linux)
- ✅ Persistent configuration storage
- ✅ Traffic statistics and logging
- ✅ Automatic failover and reconnection

### Advanced Features

- Dynamic rule updates without restart
- Kill switch protection
- DNS leak prevention
- IPv6 support
- Custom routing rules
- Traffic encryption and compression options
- Load balancing across multiple VPN servers
- Scheduled connection/disconnection

## System Requirements

### Minimum Requirements

- **OS**: Windows 10+, macOS 10.15+, Linux (Ubuntu 18.04+, CentOS 7+, Debian 10+)
- **CPU**: 1 GHz dual-core processor
- **RAM**: 512 MB minimum (1 GB recommended)
- **Storage**: 100 MB available space
- **Network**: Active internet connection
- **Root/Administrator Access**: Required for installation and operation

### Network Requirements

- Support for tunneling protocols (OpenVPN, WireGuard, or IPSec)
- Unrestricted outbound connections on VPN ports
- IPv4 or IPv6 connectivity (or both)

## Installation

### Prerequisites

Before installation, ensure you have:
- Administrator/root privileges
- A compatible VPN provider account (or self-hosted VPN server)
- Network connectivity

### Linux Installation

```bash
# Clone the repository
git clone https://github.com/CleoWixom/split-tunnel-vpn.git
cd split-tunnel-vpn

# Install dependencies
sudo apt-get update
sudo apt-get install -y openvpn wireguard iptables

# Run installation script
sudo bash install.sh

# Start the service
sudo systemctl start split-tunnel-vpn
sudo systemctl enable split-tunnel-vpn
```

### macOS Installation

```bash
# Clone the repository
git clone https://github.com/CleoWixom/split-tunnel-vpn.git
cd split-tunnel-vpn

# Install Homebrew (if not already installed)
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Install dependencies
brew install openvpn wireguard

# Run installation script
sudo bash install.sh

# Start the service
sudo launchctl load /Library/LaunchDaemons/com.splitvpn.daemon.plist
```

### Windows Installation

1. Download the latest installer from [Releases](https://github.com/CleoWixom/split-tunnel-vpn/releases)
2. Run the `.exe` installer with Administrator privileges
3. Follow the installation wizard
4. Restart your computer when prompted
5. Launch the application from the Start menu

## Configuration

### Basic Configuration

The main configuration file is located at:

- **Linux/macOS**: `~/.config/split-tunnel-vpn/config.yaml`
- **Windows**: `C:\Users\<YourUsername>\AppData\Local\split-tunnel-vpn\config.yaml`

### Example Configuration File

```yaml
vpn:
  provider: "custom"
  protocol: "wireguard"  # or "openvpn"
  server: "vpn.example.com"
  port: 51820
  credentials:
    username: "your_username"
    password: "your_password"
  
  # Connection settings
  auto_reconnect: true
  reconnect_interval: 30  # seconds
  connection_timeout: 10  # seconds

split_tunnel:
  enabled: true
  mode: "whitelist"  # or "blacklist"
  
  # Application-based routing
  applications:
    - name: "chrome"
      path: "/usr/bin/google-chrome"
      route_through_vpn: true
    - name: "firefox"
      path: "/usr/bin/firefox"
      route_through_vpn: true
    - name: "curl"
      path: "/usr/bin/curl"
      route_through_vpn: false

  # Domain-based routing
  domains:
    - domain: "*.example.com"
      route_through_vpn: true
    - domain: "streaming.service.com"
      route_through_vpn: false
    - domain: "*.local"
      route_through_vpn: false

  # IP-based routing
  ip_ranges:
    - cidr: "192.168.0.0/16"
      route_through_vpn: false
    - cidr: "10.0.0.0/8"
      route_through_vpn: false

security:
  kill_switch: true
  dns_leak_protection: true
  custom_dns_servers:
    - "1.1.1.1"
    - "8.8.8.8"
  
logging:
    level: "info"  # debug, info, warning, error
    file: "/var/log/split-tunnel-vpn.log"
    max_size: "10mb"
    retention_days: 30

monitoring:
  enable_stats: true
  stats_interval: 60  # seconds
  enable_traffic_logging: false
```

### Configuration Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `vpn.protocol` | string | `wireguard` | VPN protocol to use |
| `vpn.server` | string | - | VPN server address |
| `vpn.port` | integer | 51820 | VPN server port |
| `split_tunnel.enabled` | boolean | true | Enable split-tunnel mode |
| `split_tunnel.mode` | string | `whitelist` | Routing mode: whitelist or blacklist |
| `security.kill_switch` | boolean | true | Enable kill switch |
| `security.dns_leak_protection` | boolean | true | Protect against DNS leaks |

## Usage

### Command-Line Interface

#### Connect to VPN

```bash
split-tunnel-vpn connect
```

#### Disconnect from VPN

```bash
split-tunnel-vpn disconnect
```

#### Check Status

```bash
split-tunnel-vpn status
```

#### View Connection Statistics

```bash
split-tunnel-vpn stats
```

#### Update Rules

```bash
split-tunnel-vpn update-rules
```

#### View Logs

```bash
split-tunnel-vpn logs --lines 100
```

### GUI Usage

1. Open the Split-Tunnel VPN application
2. Configure VPN server and credentials in Settings
3. Define split-tunnel rules for applications and domains
4. Click "Connect" to establish the VPN connection
5. Monitor real-time traffic statistics in the Dashboard

### Typical Workflows

#### Example 1: Route Work Applications Through VPN

```yaml
split_tunnel:
  mode: "whitelist"
  applications:
    - name: "outlook"
      route_through_vpn: true
    - name: "slack"
      route_through_vpn: true
    - name: "vpn-app"
      route_through_vpn: true
```

#### Example 2: Exclude Streaming Services from VPN

```yaml
split_tunnel:
  mode: "blacklist"
  domains:
    - domain: "*.netflix.com"
      route_through_vpn: false
    - domain: "*.youtube.com"
      route_through_vpn: false
    - domain: "*.hulu.com"
      route_through_vpn: false
```

## Architecture

### System Components

```
┌─────────────────────────────────────────────┐
│       User Applications                      │
├─────────────────────────────────────────────┤
│       Network Stack / Socket Layer           │
├─────────────────────────────────────────────┤
│       Split-Tunnel Router                    │
│  (Decision Engine & Traffic Classifier)     │
├─────────────────────────────────────────────┤
│    ┌──────────────┐      ┌──────────────┐  │
│    │ VPN Tunnel   │      │ Local Network│  │
│    │ Interface    │      │ Interface    │  │
│    └──────────────┘      └──────────────┘  │
├─────────────────────────────────────────────┤
│       System Firewall & Routing             │
└─────────────────────────────────────────────┘
```

### Traffic Flow

1. **Packet Interception**: All outbound traffic is intercepted by the routing engine
2. **Rule Evaluation**: Packets are matched against defined rules
3. **Decision Making**: System determines if traffic should use VPN or local route
4. **Forwarding**: Traffic is forwarded to appropriate interface
5. **Monitoring**: Statistics are collected and logged

### Key Modules

- **Connection Manager**: Handles VPN connection lifecycle
- **Rule Engine**: Evaluates traffic against configured rules
- **Network Monitor**: Tracks connection status and statistics
- **Configuration Manager**: Handles rule updates and persistence
- **Logging Service**: Records events and traffic data

## API Documentation

### REST API

The system exposes a REST API for programmatic control.

#### Base URL

```
http://localhost:8080/api/v1
```

#### Authentication

All requests require Bearer token authentication:

```bash
Authorization: Bearer YOUR_API_TOKEN
```

### Endpoints

#### Connect to VPN

```http
POST /vpn/connect
```

**Response:**
```json
{
  "status": "connecting",
  "message": "VPN connection initiated"
}
```

#### Disconnect from VPN

```http
POST /vpn/disconnect
```

**Response:**
```json
{
  "status": "disconnected",
  "message": "VPN connection closed"
}
```

#### Get VPN Status

```http
GET /vpn/status
```

**Response:**
```json
{
  "connected": true,
  "server": "vpn.example.com",
  "ip_address": "203.0.113.42",
  "connected_time": 3600,
  "data_sent": 1073741824,
  "data_received": 2147483648
}
```

#### Get Rules

```http
GET /rules
```

**Response:**
```json
{
  "rules": [
    {
      "id": "rule-001",
      "type": "application",
      "name": "chrome",
      "route_through_vpn": true
    }
  ]
}
```

#### Add Rule

```http
POST /rules
Content-Type: application/json

{
  "type": "domain",
  "pattern": "*.example.com",
  "route_through_vpn": true
}
```

#### Delete Rule

```http
DELETE /rules/{rule_id}
```

## Troubleshooting

### Connection Issues

#### Problem: Unable to connect to VPN

**Solutions:**
1. Verify VPN server address and port are correct
2. Check internet connectivity: `ping 8.8.8.8`
3. Verify firewall is not blocking VPN port
4. Check VPN provider account credentials
5. Review logs: `split-tunnel-vpn logs`

#### Problem: Connected but no internet access

**Solutions:**
1. Verify DNS configuration
2. Check if kill switch is enabled: `split-tunnel-vpn status`
3. Verify routing rules: `split-tunnel-vpn list-rules`
4. Flush DNS cache: `sudo systemctl restart systemd-resolved` (Linux)

### Performance Issues

#### Problem: Slow connection speed

**Solutions:**
1. Switch to a different VPN server closer geographically
2. Change VPN protocol to WireGuard for faster speeds
3. Disable encryption compression: Edit config file
4. Check system resources: `top` or Task Manager

#### Problem: High CPU usage

**Solutions:**
1. Disable traffic logging if not needed
2. Reduce logging verbosity level
3. Check for rule matching conflicts
4. Restart the service: `sudo systemctl restart split-tunnel-vpn`

### Configuration Issues

#### Problem: Rules not applying correctly

**Solutions:**
1. Verify rule syntax in configuration file
2. Restart the service after rule changes
3. Check rule evaluation mode (whitelist vs blacklist)
4. Review application paths are correct

#### Problem: DNS leaks detected

**Solutions:**
1. Enable DNS leak protection: Set `dns_leak_protection: true`
2. Configure custom DNS servers in config
3. Verify kill switch is enabled
4. Test with DNS leak tool: `nslookup example.com`

### Advanced Debugging

Enable debug logging for detailed troubleshooting:

```bash
# Linux/macOS
split-tunnel-vpn logs --level debug

# View system logs
journalctl -u split-tunnel-vpn -f  # Linux
log stream --predicate 'process == "split-tunnel-vpn"'  # macOS
```

## Contributing

We welcome contributions to improve the split-tunnel VPN system!

### Contribution Guidelines

1. **Fork the repository** on GitHub
2. **Create a feature branch**: `git checkout -b feature/your-feature`
3. **Make your changes** and commit: `git commit -am 'Add new feature'`
4. **Push to the branch**: `git push origin feature/your-feature`
5. **Submit a Pull Request** with a clear description

### Development Setup

```bash
# Clone repository
git clone https://github.com/CleoWixom/split-tunnel-vpn.git
cd split-tunnel-vpn

# Install development dependencies
pip install -r requirements-dev.txt

# Run tests
pytest tests/

# Check code style
flake8 .

# Format code
black .
```

### Reporting Issues

Please report bugs and security vulnerabilities on the [Issues](https://github.com/CleoWixom/split-tunnel-vpn/issues) page with:

- Clear description of the issue
- Steps to reproduce
- Expected and actual behavior
- System information (OS, version)
- Relevant logs

## License

This project is licensed under the MIT License - see the LICENSE file for details.

---

**Last Updated**: 2025-12-27

For more information and support, visit the [project repository](https://github.com/CleoWixom/split-tunnel-vpn)
