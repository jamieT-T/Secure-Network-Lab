# Testing Guide

## Overview

Systematic testing procedures to validate security controls.

## Test Cases

### TC-01: LAN to Internet

From LAN VM:
```bash
ping -c 4 8.8.8.8
ping -c 4 google.com
```

Expected: Success

### TC-02: LAN to DMZ Web

From LAN VM Firefox:
```
http://192.168.2.10
```

Expected: Apache page displays

### TC-03: DMZ to LAN (Should Fail)

From DMZ Server:
```bash
ping -c 4 192.168.1.1
ping -c 4 192.168.1.100
```

Expected: No response (blocked)

### TC-04: DMZ to Internet

From DMZ Server:
```bash
ping -c 4 8.8.8.8
```

Expected: Success

### TC-05: VPN Access

From host PC (VPN connected):
```bash
ping 192.168.1.1
```

Browser: https://192.168.1.1

Expected: Both work

## Test Results

| Test | Expected | Actual | Status |
|------|----------|--------|--------|
| TC-01 | Allow | | |
| TC-02 | Allow | | |
| TC-03 | Block | | |
| TC-04 | Allow | | |
| TC-05 | Allow | | |

Target: 100% pass rate

---

Last Updated: March 2026
