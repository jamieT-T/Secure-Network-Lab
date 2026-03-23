# Firewall Rules Documentation

## Overview

Complete documentation of all firewall rules and their security rationale.

## Rule Processing

pfSense processes rules top-to-bottom:
- First match wins
- Unmatched traffic denied by default

## LAN Firewall Rules

### Rule 1: Allow LAN to Any

```
Action: Pass
Protocol: Any
Source: LAN net
Destination: any
```

**Purpose**: Allow trusted users full access

**Allows**:
- LAN → Internet
- LAN → DMZ  
- LAN → pfSense management

**Rationale**: Users need internet access and administrative capabilities

## DMZ Firewall Rules

### Rule 1: Block DMZ to LAN (CRITICAL)

```
Action: Block
Protocol: Any
Source: DMZ net
Destination: LAN net
```

**Purpose**: Prevent lateral movement

**This is the most important security control.**

**Threat Scenario**:
- Attacker compromises web server
- Tries to pivot to internal network
- Firewall blocks all DMZ → LAN traffic
- Attack contained to DMZ

### Rule 2: Allow DMZ to Internet

```
Action: Pass
Protocol: Any
Source: DMZ net
Destination: WAN net
```

**Purpose**: Allow updates and patches

**Allows**: Security updates, dependencies

**Risks**: Potential C2 communication, data exfiltration

**Mitigation**: Monitor outbound traffic, add IDS (future)

## WAN Firewall Rules

### Rule 1: Allow OpenVPN

```
Action: Pass
Protocol: UDP
Source: any
Destination: WAN address
Port: 1194
```

**Purpose**: Enable VPN access

**Security**: Only port open to internet, requires authentication

## OpenVPN Firewall Rules

### Rule 1: Allow VPN Full Access

```
Action: Pass
Protocol: Any
Source: any (VPN clients)
Destination: any
```

**Purpose**: Authenticated administrators need full access

**Security**: Certificate + password required

## Testing Validation

All rules tested and validated:

| Source | Destination | Expected | Result |
|--------|-------------|----------|--------|
| LAN | Internet | Allow | Pass |
| LAN | DMZ:80 | Allow | Pass |
| DMZ | LAN | Block | Pass |
| DMZ | Internet | Allow | Pass |
| VPN | All | Allow | Pass |

**100% Pass Rate**

---

Last Updated: March 2026
