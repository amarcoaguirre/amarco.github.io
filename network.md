[Home](index.md) | [Media Server](media-server.md) | [Network](network.md) | [Lab](lab.md)

# Network Setup & VLAN Configuration

## UniFi Dream Machine Pro & Network Hardware
- **Router/Firewall:** UniFi Dream Machine Pro (UDM Pro)
- **Access Points:** UniFi AP Lite for wireless coverage
- **Switch:** UniFi managed switch for VLAN segmentation and routing

---

## VLAN Configuration Table
| VLAN | Name     | Purpose                        | Access Rules                                  |
|------|----------|-------------------------------|----------------------------------------------|
| 10   | IoT      | Cameras, smart devices         | Isolated; no access except Admin (20)         |
| 20   | Admin    | Management devices             | Full access to all VLANs                      |
| 25   | Printer  | Printers only                  | Isolated; only from VLAN 30 or Admin (20)     |
| 30   | Work     | PCs, laptops, work devices      | One-way → IoT (10), Printer (25) only         |
| 40   | Media    | Plex, Servarr stack, TrueNAS    | Isolated except Admin (20)                    |
| 50   | Guest    | Guest WiFi                      | Internet only, no inter-VLAN                  |
| 60   | Lab      | Windows Server 2022, Azure Labs | Isolated except Admin (20)                    |

---

## Firewall Rules Overview
- **Admin VLAN (20)** → Full access to all VLANs  
- **Work VLAN (30)** → One-way access to IoT (10) & Printer (25) only  
- **IoT VLAN (10)** → Isolated, only outbound internet access or responses to Work and Admin VLAN requests  
- **Printer VLAN (25)** → Isolated, responses only to Work VLAN or Admin VLAN  
- **Media VLAN (40)** → Restricted to Admin VLAN for management  
- **Lab VLAN (60)** → Restricted to Admin VLAN for management  
- **Guest VLAN (50)** → Internet only, no inter-VLAN access  

---

## Remote Access
- Remote administration via VPN Tailscale  for VLAN 40 (Media) & VLAN 60 (Lab)  
- Admin VLAN used for all management tasks to maintain strict segmentation  

---

## Future Enhancements
- Advanced firewall logging & alerting  
- Site-to-site VPN for secure external access 
