# Network Design Documentation

## Overview

This document explains the network architecture, security design, and rationale.

## Architecture

### Three-Tier Design
- WAN: Internet boundary (10.0.2.0/24)
- LAN: Trusted internal network (192.168.1.0/24)
- DMZ: Public services zone (192.168.2.0/24)

### Security Zones

**LAN (Trusted)**
- Corporate users and administrators
- Full internet access
- Limited DMZ access

**DMZ (Semi-Trusted)**
- Public-facing web services
- NO access to LAN (critical isolation)
- Limited internet access for updates

**VPN (Authenticated)**
- Remote administrators
- Full network access after authentication
- Encrypted tunnel

## Design Decisions

### Why Segment Networks?

Without segmentation: Single breach = complete compromise
With segmentation: Breaches contained to zones

### Why Block DMZ to LAN?

Threat scenario:
1. Attacker exploits web server
2. Gains access to DMZ
3. Attempts lateral movement
4. Firewall blocks DMZ → LAN
5. Attack contained

### Why VPN?

- Secure remote access
- No exposed services
- Certificate + password authentication
- Centralized access control

## IP Addressing Strategy

| Network | CIDR | Type | Purpose |
|---------|------|------|---------|
| WAN | 10.0.2.0/24 | DHCP | Internet via NAT |
| LAN | 192.168.1.0/24 | DHCP | Internal users |
| DMZ | 192.168.2.0/24 | Static | Public services |
| VPN | 10.8.0.0/24 | Dynamic | Remote access |

## Security Controls

- Default deny firewall posture
- Zone-based policies  
- DMZ isolation from LAN
- Encrypted VPN access
- Defense in depth

---

Last Updated: March 2026
