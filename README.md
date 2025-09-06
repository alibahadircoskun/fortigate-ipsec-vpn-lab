# IPSec Site-to-Site VPN Lab
## Multi-Vendor Network Infrastructure with Cisco & Fortinet

![Network Topology](topology-diagram.png)

## Lab Overview

This lab demonstrates a real-world enterprise network scenario featuring an IPSec site-to-site VPN connection between headquarters (HQ) and a branch office. The lab showcases multi-vendor integration using Cisco switching infrastructure and Fortinet security appliances.

## Network Architecture

### Headquarters (HQ) Site
- Fortinet Firewall (HQ-FW): 203.0.113.0/30 (WAN) | 192.168.1.0/30 (LAN)
- Core Switch (HQ-COR-SW): Layer 3 routing and inter-VLAN communication
- Access Switches: HQ-SW1, HQ-SW2, HQ-SW3
- Server: AD-FILE-DNS-SERVER (Active Directory, File Services, DNS)
- End Devices: 4 client workstations across multiple VLANs

### Branch Office Site
- Fortinet Firewall (BR-FW): 198.51.100.0/30 (WAN) | 10.10.10.0/30 (LAN)
- Access Switch (BR-SW): Layer 2 switching
- End Devices: 4 client workstations

### VLANs Configuration
| VLAN | Name | Network | Location |
|------|------|---------|----------|
| 10 | Management | 192.168.10.0/24 | HQ |
| 20 | Sales | 192.168.20.0/24 | HQ |
| 30 | Engineering | 192.168.30.0/24 | HQ |
| 40 | Branch-LAN | 10.10.40.0/24 | Branch |

## Technologies Implemented

### Security
- IPSec Site-to-Site VPN between HQ and Branch offices
- Fortinet FortiGate Firewalls for network security and VPN termination
- Network Access Control and traffic filtering

### Switching & Routing
- Cisco Layer 3 Switching for inter-VLAN routing at HQ
- VLAN Segmentation for network organization
- STP (Spanning Tree Protocol) for loop prevention
- Static/Dynamic Routing configuration

### Services
- Windows Server 2019 with multiple roles:
  - Active Directory Domain Services (AD DS)
  - DNS Server for name resolution
  - File Server for centralized file storage
- DHCP Services for automatic IP assignment

## Lab Objectives

1. Configure IPSec VPN tunnel between FortiGate firewalls
2. Implement secure site-to-site connectivity
3. Set up inter-VLAN routing on Cisco switches
4. Deploy Windows Server infrastructure services:
   - Install and configure Active Directory Domain Services
   - Set up DNS for internal name resolution
   - Implement file sharing services with proper access controls
   - Configure services to work across multiple VLANs
5. Establish network segmentation using VLANs:
   - Create dedicated subnets for management, sales, and engineering departments
   - Configure VLAN interfaces and routing between segments
6. Test end-to-end connectivity across the VPN tunnel:
   - Verify network connectivity between sites
   - Test domain authentication for branch office users
   - Validate file sharing access across the VPN
7. Implement security policies and access controls:
   - Configure firewall rules for inter-VLAN communication
   - Set up VPN access policies
   - Test security boundaries between network segments

## Configuration Highlights

### FortiGate VPN Configuration
```bash
# HQ FortiGate IPSec Configuration
config vpn ipsec phase1-interface
    edit "HQtoBR"
        set interface "port3"
        set peertype any
        set proposal des-md5 des-sha1
        set wizard-type static-fortigate
        set remote-gw 198.51.100.1
        set psksecret "********"
    next
end

config vpn ipsec phase2-interface
    edit "HQtoBR"
        set phase1name "HQtoBR"
        set proposal des-md5 des-sha1
        set src-addr-type name
        set dst-addr-type name
        set src-name "HQtoBR_local"
        set dst-name "HQtoBR_remote"
    next
end
```

### Cisco Switch VLAN Configuration
```cisco
! HQ Core Switch Configuration
vlan 10
 name Management
vlan 20
 name Sales
vlan 30
 name Engineering

interface vlan 10
 ip address 192.168.10.1 255.255.255.0
interface vlan 20
 ip address 192.168.20.1 255.255.255.0
interface vlan 30
 ip address 192.168.30.1 255.255.255.0

ip routing
```

## Images Used

| Device Type | Model | Quantity | Purpose |
|-------------|-------|----------|---------|
| Firewall | Fortinet FortiGate | 2 | VPN termination & security |
| Core Switch | Cisco | 1 | L3 routing & VLAN management |
| Access Switch | Cisco | 4 | End-device connectivity |
| Server | Windows Server 2019 | 1 | AD, DNS, File services |
| Workstations | Windows 7 | 8 | End-user devices |

## Repository Structure

```
├── configs/
│   ├── fortigate/
│   │   ├── hq-fw-config.conf
│   │   └── br-fw-config.conf
│   ├── cisco/
│   │   ├── hq-core-sw.cfg
│   │   ├── hq-access-sw.cfg
│   │   └── br-sw.cfg
├── documentation/
│   ├── network-diagram.png
│   ├── ip-addressing.xlsx
│   └── troubleshooting-guide.md
└── README.md
```
