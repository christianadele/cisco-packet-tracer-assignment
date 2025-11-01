# Cisco Packet Tracer: Secure Enterprise Network Simulation
Designed and implemented a secure small business network with multiple security zones, firewall protection, and controlled inter-VLAN routing using Cisco Packet Tracer.
## Network Architecture
![Network Topology](https://github.com/user-attachments/assets/7fb30d60-df26-4693-b38e-f4ec9a4d18b4)
## Network Components
- **VLAN 1 (Inside)**: User workstation network
- **VLAN 2 (Server)**: Database server segment  
- **ASA 5505 Firewall**: Perimeter security with NAT
- **Edge Router**: Internet gateway simulation
- **Access Control Lists**: Traffic filtering between zones
## Security Implementation
### Defense Strategy
This network implements **network segmentation** and **least privilege access** principles:
- Users isolated from servers via VLANs
- All inter-zone traffic inspected by ASA firewall
- Outbound traffic masked via NAT
- ACLs enforce explicit allow rules (implicit deny all)
### Key Security Features
#### 1. Firewall Configuration
- Stateful packet inspection enabled
- Zone-based security policies (inside/outside/DMZ)
- NAT/PAT for internal IP masking
#### 2. Access Control Lists
```cisco
! For Internal ASA Firewall (VLAN 1 = Inside, VLAN 2 = Server)
! ========================================
! -----------------------------
! 1. Allow inside users to reach anywhere (Internet + Server)
! -----------------------------
access-list INSIDE_OUT extended permit ip 192.168.10.0 255.255.255.0 any
! This allows all devices in the 192.168.10.0/24 network (inside VLAN)
! to initiate connections to any destination (servers or Internet).
! -----------------------------
! 2. Allow inside users to connect to DB Server via MySQL (TCP port 3306)
! -----------------------------
access-list INSIDE_OUT extended permit tcp 192.168.10.0 255.255.255.0 192.168.20.10 255.255.255.255 eq 3306
! This specifically permits PC1 and other inside devices
! to access the database server on TCP port 3306 (MySQL).
! -----------------------------
! 3. Allow server (DB) to reply only to established inside connections
! -----------------------------
access-list SERVER_IN extended permit tcp 192.168.20.10 255.255.255.255 192.168.10.0 255.255.255.0 established
! The “established” keyword ensures the server only sends responses
! to connections that were started by inside devices (not unsolicited traffic).
! -----------------------------
! 4. Deny any direct traffic from Server VLAN to Inside VLAN
! -----------------------------
access-list SERVER_IN extended deny ip 192.168.20.0 255.255.255.0 192.168.10.0 255.255.255.0
! This prevents the server VLAN (DMZ) from initiating new connections
! to the inside network — a key security control.
! -----------------------------
! 5. Deny all inbound Internet access to internal networks
! -----------------------------
access-list OUTSIDE_IN extended deny ip any 192.168.10.0 255.255.255.0
access-list OUTSIDE_IN extended deny ip any 192.168.20.0 255.255.255.0
! This blocks all traffic from the Internet trying to access
! your private internal or server networks.
! -----------------------------
! 6. Allow internal users to access the Internet (for NAT)
! -----------------------------
access-list OUTSIDE_NAT extended permit ip 192.168.10.0 255.255.255.0 any
! This ACL defines which internal addresses are allowed to be translated
! when going out to the Internet via NAT (Network Address Translation).
```
#### 3. VLAN Segmentation
- Logical separation between user and server networks
- Prevents lateral movement in case of compromise
- Controlled routing via firewall
## Configuration Examples

### ASA Firewall - Interface Setup
```cisco
interface Vlan1
 nameif inside
 security-level 100
 ip address 192.168.10.1 255.255.255.0
interface Vlan2
 nameif server
 security-level 50
 ip address 192.168.20.1 255.255.255.0
```
### NAT Configuration
```cisco
object network INSIDE_NET
 subnet 192.168.10.0 255.255.255.0
 nat (inside,outside) dynamic interface

object network SERVER_NET
 subnet 192.168.20.0 255.255.255.0
 nat (server,outside) dynamic interface

object network DB_SERVER
 host 192.168.20.10
 nat (server,inside) static 192.168.10.10
```
## Implementation Challenges & Solutions
| Challenge | Solution | Lesson Learned |
|-----------|----------|----------------|
| Cloud object wouldn't accept IP configuration | Redesigned using router to simulate internet | Physical devices have limitations in simulation |
| DHCP pool conflicts on ASA | Removed DHCP, assigned static IPs | Manual IP assignment provides more control for lab |
| PC1 couldn't ping DB Server despite correct VLAN config | Reviewed and corrected NAT exemption rules | NAT can interfere with internal routing if misconfigured |
| Duplicate ACL warnings | Cleaned up and reorganized access lists | ACL order and syntax matters for proper application |
## Testing & Validation
### Connectivity Tests Performed
- ✅ PC1 → Database Server (ICMP)
- ✅ Inter-VLAN routing via ASA
- ✅ ACL enforcement (blocked unauthorized traffic)
- 🔄 Outbound internet access (in progress)
### Current Status
**Phase 1 Complete**: Internal network connectivity and security controls verified.  
**Phase 2 In Progress**: Completing outbound NAT and internet access testing.
## Technical Specifications
- **Firewall**: Cisco ASA 5505
- **Router**: Cisco 2911
- **Switches**: Catalyst 2960
- **Addressing**: Private IP space (RFC 1918)
- **Protocols**: TCP/IP, NAT, ACLs, VLANs
## Next Steps
- [ ] Expand VLAN structure (HR, Finance departments)
- [ ] Implement object-group-based ACLs for scalability
- [ ] Add redundancy (backup ASA with failover)
- [ ] Configure Syslog for monitoring simulation
- [ ] Test static routing scenarios
## 📂 Files Included
- `Cisco Network Simulation Assignment.pkt` – Complete Packet Tracer simulation file
- `README.md` – Project documentation
## Tools & Skills Demonstrated
Cisco Packet Tracer | ASA Firewall Configuration | VLANs | NAT/PAT | Access Control Lists | Network Security Design | CLI Configuration | Troubleshooting
---

**Key Takeaway**: This project demonstrates practical understanding of network segmentation, firewall policies, and secure network design principles applicable to real enterprise environments.
