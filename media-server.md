[Home](index.md) | [Media Server](media-server.md) | [Network](network.md) | [Lab](lab.md)

# Media Server & Proxmox Setup

## Proxmox Host Custom Build
- **CPU:** Intel i5-10400 (6c/12t)
- **RAM:** 16 GB DDR4
- **Storage:** 1TB NVMe SSD
- **OS:** Proxmox VE hosting Ubuntu VM for media services
- **GPU:** NVIDIA GTX 1650 for Plex transcoding
- **Media Services:** Plex Media Server runs in Docker on the Ubuntu Server VM.

---

## Lenovo T480 Docker Host
- **CPU:** Intel i7-8650U @ 1.90GHz (4c/8t)
- **RAM:** 16 GB DDR4
- **Storage:** 512 GB NVMe SSD
- **OS:** Ubuntu Server (bare metal)
- **Media Services:** Plex, Servarr (Sonarr, Radarr, Lidarr, Bazarr, Prowlarr), and qBittorrent run inside **Docker containers** within the Ubuntu Server VM. Gluetun provides VPN for secure downloads.
  - **Servarr Stack + qBittorrent + Gluetun** for media automation  
  - **Portainer** for Docker management  
  - **Tailscale** for secure remote access with VLAN 40 (Media) & VLAN 60 (Lab) open for use  

---

## Automation Workflow
1. Plex Watchlist → Radarr/Sonarr triggers → qBittorrent downloads via Gluetun VPN  
2. Files auto-organized via Docker stack → Bazarr adds subtitles  
3. **Prowlarr** provides indexer integration for all download requests  
4. Plex libraries auto-refresh with new media  



Below is the **Docker Compose configuration** for the Servarr Stack, including  
VPN isolation with Gluetun, and media automation tools.

```yaml
version: "3.9"

services:
  # GLUETUN VPN CONTAINER
  gluetun:
    image: qmcgaw/gluetun
    container_name: gluetun
    cap_add:
      - NET_ADMIN
    devices:
      - /dev/net/tun:/dev/net/tun
    environment:
      - VPN_SERVICE_PROVIDER=airvpn
      - VPN_TYPE=wireguard
      - WIREGUARD_PRIVATE_KEY=************
      - WIREGUARD_PRESHARED_KEY=************
      - WIREGUARD_ADDRESSES=XXX.XXX.XXX.XXX/XX
      - SERVER_COUNTRIES=Country
      - FIREWALL_VPN_INPUT_PORTS=XXXX
    ports:
      - 32400:32400   # Plex
      - 9080:8080     # qBittorrent WebUI
      - 7878:7878     # Radarr
      - 8989:8989     # Sonarr
      - 8686:8686     # Lidarr
      - 6767:6767     # Bazarr
      - 9696:9696     # Prowlarr
      - 56881:6881    # Torrent TCP
      - 56881:6881/udp# Torrent UDP
    restart: unless-stopped

  # PLEX MEDIA SERVER
  plex:
    image: lscr.io/linuxserver/plex:latest
    container_name: plex
    network_mode: "service:gluetun"
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Etc/UTC
    volumes:
      - /docker/plex/config:/config
      - /mnt/movies:/movies
      - /mnt/tv:/tv
      - /mnt/music:/music
    depends_on:
      - gluetun
    restart: unless-stopped

  # RADARR
  radarr:
    image: lscr.io/linuxserver/radarr:latest
    container_name: radarr
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Etc/UTC
    volumes:
      - /docker/servarr/radarr/config:/config
      - /mnt/movies:/movies
      - /mnt/media/downloads:/downloads
    network_mode: "service:gluetun"
    depends_on:
      - gluetun
    restart: unless-stopped

  # SONARR
  sonarr:
    image: lscr.io/linuxserver/sonarr:latest
    container_name: sonarr
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Etc/UTC
    volumes:
      - /docker/servarr/sonarr/config:/config
      - /mnt/tv:/tv
      - /mnt/media/downloads:/downloads
    network_mode: "service:gluetun"
    depends_on:
      - gluetun
    restart: unless-stopped

  # LIDARR
  lidarr:
    image: lscr.io/linuxserver/lidarr:latest
    container_name: lidarr
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Etc/UTC
    volumes:
      - /docker/servarr/lidarr/config:/config
      - /mnt/music:/music
      - /mnt/media/downloads:/downloads
    network_mode: "service:gluetun"
    depends_on:
      - gluetun
    restart: unless-stopped

  # BAZARR
  bazarr:
    image: lscr.io/linuxserver/bazarr:latest
    container_name: bazarr
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Etc/UTC
    volumes:
      - /docker/servarr/bazarr/config:/config
      - /mnt/movies:/movies
      - /mnt/tv:/tv
      - /mnt/music:/music
    network_mode: "service:gluetun"
    depends_on:
      - gluetun
    restart: unless-stopped

  # PROWLARR
  prowlarr:
    image: lscr.io/linuxserver/prowlarr:latest
    container_name: prowlarr
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Etc/UTC
    volumes:
       - /docker/servarr/prowlarr/config:/config
    network_mode: "service:gluetun"
    depends_on:
       - gluetun
    restart: unless-stopped

  # QBITTORRENT
  qbittorrent:
    image: lscr.io/linuxserver/qbittorrent:latest
    container_name: qbittorrent
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=America/Los_Angeles
    volumes:
      - /docker/servarr/qbittorrent/config:/config
      - /mnt/media/downloads:/downloads
    network_mode: "service:gluetun"
    depends_on:
      - gluetun
    restart: unless-stopped
```