# OpenVPN Setup Guide

## Overview

Configure OpenVPN server on pfSense for secure remote access.

**Time Required**: 40 minutes

## Phase 1: Install Package

1. System → Package Manager
2. Available Packages → Search "openvpn"
3. Install "openvpn-client-export"

## Phase 2: Create Certificates

### Create Certificate Authority

System → Cert Manager → CAs → Add

```
Name: Lab-VPN-CA
Method: Create internal CA
Key length: 2048 bit
Lifetime: 3650 days
Common Name: lab-vpn-ca
```

### Create Server Certificate

System → Cert Manager → Certificates → Add

```
Name: pfSense-VPN-Server
CA: Lab-VPN-CA
Type: Server Certificate
```

### Create Client Certificate

```
Name: VPN-Client-User1
CA: Lab-VPN-CA
Type: User Certificate
```

## Phase 3: Configure OpenVPN Server

VPN → OpenVPN → Servers → Add

```
Server mode: Remote Access (SSL/TLS + User Auth)
Protocol: UDP on IPv4
Interface: WAN
Port: 1194

TLS Configuration: Auto-generate
Peer CA: Lab-VPN-CA
Server cert: pfSense-VPN-Server
Encryption: AES-256-CBC
Auth digest: SHA256

Tunnel Network: 10.8.0.0/24
Local networks: 192.168.1.0/24, 192.168.2.0/24
```

## Phase 4: Create VPN User

System → User Manager → Add

```
Username: vpnuser1
Password: VPNpass123
Certificate: Create user certificate
```

## Phase 5: Firewall Rules

### WAN Rule

Firewall → Rules → WAN → Add

```
Action: Pass
Protocol: UDP
Destination: WAN address
Port: 1194
```

### OpenVPN Rule

Firewall → Rules → OpenVPN → Add

```
Action: Pass
Protocol: Any
Source: any
Destination: any
```

## Phase 6: Port Forwarding

Power off pfSense VM → Settings → Network → Adapter 1 → Port Forwarding

```
Name: OpenVPN
Protocol: UDP
Host Port: 1194
Guest Port: 1194
```

## Phase 7: Export Client Config

VPN → OpenVPN → Client Export

Download "Most Clients" for vpnuser1

Edit .ovpn file:
```
Change: remote 10.0.2.15 1194
To: remote 127.0.0.1 1194
```

## Phase 8: Install Client

**Windows**: Download OpenVPN GUI, copy .ovpn to config folder

**Mac**: Install Tunnelblick

**Linux**: sudo openvpn --config file.ovpn

## Testing

From host PC:
```
ping 192.168.1.1
https://192.168.1.1 (browser)
http://192.168.2.10 (browser)
```

All should work when VPN connected.

---

Last Updated: March 2026
