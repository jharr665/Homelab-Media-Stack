# Homelab-Media-Stack
# Homelab Media Stack — Servarr + Jellyfin + Gluetun (with Authentik SSO)

A **provider-agnostic, production-ready** media stack: *Arr apps + Jellyfin, with a **fail-closed** torrent client bound to **Gluetun** (OpenVPN/WireGuard). Optional extras include **FlareSolverr**, **Striparr**, **Tdarr**, **Navidrome**, **Calibre-Web**, **Paperless-ngx**, **Immich**, and **Caddy+Authentik** for Single Sign-On.

> ⚠️ **Disclaimer**: This project is for educational purposes. Know your local laws and your tracker/indexer rules when using torrents.

---

## ✨ Highlights (TL;DR)
- **Fail-closed VPN**: qBittorrent is **namespace-bound** to Gluetun. If the VPN drops, torrent traffic stops.
- **Provider-agnostic**: Gluetun supports OpenVPN/WireGuard with many VPNs.
- **Full Servarr suite**: Prowlarr, Sonarr, Radarr, Readarr, Lidarr, Bazarr.
- **Media server & requests**: Jellyfin + Jellyseerr, with optional GPU transcoding.
- **SSO-ready**: Caddy + Authentik ForwardAuth, domain-driven hostnames.
- **Modular profiles**: Enable extras per profile (tdarr, navidrome, paperless, immich, etc.).

---

## 🧭 Architecture

```mermaid
flowchart LR
  %% --- Subgraphs ---
  subgraph Internet
    U["User Browser"];
  end

  subgraph Host
    Caddy["Caddy (TLS + Authentik ForwardAuth)"];
    AuthOut["Authentik Outpost"];
    AuthS["Authentik Server"];
  end

  subgraph Docker
    Gluetun["Gluetun (VPN)"];
    QB["qBittorrent"];
    Prowlarr["Prowlarr"];
    Sonarr["Sonarr"];
    Radarr["Radarr"];
    Readarr["Readarr"];
    Lidarr["Lidarr"];
    Bazarr["Bazarr"];
    Jellyfin["Jellyfin"];
    Jellyseerr["Jellyseerr"];
    Flare["FlareSolverr*"];
    Strip["Striparr*"];
    Tdarr["Tdarr*"];
    Navi["Navidrome*"];
    Calib["Calibre-Web*"];
    Paper["Paperless-ngx*"];
    Immich["Immich*"];
  end

  %% --- Edges ---
  U --> Caddy;
  Caddy -->|"ForwardAuth"| AuthOut;
  AuthOut --> AuthS;

  Caddy -->|"TLS"| Jellyfin;
  Caddy -->|"TLS"| Jellyseerr;
  Caddy -->|"TLS"| Prowlarr;
  Caddy -->|"TLS"| Sonarr;
  Caddy -->|"TLS"| Radarr;
  Caddy -->|"TLS"| Readarr;
  Caddy -->|"TLS"| Lidarr;
  Caddy -->|"TLS"| Bazarr;
  Caddy -->|"TLS"| Navi;
  Caddy -->|"TLS"| Calib;
  Caddy -->|"TLS"| Paper;
  Caddy -->|"TLS"| Immich;

  QB --- Gluetun;

  %% --- Styling optional apps ---
  classDef opt fill:#eee,stroke:#999,stroke-dasharray: 5 5;
  class Flare,Strip,Tdarr,Navi,Calib,Paper,Immich opt;
