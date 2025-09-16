[Home](index.md) | [Media Server](media-server.md) | [Network](network.md) | [Lab](lab.md)

# Network Setup & VLAN Configuration

## UniFi Dream Machine Pro & Network Hardware
- **Router/Firewall:** UniFi Dream Machine Pro (UDM Pro)
- **Access Points:** UniFi AP Lite for wireless coverage
- **Switch:** UniFi managed switch for VLAN segmentation and routing

---

## VLAN Configuration Table

<table>
  <thead>
    <tr>
      <th>VLAN</th>
      <th>Name</th>
      <th>Purpose</th>
      <th>Access Rules</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>10</td>
      <td>IoT</td>
      <td>Cameras, smart devices</td>
      <td>Isolated; no access except Admin (20)</td>
    </tr>
    <tr>
      <td>20</td>
      <td>Admin</td>
      <td>Management devices</td>
      <td>Full access to all VLANs</td>
    </tr>
    <tr>
      <td>25</td>
      <td>Printer</td>
      <td>Printers only</td>
      <td>Isolated; only from VLAN 30 or Admin (20)</td>
    </tr>
    <tr>
      <td>30</td>
      <td>Work</td>
      <td>PCs, laptops, work devices</td>
      <td>One-way → IoT (10), Printer (25) only</td>
    </tr>
    <tr>
      <td>40</td>
      <td>Media</td>
      <td>Plex, Servarr stack, TrueNAS</td>
      <td>Isolated except Admin (20)</td>
    </tr>
    <tr>
      <td>50</td>
      <td>Guest</td>
      <td>Guest WiFi</td>
      <td>Internet only, no inter-VLAN</td>
    </tr>
    <tr>
      <td>60</td>
      <td>Lab</td>
      <td>Windows Server 2022, Azure Labs</td>
      <td>Isolated except Admin (20)</td>
    </tr>
  </tbody>
</table>

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
