# Cisco Packet Tracer: Network Simulation Project
This project is a simulation of a small business network setup using Cisco Packet Tracer. It includes internal and external firewalls, VLAN segmentation, NAT configuration, and end-to-end connectivity testing between PCs, servers, and the simulated internet.
## What I Built
- VLAN 1: Inside (PC1 and Switch)
- VLAN 2: Server (Database Server)
- ASA Firewall with NAT Configuration
- Router simulating internet
- ACLs controlling traffic between internal, DMZ, and external
## Challenges I Faced
Couldn’t configure IP on cloud: forced me to redesign
DHCP errors on ASA: had to delete the pool and assign IPs manually
No IP assignment GUI on ASA: I had to use the CLI for everything
Access-lists: kept giving duplicate warnings until I cleaned them properly
PC1 Not Pinging the DB Server: Although I was confident that my VLAN setup and IP assignments were correct, PC1 was still unable to ping the DB Server. I guess it's because the NAT rule was not applying to the external ASA
Confusing layout at first: needed a diagram to visualise better
NAT to Internet Still Incomplete: Although NAT rules were added, I haven’t completed a full test of outbound internet access (PC1 → Internet). This is because I focused more on resolving internal connectivity issues first.
## Lessons Learned
- VLAN interface names must be configured, not physical Ethernet interfaces directly.
- Every interface connected to a VLAN must be assigned properly on the ASA.
- ASA requires VLANs for nameif, not physical ports.
- Even if "up" shows, protocol can still be down if cables/interfaces are wrong.
## 📸 Screenshot
![Network Diagram] <img width="956" height="472" alt="image" src="https://github.com/user-attachments/assets/7fb30d60-df26-4693-b38e-f4ec9a4d18b4" />
## 📂 Files Included
- `network-assignment.pkt` – the full Packet Tracer simulation file
- `network-diagram.png` – screenshot of the topology
- `README.md` – this documentation
  ## 🧠 Next Steps
  Create separate VLANs for each PC (e.g., HR, Finance)
Implement proper ACLs: Only specific traffic is allowed
Reconfigure NAT rule for internet access from user PCs
Add a backup router or ASA (failover path)
Include basic Syslog or SNMP to simulate monitoring
Use object-group-based ACLs instead of “any any”
Add static routes to test route learning and control
