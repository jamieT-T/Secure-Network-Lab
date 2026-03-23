# Getting Started

Quick start guide for this repository.

## What This Is

A complete secure network security lab demonstrating:
- Network segmentation (WAN/LAN/DMZ)
- pfSense firewall with zone-based policies
- OpenVPN remote access
- Defense-in-depth architecture

## Quick Setup

1. **Clone repository**
   ```bash
   git clone https://github.com/yourusername/secure-network-lab.git
   cd secure-network-lab
   ```

2. **Read installation guide**
   - See [docs/INSTALLATION.md](docs/INSTALLATION.md)

3. **Build the lab**
   - Phase 1: pfSense (30 min)
   - Phase 2: LAN VM (20 min)
   - Phase 3: DMZ Server (25 min)
   - Phase 4: OpenVPN (40 min)
   - Total: ~2 hours

4. **Test everything**
   - Follow [docs/TESTING.md](docs/TESTING.md)

## Documentation

| Document | Purpose |
|----------|---------|
| [INSTALLATION](docs/INSTALLATION.md) | Step-by-step setup |
| [NETWORK-DESIGN](docs/NETWORK-DESIGN.md) | Architecture |
| [FIREWALL-RULES](docs/FIREWALL-RULES.md) | Security policies |
| [VPN-SETUP](docs/VPN-SETUP.md) | OpenVPN config |
| [TESTING](docs/TESTING.md) | Validation |
| [TROUBLESHOOTING](docs/TROUBLESHOOTING.md) | Common issues |

## Prerequisites

- VirtualBox 7.0+
- 8 GB RAM minimum
- 30 GB disk space
- pfSense ISO
- Ubuntu Server 24.04 ISO

## Next Steps

1. Download ISOs
2. Follow installation guide
3. Build and test
4. Document with screenshots
5. Update this README with your results

## Need Help?

See [TROUBLESHOOTING.md](docs/TROUBLESHOOTING.md)

---

Built with pfSense, Ubuntu, and VirtualBox
