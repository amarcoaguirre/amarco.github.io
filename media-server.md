# Proxmox & Media Server Setup  

## Proxmox Hypervisor
- **Hardware:** Lenovo ThinkStation P330 (Xeon E-2176G, 32GB RAM)  
- **VMs Hosted:**  
  - Ubuntu VM → Plex Media Server  
  - Ubuntu VM → Servarr Stack (Sonarr, Radarr, Lidarr, Bazarr, Prowlarr)  
  - qBittorrent container with Gluetun VPN for secure downloads  
  - TrueNAS VM for 1TB storage (NFS shares for movies, TV, music)  

## Plex & Automation
1. **Plex** for centralized media streaming  
2. **Sonarr/Radarr/Lidarr** for automated media acquisition  
3. **qBittorrent + Gluetun** for VPN-isolated torrenting  
4. **Bazarr** for automatic subtitles  
5. **Prowlarr** for indexer management  

## Workflow Overview
- Plex Watchlist → Radarr/Sonarr triggers → qBittorrent downloads → Files renamed & sorted → Plex auto-refreshes  

## Future Plans
- Add Trakt.tv sync  
- Automated library backups  
- Monitoring with Grafana + Prometheus  
