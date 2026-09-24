# Narzo Server Diagrams

These diagrams are logical and evidence-led. They show observed relationships while omitting credentials, secrets, and unverified public-exposure claims.

## Runtime stack

```mermaid
flowchart TD
    Phone[Realme Narzo 20 Pro<br/>Android 12] --> Termux[Termux]
    Termux --> Boot[Termux:Boot script]
    Boot --> Thermal[Wake-lock and thermal loop]
    Boot --> PRoot[proot-distro Ubuntu 26.04]
    PRoot --> Supervisor[start-services.sh<br/>bootstrap and watchdog]
    Supervisor --> Nginx[nginx :8080]
    Supervisor --> FileBrowser[FileBrowser :8081]
    Supervisor --> Glances[Glances :61208]
    Supervisor --> FTP[pyftpdlib :2121]
    Supervisor --> Aria2[aria2 :6800]
    Supervisor --> Navidrome[Navidrome :4533]
    Supervisor --> AddSong[AddSong :5001]
```

## HTTP and data path

```mermaid
flowchart LR
    LAN[LAN client] --> Nginx[nginx :8080]
    Tailnet[Tailscale peer] --> Nginx
    Nginx --> Dashboard[Dashboard and static AriaNg]
    Nginx --> FB[FileBrowser :8081]
    Nginx --> ND[Navidrome :4533]
    Nginx --> AS[AddSong :5001]
    Aria2[aria2] --> Downloads[/root/files/Aria Downloads]
    AS --> Music[Personal music directory]
    FB --> Files[Shared data root]
    FTP[FTP on tailnet] --> Files
    ND --> Music
    ND --> NavDB[/root/navidrome/navidrome.db]
    AS --> NavDB
```

## Exposure boundary

```text
Confirmed LAN reachability
  nginx, FileBrowser, Glances, aria2, and Navidrome

Confirmed tailnet reachability
  nginx, FTP, and AddSong through nginx

Unknown
  Router forwarding, IPv6 inbound policy, UPnP/NAT-PMP,
  Tailscale Funnel, and Internet reachability
```

See [network and exposure](../architecture/network-and-exposure.md) for the evidence behind each boundary.
