# Network Setup  

## Hardware & Topology
- **Router/Firewall:** UniFi Dream Machine Pro  
- **Access Points:** UniFi AP Lite  
- **Switch:** Managed VLAN-capable switch  

## VLAN Segmentation
- **VLAN 10:** Admin  
- **VLAN 20:** IoT  
- **VLAN 40:** Media  
- **VLAN 50:** Guest  
- **VLAN 60:** Lab  

## Firewall Rules
- Admin VLAN can access all others  
- Media VLAN → outbound traffic only, isolated from Admin  

## Remote Access
- **Tailscale:** Secure VPN for remote SSH/RDP access  
