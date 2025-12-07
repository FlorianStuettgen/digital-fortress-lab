# 02 Interface Segmentation & VLAN Mapping

## Objective

Design and implement secure and structured network segmentation across the lab environment. Isolate zones, reduce attack surface, simplify monitoring, and enforce least-privilege access between lab, production, and management networks.

---

## 1. Zone Definitions and Security Levels

| Zone       | ASA Interface | SonicWall Interface | Security Level | Purpose               | VLAN / Notes  |
| ---------- | ------------- | ------------------- | -------------- | --------------------- | ------------- |
| Outside    | G0/0          | X0                  | 0              | Untrusted Internet    | VLAN 10       |
| Inside     | G0/1          | X1                  | 100            | Trusted LAN           | VLAN 20       |
| DMZ        | G0/2          | X2                  | 50             | Semi-trusted Services | VLAN 30       |
| VPN        | G0/3          | X3                  | 70             | Remote Access         | VPN Subnet    |
| Management | G0/4          | X4                  | 90             | Admin only            | Isolated VLAN |
| Lab/Dev    | G0/5          | X5                  | 60             | Testing & Development | VLAN 40       |

**Best Practices:**

* Assign security levels to enforce traffic rules (higher security cannot initiate traffic to lower unless explicitly allowed).
* Use descriptive names and document all interfaces.
* Keep lab/dev traffic fully isolated from production networks.
* Ensure management VLAN is restricted and monitored.

---

## 2. ASA Interface Configuration Examples

```bash
interface GigabitEthernet0/0
 nameif outside
 security-level 0
 ip address dhcp setroute
!
interface GigabitEthernet0/1
 nameif inside
 security-level 100
 ip address 192.168.1.1 255.255.255.0
!
interface GigabitEthernet0/2
 nameif dmz
 security-level 50
 ip address 192.168.2.1 255.255.255.0
!
interface GigabitEthernet0/3
 nameif vpn
 security-level 70
 ip address 192.168.3.1 255.255.255.0
!
interface GigabitEthernet0/4
 nameif management
 security-level 90
 ip address 192.168.4.1 255.255.255.0
!
interface GigabitEthernet0/5
 nameif lab
 security-level 60
 ip address 192.168.5.1 255.255.255.0
```

---

## 3. SonicWall Interface Configuration Examples

```bash
interface X0 name outside ip dhcp
interface X1 name inside ip 192.168.1.1/24
interface X2 name dmz ip 192.168.2.1/24
interface X3 name vpn ip 192.168.3.1/24
interface X4 name management ip 192.168.4.1/24
interface X5 name lab ip 192.168.5.1/24
```

---

## 4. VLAN Tagging & Trunking

**Purpose:** Allow multiple VLANs across shared physical links while maintaining isolation.

**Best Practices:**

* Use 802.1Q trunking for switches connecting multiple zones.
* Tag VLANs on all trunk ports; untagged ports should be assigned to specific access VLANs.
* Ensure VLAN IDs are consistent across ASA, SonicWall, and switches.
* Implement PVLANs for additional micro-segmentation if required.
* Restrict VLAN propagation to only the necessary switches.
* Apply ACLs at VLAN boundaries for granular control.

---

## 5. Inter-Zone Traffic Control

**Best Practices:**

* Enforce firewall policies between zones; higher-security zones should restrict access to lower-security zones.
* Lab/dev traffic should not reach production or management unless explicitly allowed.
* DMZ should have tightly controlled access to internal networks and monitored for anomalies.
* VPN users should have segmented access based on roles and project requirements.
* Implement logging for all inter-zone traffic for auditing.
* Use object groups to simplify rules and policy management.

---

## 6. Redundancy & High Availability

**Purpose:** Ensure resilient network operations in the lab environment.

**Recommendations:**

* Configure redundant ASA/Firewall pairs with failover where supported.
* Use redundant uplinks with proper spanning-tree or routing configuration.
* Maintain synchronized VLAN and interface configurations across redundant devices.
* Test failover regularly to validate segmentation and access policies.

---

## 7. Documentation & Lab Inventory

* Maintain a **network diagram** showing all interfaces, zones, and VLAN IDs.
* Keep a **VLAN-to-interface mapping table** for quick reference.
* Document **interface IPs, subnets, and usage**.
* Record changes in a **change-control log** to track interface and VLAN modifications.
* Include **ACL references and NAT interactions** relevant to each zone.

---

## References

* Cisco ASA 5510/5515-X Configuration Guides
* SonicWall NSa Series Security Configuration Guides
* NIST Cybersecurity Framework – Network Segmentation
* CIS Benchmarks for Firewall and VLAN Configuration
