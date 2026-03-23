# Secure Network Lab

> Enterprise network security laboratory demonstrating network segmentation, firewall policies, and remote VPN access

<div align="center">

![Network Status](https://img.shields.io/badge/Status-Production-success)
![Platform](https://img.shields.io/badge/Platform-VirtualBox-blue)
![License](https://img.shields.io/badge/License-MIT-green)

</div>

---

## Overview

A virtualized enterprise network built to demonstrate network security concepts, segmentation strategies, and defense-in-depth architecture. This lab simulates a corporate network environment with multiple security zones, firewall policies, and secure remote access.

### Key Features

- Three-tier network architecture (WAN/LAN/DMZ)
- pfSense firewall with zone-based policies
- Network segmentation and isolation
- OpenVPN remote access with PKI authentication
- Comprehensive documentation and testing

### Why This Project

Modern organizations require segmented networks to contain security breaches, protect internal assets from compromised services, enforce least-privilege access, and provide secure remote access. This lab implements these principles in a controlled virtual environment.

---

## Architecture

### Network Topology

```
Internet
   |
[WAN] -- pfSense Firewall -- [LAN] -- User VM
   |                    |
   |                 [DMZ] -- Web Server
   |
[VPN] -- Remote Client
```

### IP Addressing

| Segment | CIDR | Gateway | Range | Assignment | Purpose |
|---------|------|---------|-------|------------|---------|
| WAN | 10.0.2.0/24 | 10.0.2.2 | 10.0.2.15 | DHCP | Internet connectivity |
| LAN | 192.168.1.0/24 | 192.168.1.1 | .100-.200 | DHCP | Internal network |
| DMZ | 192.168.2.0/24 | 192.168.2.1 | .10+ | Static | Public services |
| VPN | 10.8.0.0/24 | 10.8.0.1 | .6+ | Dynamic | Remote access |

### System Specifications

| Component | OS | RAM | Disk | Network | Services |
|-----------|-----|-----|------|---------|----------|
| pfSense-Firewall | FreeBSD 2.7.x | 1 GB | 8 GB | WAN/LAN/DMZ | Firewall, Router, VPN |
| LAN-User-VM | Ubuntu 24.04 + XFCE | 1 GB | 10 GB | LAN | Management |
| DMZ-Server-VM | Ubuntu 24.04 + Apache | 1 GB | 10 GB | DMZ | Web server |
| **Total** | - | **3 GB** | **28 GB** | **7 NICs** | - |

---

## Quick Start

### Prerequisites

**Hardware:**
- RAM: 8 GB minimum
- Disk: 30 GB free
- CPU: Multi-core with VT-x/AMD-V

**Software:**
- VirtualBox 7.0+
- pfSense ISO
- Ubuntu Server 24.04 ISO

### Installation

| Phase | Duration | Deliverable |
|-------|----------|-------------|
| Phase 1 | 30 min | pfSense firewall with interfaces |
| Phase 2 | 20 min | LAN VM with management GUI |
| Phase 3 | 40 min | Firewall rules and DMZ server |
| Phase 4 | 40 min | OpenVPN remote access |
| Testing | 10 min | Validation of all traffic flows |
| **Total** | **2-3 hours** | **Complete functional lab** |

### Setup Steps

1. Clone repository
   ```bash
   git clone https://github.com/yourusername/secure-network-lab.git
   ```

2. Follow installation guide
   - See [Complete Setup Guide](docs/fresh-start-complete-guide.md)

3. Build and test
   - Create VMs per documentation
   - Configure firewall rules
   - Validate traffic flows

---

## Documentation

### Guides

| Document | Description |
|----------|-------------|
| [Complete Setup](docs/fresh-start-complete-guide.md) | End-to-end installation |
| [pfSense Setup](docs/pfsense-installation-guide.md) | Firewall configuration |
| [Firewall Rules](docs/milestone-2-firewall-rules.md) | Security policies |
| [OpenVPN Setup](docs/openvpn-setup-guide.md) | VPN configuration |
| [Network Diagram](diagrams/network-topology-diagram.html) | Interactive topology |

---

## Testing Results

All security controls validated with systematic testing.

| Test | Source | Destination | Expected | Result | Status |
|------|--------|-------------|----------|--------|--------|
| Internet Access | LAN | 8.8.8.8 | Allow | Pass | Pass |
| DMZ Web Access | LAN | DMZ:80 | Allow | Pass | Pass |
| DMZ SSH Block | LAN | DMZ:22 | Block | Block | Pass |
| DMZ to LAN | DMZ | LAN | Block | Block | Pass |
| DMZ Internet | DMZ | 8.8.8.8 | Allow | Pass | Pass |
| VPN to pfSense | VPN | 192.168.1.1 | Allow | Pass | Pass |
| VPN to DMZ | VPN | DMZ:80 | Allow | Pass | Pass |

**Results: 7/7 Tests Passed (100%)**

### Traffic Flow Summary

```
LAN → Internet         : Allowed
LAN → DMZ (HTTP/HTTPS) : Allowed
DMZ → LAN              : Blocked (Critical isolation control)
DMZ → Internet         : Allowed
VPN → All Networks     : Allowed
Internet → DMZ         : Blocked
```

---

## Skills Demonstrated

### Technical Competencies

**Network Engineering**
- Network architecture and topology design
- IP addressing and subnetting (CIDR)
- Routing and gateway configuration
- Network segmentation strategies

**Security Engineering**
- Firewall rule creation and management
- Zone-based security policies
- Defense-in-depth implementation
- VPN deployment and PKI infrastructure
- Threat modeling and containment

**Systems Administration**
- Linux server deployment
- Service configuration (Apache, OpenVPN, SSH)
- Virtual infrastructure management
- System troubleshooting

**Documentation & Project Management**
- Technical writing
- Network diagramming
- Version control (Git)
- Testing and validation procedures

---

## Technologies

| Category | Technology | Version |
|----------|-----------|---------|
| Virtualization | VirtualBox | 7.0+ |
| Firewall | pfSense | 2.7.x |
| Operating System | Ubuntu Server | 24.04 LTS |
| Desktop | XFCE | 4.x |
| VPN | OpenVPN | 2.6.x |
| Web Server | Apache | 2.4.x |
| Encryption | AES-256-CBC | - |

---

## Roadmap

### Current: v1.0.0

- Complete 3-tier architecture
- Firewall with security policies
- DMZ server with web service
- OpenVPN remote access
- Full documentation

### Planned Enhancements

**Phase 4: IDS/IPS**
- Suricata intrusion detection
- Custom detection rules
- Alerting configuration

**Phase 5: Logging**
- ELK stack deployment
- Centralized log management
- Security dashboards

**Phase 6: Monitoring**
- Grafana + Prometheus
- Real-time traffic monitoring
- Performance dashboards

**Phase 7: Advanced Security**
- Two-factor authentication
- Certificate revocation
- GeoIP blocking
- Rate limiting

---

## Contributing

Contributions welcome. Please fork the repository and submit a pull request.

1. Fork the project
2. Create feature branch
3. Commit changes
4. Push to branch
5. Open pull request

---

## License

MIT License - see [LICENSE](LICENSE) file for details.

---

## Contact

**Maintainer**: [Your Name]

- GitHub: [@yourusername](https://github.com/yourusername)
- LinkedIn: [Your Profile](https://linkedin.com/in/yourprofile)
- Email: your.email@example.com

---

## Project Status

**Status**: Production Ready  
**Version**: 1.0.0  
**Last Updated**: March 2026

All milestones complete. Fully tested and documented.

---

**Note**: This is a laboratory environment for educational purposes. Professional security review recommended before production deployment.
