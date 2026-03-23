# Secure Network Lab - Complete Fresh Start Guide

## 🎯 Project Overview

**What You're Building:**
A professional enterprise network with:
- 3 segmented networks (WAN/LAN/DMZ)
- pfSense firewall controlling all traffic
- Security policies enforcing isolation
- Web server in DMZ accessible from LAN only

**Time Estimate:** 2-3 hours total

**Skills Demonstrated:**
- Network design and segmentation
- Firewall configuration and policy management
- Security testing and validation
- Systems administration

---

## 💻 System Requirements

**Your PC:**
- **RAM:** 8 GB (you have this!) ✅
- **Disk Space:** 30 GB free
- **Software:** VirtualBox installed

**VM Allocation:**
```
pfSense:        1 GB RAM, 8 GB disk
LAN-User-VM:    1 GB RAM, 10 GB disk
DMZ-Server-VM:  1 GB RAM, 10 GB disk
────────────────────────────────────
Total:          3 GB RAM, 28 GB disk
```

**Host RAM usage:** ~5-6 GB total (leaves 2-3 GB headroom) ✅

---

## 📥 Part 0: Downloads & Preparation

### What You Need to Download:

**1. VirtualBox**
- Download from: https://www.virtualbox.org/
- Install with default settings

**2. pfSense ISO**
- Download from: https://www.pfsense.org/download/
- Select:
  - Architecture: AMD64 (64-bit)
  - Installer: DVD Image (ISO)
  - Mirror: Choose closest location
- File: ~650 MB (netgate-installer-xxx.iso)

**3. Ubuntu Server ISO**
- Download from: https://ubuntu.com/download/server
- Get: Ubuntu Server 24.04 LTS
- File: ~2.6 GB (ubuntu-24.04-live-server-amd64.iso)

**Save these to your Downloads folder!**

---

## 🏗️ Part 1: Create pfSense Firewall VM (30 minutes)

### Step 1: Create pfSense VM

**In VirtualBox:**

1. Click **"New"**

2. **Settings:**
   ```
   Name: pfSense-Firewall
   Type: BSD
   Version: FreeBSD (64-bit)
   ```

3. **Memory:** 1024 MB (1 GB)

4. **Hard Disk:** Create virtual hard disk now
   - Type: VDI
   - Storage: Dynamically allocated
   - Size: 8 GB

5. Click **"Create"**

---

### Step 2: Configure Network Adapters (CRITICAL!)

**Select pfSense-Firewall → Settings → Network**

**Adapter 1 (WAN):**
```
☑ Enable Network Adapter
Attached to: NAT
```

**Adapter 2 (LAN):**
```
☑ Enable Network Adapter
Attached to: Internal Network
Name: LAN_Network
```

**Adapter 3 (DMZ):**
```
☑ Enable Network Adapter
Attached to: Internal Network
Name: DMZ_Network
```

Click **OK**

---

### Step 3: Configure Graphics

**Settings → Display:**
```
Video Memory: 16 MB
Graphics Controller: VBoxVGA
☐ Enable 3D Acceleration [UNCHECKED]
```

---

### Step 4: Attach pfSense ISO

**Settings → Storage:**
1. Click "Empty" under Controller: IDE
2. Click CD icon (right side)
3. Choose pfSense ISO
4. Click **OK**

---

### Step 5: Install pfSense

**Start the VM**

1. **Accept** copyright notice (press Enter)

2. **Install pfSense** (select "Install", press Enter)

3. **Keymap:** Select your keyboard layout (usually "US")

4. **Partitioning:** Select "Auto (ZFS)"
   - Install: Install
   - Pool Type: stripe
   - Disk: Select the disk (press Space to mark with X)
   - Confirm: YES

5. **Wait 2-5 minutes** for installation

6. **Manual configuration:** NO

7. **Reboot:** Yes

**VM will reboot (ISO should auto-eject)**

---

### Step 6: Assign Interfaces

**After reboot, you'll see interface detection:**

```
Valid interfaces are:
em0  [MAC address]
em1  [MAC address]
em2  [MAC address]
```

**Answer these prompts:**

1. **VLANs:** `n` (no)

2. **WAN interface:** `em0`

3. **LAN interface:** `em1`

4. **Optional 1:** `em2`

5. **Optional 2:** (just press Enter - leave blank)

6. **Proceed?** Verify assignments look correct, then: `y`

**Wait 30-60 seconds for pfSense to configure interfaces**

---

### Step 7: Configure DMZ Interface

**You should see the main menu. Type: `2`** (Set interface(s) IP address)

1. **Which interface:** `3` (OPT1/DMZ)

2. **DHCP:** `n`

3. **IPv4 address:** `192.168.2.1`

4. **Subnet bits:** `24`

5. **Gateway:** (press Enter - none)

6. **IPv6 DHCP:** `n`

7. **IPv6 address:** (press Enter - skip)

8. **DHCP server on OPT1:** `n`

9. **Revert to HTTP:** `n`

**Wait for changes to apply**

---

### Step 8: Verify Configuration

**Main menu should now show:**

```
WAN  (wan)  → em0 → v4: 10.0.2.15/24 (or similar)
LAN  (lan)  → em1 → v4: 192.168.1.1/24
OPT1 (opt1) → em2 → v4: 192.168.2.1/24
```

**✅ If you see all three interfaces with IPs, pfSense is ready!**

---

## 🖥️ Part 2: Create LAN User VM (20 minutes)

### Step 1: Create LAN VM

**VirtualBox → New:**

```
Name: LAN-User-VM
Type: Linux
Version: Ubuntu (64-bit)
RAM: 1024 MB
Disk: 10 GB (VDI, dynamically allocated)
```

---

### Step 2: Configure Network

**Settings → Network:**

**Adapter 1:**
```
☑ Enable Network Adapter
Attached to: Internal Network
Name: LAN_Network
```

**Settings → Display:**
```
Graphics Controller: VBoxVGA
```

---

### Step 3: Attach Ubuntu Server ISO

**Settings → Storage:**
- Empty → CD icon → Choose Ubuntu Server ISO

---

### Step 4: Install Ubuntu Server

**Start VM and follow installer:**

1. **Language:** English
2. **Installer update:** Continue without updating
3. **Keyboard:** English (US)
4. **Install type:** Ubuntu Server
5. **Network:** Should show `192.168.1.xxx` from pfSense DHCP ✅
   - Tab to "Done"
6. **Proxy:** Leave blank
7. **Mirror:** Default
8. **Storage:** Use entire disk → Done → Continue
9. **Profile:**
   ```
   Your name: Lab User
   Server name: lan-vm
   Username: labuser
   Password: Password123
   ```
10. **Ubuntu Pro:** Skip
11. **SSH:** ☑ Install OpenSSH server (CHECK THIS!)
12. **Snaps:** None
13. **Wait 5-10 minutes** for installation
14. **Reboot**

---

### Step 5: Install GUI and Firefox

**After reboot, login:** labuser / Password123

**Install lightweight desktop:**

```bash
sudo apt update
sudo apt install --no-install-recommends xorg xfce4 firefox -y
```

**This takes ~10 minutes, downloads ~500MB**

**After installation:**
```bash
sudo reboot
```

**After reboot:** You'll see a graphical login screen!

**Login:** labuser / Password123

**✅ You now have a desktop with Firefox!**

---

### Step 6: Test pfSense Access

**Open Firefox in LAN VM**

**Navigate to:**
```
https://192.168.1.1
```

**Accept security warning:**
- Advanced → Accept Risk and Continue

**Login:**
```
Username: admin
Password: pfsense
```

**✅ If you see pfSense dashboard, LAN VM is working!**

---

### Step 7: Run pfSense Setup Wizard

**You'll see Setup Wizard - follow these steps:**

**Page 1:** Next

**Page 2:** Uncheck boxes → Next

**Page 3: General Information**
```
Hostname: pfsense
Domain: lab.local
Primary DNS: 8.8.8.8
Secondary DNS: 8.8.4.4
```
Next

**Page 4: Time Server**
```
Timezone: [Select your timezone]
```
Next

**Page 5: WAN Configuration**
```
☐ Block RFC1918 Private Networks [UNCHECK!]
☐ Block bogon networks [UNCHECK!]
```
Next

**Pages 6-10:** Keep clicking Next (accept defaults for static IP, DHCP client, PPPoE, PPTP)

**Page 11: LAN IP**
```
Keep: 192.168.1.1/24
```
Next

**Page 12: Admin Password**
```
Admin Password: Admin123
Confirm: Admin123
```
⚠️ **Write this down!**

Next

**Page 13:** Reload (wait 20 seconds)

**Page 14:** Finish

**✅ You're now logged into pfSense!**

---

### Step 8: Rename OPT1 to DMZ

**In pfSense web interface:**

1. **Interfaces → OPT1**
2. **Enable:** ☑ Enable interface
3. **Description:** `DMZ`
4. Scroll down → **Save**
5. **Apply Changes**

**✅ OPT1 is now called DMZ!**

---

## 🔥 Part 3: Configure Firewall Rules (15 minutes)

### Step 1: Add DMZ Firewall Rule (Temporary)

**First, we need DMZ to reach internet:**

**Firewall → Rules → DMZ**

1. Click **Add** (↑ arrow)
2. Fill in:
   ```
   Action: Pass
   Interface: DMZ
   Protocol: Any
   Source: DMZ net
   Destination: any
   Description: Allow DMZ to any (temporary)
   ```
3. **Save**
4. **Apply Changes**

**This allows DMZ to access internet while we build the server**

---

### Step 2: Configure LAN Firewall Rules

**Firewall → Rules → LAN**

**You'll see default IPv4 and IPv6 rules - delete both:**
1. Click trash icon for each
2. **Apply Changes**

**Now add specific rules:**

**Rule 1: Allow LAN to Any**

1. Click **Add** (↑ arrow)
2. Fill in:
   ```
   Action: Pass
   Interface: LAN
   Protocol: Any
   Source: LAN net
   Destination: any
   Description: Allow LAN to any
   ```
3. **Save**
4. **Apply Changes**

**For now, we'll keep it simple. We'll refine rules after testing.**

---

## 🖥️ Part 4: Create DMZ Server VM (25 minutes)

### Step 1: Create DMZ VM

**VirtualBox → New:**

```
Name: DMZ-Server-VM
Type: Linux
Version: Ubuntu (64-bit)
RAM: 1024 MB
Disk: 10 GB
```

---

### Step 2: Configure Network

**Settings → Network:**

**Adapter 1:**
```
☑ Enable Network Adapter
Attached to: Internal Network
Name: DMZ_Network
```

**Settings → Display:**
```
Graphics Controller: VBoxVGA
```

**Settings → Storage:**
- Attach Ubuntu Server ISO

---

### Step 3: Install Ubuntu Server

**Start VM and install:**

1. Language → Keyboard → Type (same as before)
2. **Network:** Will show interface but NO IP (normal)
   - Tab to Done
3. Storage, etc. (same as LAN VM)
4. **Profile:**
   ```
   Your name: DMZ Admin
   Server name: dmz-server
   Username: dmzadmin
   Password: Password123
   ```
5. **SSH:** ☑ Install OpenSSH server
6. Complete installation → Reboot

---

### Step 4: Configure Static IP

**After reboot, login:** dmzadmin / Password123

**Check interface name:**
```bash
ip addr show
```

**Note the interface name** (usually `enp0s3`)

**Edit netplan:**
```bash
sudo nano /etc/netplan/50-cloud-init.yaml
```

**Delete everything and replace with:**

```yaml
network:
  version: 2
  ethernets:
    enp0s3:
      addresses:
        - 192.168.2.10/24
      routes:
        - to: default
          via: 192.168.2.1
      nameservers:
        addresses:
          - 8.8.8.8
          - 8.8.4.4
```

⚠️ **Important:**
- Use spaces, not tabs
- Change `enp0s3` if your interface is different
- Indentation must be exact

**Save:** Ctrl+O, Enter, Ctrl+X

**Apply:**
```bash
sudo netplan apply
```

**Verify:**
```bash
ip addr show
```

**Should show:** `192.168.2.10/24` ✅

---

### Step 5: Test Connectivity

**Ping gateway:**
```bash
ping -c 4 192.168.2.1
```
**Should work!** ✅

**Ping internet:**
```bash
ping -c 4 8.8.8.8
```
**Should work!** ✅

---

### Step 6: Install Apache Web Server

```bash
sudo apt update
sudo apt install apache2 -y
```

**Wait 1-2 minutes**

**Check status:**
```bash
sudo systemctl status apache2
```

**Should say:** `active (running)` ✅

Press **Q** to quit

**Test locally:**
```bash
curl http://localhost
```

**Should see HTML!** ✅

---

### Step 7: Disable Ubuntu Firewall

**Critical step!**

```bash
sudo ufw status
```

**If active:**
```bash
sudo ufw disable
```

**This ensures pfSense controls all traffic, not Ubuntu's firewall**

---

## ✅ Part 5: Test Everything (10 minutes)

### Test 1: LAN to DMZ HTTP ✅

**From LAN-User-VM, open Firefox:**

**Navigate to:**
```
http://192.168.2.10
```

**Expected:** Apache2 Ubuntu Default Page ✅

**Also test from terminal:**
```bash
curl http://192.168.2.10
```

**Expected:** HTML output ✅

---

### Test 2: LAN to Internet ✅

**From LAN-User-VM:**

```bash
ping -c 4 8.8.8.8
ping -c 4 google.com
```

**Expected:** Both work ✅

---

### Test 3: DMZ to LAN ❌ (Should Fail)

**We need to add the blocking rule first!**

**Go to pfSense: Firewall → Rules → DMZ**

**Delete the "Allow DMZ to any" rule**

**Add two new rules:**

**Rule 1: Block DMZ to LAN**
1. Click **Add** (↑ arrow - add to top)
2. Fill in:
   ```
   Action: Block
   Interface: DMZ
   Protocol: Any
   Source: DMZ net
   Destination: LAN net
   Description: Block DMZ to LAN
   ```
3. **Save**

**Rule 2: Allow DMZ to Internet**
1. Click **Add** (↓ arrow - add to bottom)
2. Fill in:
   ```
   Action: Pass
   Interface: DMZ
   Protocol: Any
   Source: DMZ net
   Destination: WAN net
   Description: Allow DMZ to Internet
   ```
3. **Save**
4. **Apply Changes**

**Now test from DMZ-Server-VM:**

```bash
ping -c 4 192.168.1.1
```

**Expected:** No response / timeout ❌ (GOOD!)

```bash
ping -c 4 8.8.8.8
```

**Expected:** Works ✅ (DMZ can still reach internet)

---

## 📊 Final Test Results

**All tests should show:**

| Test | Source | Destination | Expected | Result |
|------|--------|-------------|----------|--------|
| 1 | LAN | DMZ Web (80) | ✅ PASS | |
| 2 | LAN | Internet | ✅ PASS | |
| 3 | DMZ | LAN | ❌ BLOCKED | |
| 4 | DMZ | Internet | ✅ PASS | |

**✅ If all tests match expected results: PROJECT COMPLETE!**

---

## 📸 Documentation

### Screenshots to Take:

1. **pfSense Dashboard** (showing all 3 interfaces)
2. **Firewall → Rules → LAN** (your LAN rules)
3. **Firewall → Rules → DMZ** (your DMZ rules)
4. **Firefox showing Apache page** (http://192.168.2.10)
5. **Terminal showing successful LAN to DMZ curl**
6. **Terminal showing blocked DMZ to LAN ping**
7. **Network topology diagram** (draw in draw.io or similar)

Save to: `screenshots/`

---

## 📝 Create GitHub Repository

### Repository Structure:

```
secure-network-lab/
├── README.md
├── docs/
│   ├── network-design.md
│   ├── firewall-rules.md
│   └── testing-results.md
├── diagrams/
│   └── network-topology.png
├── screenshots/
│   ├── pfsense-dashboard.png
│   ├── lan-rules.png
│   ├── dmz-rules.png
│   └── testing/
└── configs/
    └── notes.txt
```

---

### README Template:

```markdown
# Secure Network Lab

## Overview
A virtualized enterprise network demonstrating network segmentation, 
firewall policies, and defense-in-depth security architecture.

## Architecture

### Network Topology
```
Internet (NAT)
      |
   [WAN] ← pfSense Firewall
      |          |
   [LAN]      [DMZ]
      |          |
 User VM    Web Server
```

### IP Addressing
- **WAN**: 10.0.2.0/24 (NAT - Dynamic)
- **LAN**: 192.168.1.0/24 (DHCP: .100-.200)
- **DMZ**: 192.168.2.0/24 (Static)

### Systems
- **pfSense**: Firewall/Router (1GB RAM)
- **LAN-User-VM**: Ubuntu Server 24.04 + XFCE (1GB RAM)
- **DMZ-Server-VM**: Ubuntu Server 24.04 + Apache (1GB RAM)

## Security Controls

### Network Segmentation
- Three isolated security zones (WAN/LAN/DMZ)
- Zone-based firewall policies
- Default-deny posture

### Firewall Rules

**LAN Rules:**
- Allow LAN → Any (all protocols)

**DMZ Rules:**
- Block DMZ → LAN (all protocols)
- Allow DMZ → Internet (all protocols)

### Testing Results
✅ LAN can access DMZ web services  
✅ LAN can access internet  
❌ DMZ cannot reach LAN (isolated)  
✅ DMZ can reach internet (for updates)

## Technologies
- **Virtualization**: VirtualBox
- **Firewall**: pfSense (FreeBSD-based)
- **Operating System**: Ubuntu Server 24.04 LTS
- **Web Server**: Apache2
- **Desktop Environment**: XFCE4

## Skills Demonstrated
- Network design and architecture
- Firewall configuration and management
- Security policy implementation
- Linux system administration
- Virtual infrastructure deployment
- Security testing and validation

## Setup Instructions
See [docs/setup-guide.md](docs/setup-guide.md)

## Future Enhancements
- [ ] OpenVPN remote access
- [ ] Suricata IDS/IPS
- [ ] Centralized logging (ELK stack)
- [ ] Network monitoring (Grafana)
- [ ] Additional DMZ services
```

---

## 🎓 Interview Talking Points

### Technical Discussion Points:

**Network Design:**
- "I designed a three-tier network architecture with isolated security zones"
- "Used internal networks in VirtualBox to simulate physical network segregation"
- "Each zone has a specific security purpose - LAN for users, DMZ for public services"

**Firewall Configuration:**
- "Implemented zone-based firewall policies in pfSense"
- "Configured default-deny posture with explicit allow rules"
- "Blocked lateral movement from DMZ to LAN to contain potential breaches"

**Security Principles:**
- "Applied defense-in-depth with multiple security layers"
- "Used principle of least privilege - only allowing necessary traffic"
- "DMZ isolation prevents compromised web server from attacking internal network"

**Testing & Validation:**
- "Systematically tested each security control"
- "Validated that DMZ isolation works as intended"
- "Documented expected vs actual behavior for each test case"

### Why Questions:

**Q: Why segment the network?**
A: "To contain security breaches and limit blast radius. If the DMZ web server is compromised, the attacker can't pivot to internal systems."

**Q: Why block DMZ → LAN?**
A: "DMZ hosts public-facing services that are at higher risk. If compromised, we don't want attackers reaching our trusted internal network."

**Q: Why allow DMZ → Internet?**
A: "Legitimate operational need - servers need to download security updates and patches. But we could further restrict this to specific update servers."

**Q: What would you do differently in production?**
A: "Add IDS/IPS for threat detection, implement logging and monitoring, use more granular rules (restrict DMZ → Internet to specific ports), add VPN for remote access, implement VLAN tagging for additional separation."

---

## 🎯 Project Complete Checklist

**Infrastructure:**
- [ ] pfSense VM created and configured
- [ ] LAN-User-VM created with GUI
- [ ] DMZ-Server-VM created with Apache
- [ ] All network adapters configured correctly
- [ ] All VMs have correct IP addresses

**Firewall Rules:**
- [ ] LAN rules configured
- [ ] DMZ rules configured
- [ ] Rules tested and validated
- [ ] Screenshots captured

**Testing:**
- [ ] Test 1: LAN → DMZ HTTP ✅
- [ ] Test 2: LAN → Internet ✅
- [ ] Test 3: DMZ → LAN ❌
- [ ] Test 4: DMZ → Internet ✅

**Documentation:**
- [ ] Screenshots organized
- [ ] Network diagram created
- [ ] GitHub repository created
- [ ] README written
- [ ] Testing results documented

**Knowledge:**
- [ ] Can explain network segmentation
- [ ] Can explain firewall rule logic
- [ ] Can describe security benefits
- [ ] Can discuss improvements

---

## 🐛 Troubleshooting Guide

### Issue: VM won't start / graphics errors
**Solution:**
- Settings → Display → Graphics Controller: VBoxVGA
- Disable 3D acceleration

### Issue: LAN VM not getting IP
**Solution:**
```bash
sudo dhclient
ip addr show
```
Verify pfSense DHCP is enabled on LAN

### Issue: DMZ can't reach internet
**Solution:**
1. Check static IP config: `ip addr show`
2. Check gateway: `ip route show`
3. Reapply netplan: `sudo netplan apply`
4. Check pfSense DMZ rules

### Issue: Can't access DMZ web server from LAN
**Solution:**
1. Check Apache running: `sudo systemctl status apache2`
2. Check Ubuntu firewall: `sudo ufw status` (should be inactive)
3. Test locally on DMZ: `curl http://localhost`
4. Reset pfSense states: Diagnostics → States → Reset States
5. Check pfSense LAN rules allow traffic to DMZ

### Issue: DMZ CAN reach LAN (shouldn't!)
**Solution:**
1. Check DMZ rules - must have "Block DMZ to LAN" rule
2. Rule must be ABOVE "Allow DMZ to Internet" rule
3. Make sure destination is "LAN net" not "any"
4. Apply changes after modifying rules

---

## 💾 Saving Your Work

### To Pause:

**Option 1: Save State (Recommended)**
- Select VM → Machine → Save State
- Freezes exactly where you are
- Quick resume

**Option 2: Shut Down**
- Ubuntu VMs: `sudo poweroff`
- pfSense: Type `6` → `y`

### To Resume:

1. Start pfSense first
2. Wait 1-2 minutes for full boot
3. Start LAN and DMZ VMs
4. Everything works as before!

---

## 🚀 Next Steps (Optional Phase 3)

Once comfortable with Milestone #2:

**Advanced Features:**
1. **OpenVPN Server** - Remote access VPN
2. **Suricata IDS** - Intrusion detection
3. **Grafana Monitoring** - Traffic visualization
4. **Centralized Logging** - Syslog or ELK
5. **Additional Services** - DNS, DHCP reservations
6. **VLAN Tagging** - Advanced segmentation

**But first - celebrate!** You've built a production-grade lab! 🎊

---

## ✅ Success Criteria

**You've successfully completed this project when:**

- ✅ All 3 VMs running and networked
- ✅ LAN can access DMZ web server
- ✅ DMZ is isolated from LAN
- ✅ All security tests pass as expected
- ✅ Screenshots and documentation complete
- ✅ Can explain design decisions and security benefits
- ✅ GitHub repository published

**This is a portfolio-ready, interview-worthy project!** 🏆

---

## 📚 Resources

**pfSense:**
- Official Docs: https://docs.netgate.com/pfsense/
- Forums: https://forum.netgate.com/

**Ubuntu Server:**
- Official Docs: https://ubuntu.com/server/docs
- Netplan Guide: https://netplan.io/

**Networking:**
- Subnet Calculator: https://www.subnet-calculator.com/
- Draw.io: https://app.diagrams.net/

**VirtualBox:**
- Manual: https://www.virtualbox.org/manual/

---

## 🎓 Learning Outcomes

By completing this project, you've demonstrated:

**Technical Skills:**
- Virtual infrastructure deployment
- Network design and implementation
- Firewall configuration and management
- Linux system administration
- Security policy implementation
- Testing and validation methodologies

**Security Concepts:**
- Network segmentation
- Defense in depth
- Least privilege principle
- Zone-based security
- Default-deny philosophy
- Threat containment

**Professional Skills:**
- Technical documentation
- Systematic troubleshooting
- Project planning and execution
- Version control (Git/GitHub)

**Perfect for:**
- Network Engineer positions
- Security Analyst roles
- Systems Administrator jobs
- DevOps positions
- IT Support roles

---

## 🎊 Congratulations!

You've built a professional-grade network security lab from scratch!

This project demonstrates real-world skills that employers value:
- Practical hands-on experience
- Security-focused thinking
- Systematic approach to problems
- Clear documentation
- Portfolio presence

**You should be proud of this accomplishment!** 🏆

Now go ace those interviews! 💪

---

**Questions or issues? Review the troubleshooting section or start fresh with these steps!**

**Good luck!** 🚀
