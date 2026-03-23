# Troubleshooting Guide

## pfSense Issues

### Can't Access Web Interface

Check:
1. LAN VM on correct network (LAN_Network)
2. LAN VM has IP: `ip addr show`
3. Run: `sudo dhclient`
4. Ping gateway: `ping 192.168.1.1`

### No Internet Access

Check:
1. WAN adapter set to NAT
2. pfSense WAN has IP
3. Gateway configured
4. DNS servers set

## LAN VM Issues

### No IP Address

```bash
sudo dhclient -r
sudo dhclient
ip addr show
```

### Can't Reach Internet

```bash
# Check gateway
ip route show

# Test gateway
ping 192.168.1.1

# Check DNS
cat /etc/resolv.conf
```

## DMZ Server Issues

### No Network

```bash
# Check static IP
ip addr show

# Should see: 192.168.2.10/24

# Reconfigure if needed
sudo nano /etc/netplan/50-cloud-init.yaml
sudo netplan apply
```

### Apache Not Accessible

```bash
# Check Apache running
sudo systemctl status apache2
sudo systemctl start apache2

# Check firewall
sudo ufw status
sudo ufw disable

# Test locally
curl http://localhost
```

## Firewall Rule Issues

### DMZ Can Access LAN

CRITICAL ISSUE

1. Firewall → Rules → DMZ
2. Verify "Block DMZ → LAN" exists
3. Must be ABOVE allow rules
4. Diagnostics → States → Reset
5. Test immediately

### Changes Not Working

1. Click "Apply Changes"
2. Clear states
3. Test again

## VPN Issues

### Won't Connect

Check:
1. pfSense running
2. Port forwarding configured (UDP 1194)
3. .ovpn uses 127.0.0.1
4. Run client as Administrator

### Connected But No Access

Check:
1. VPN IP is 10.8.0.x
2. OpenVPN firewall rules exist
3. Ping 10.8.0.1
4. Tunnel network configured

---

Last Updated: March 2026
