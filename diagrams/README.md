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
    AS --> Music[/root/files/personal/.../music]
    FB --> Files[/root/files]
    FTP[FTP on tailnet] --> Files
    ND --> Music
    ND --> NavDB[/root/navidrome/navidrome.db]
    AS --> NavDB
```

## Exposure boundary

```text
Confirmed LAN reachability
  10.248.33.58:8080, :8081, :61208, :6800, :4533

Confirmed tailnet reachability
  100.70.24.74:8080, :2121, nginx /add-api/ route

Unknown
  Router forwarding, IPv6 inbound policy, UPnP/NAT-PMP,
  Tailscale Funnel, and Internet reachability
```

See [network and exposure](../architecture/network-and-exposure.md) for the evidence behind each boundary.

