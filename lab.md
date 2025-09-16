[Home](index.md) | [Media Server](media-server.md) | [Network](network.md) | [Lab](lab.md)

# Windows Server & Azure Labs

## Windows Server 2022 (Lenovo P330)
- **CPU:** Intel Xeon E-2176G (6c/12t)
- **RAM:** 32 GB DDR4 ECC
- **Storage:** SSD for OS, 1 TB HDD for TrueNAS VM
- **OS:** Windows Server 2022 on VLAN 60
- **Role:** Active Directory Domain Services (AD DS), Group Policy Management

---

## Hyper-V & TrueNAS VM
- **Hyper-V VM** on Windows Server 2022 hosting **TrueNAS Core**
- Provides **NFS shares** for Movies, TV, Music
- Accessible by Plex, Servarr stack, and other lab services
- VLAN 40 for Media, VLAN 60 for Lab

---

## Microsoft Cloud Labs
- **Azure AD / Entra ID** for Identity & Access Management
- **Microsoft Intune** for Device Management & Compliance
- **Defender for Endpoint** for Security Testing
- **Hybrid AD** with Azure AD Connect planned for future testing

---

