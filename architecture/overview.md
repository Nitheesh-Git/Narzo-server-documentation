# Architecture overview

## Current logical topology

```mermaid
flowchart TB
  A[Android 12<br/>Realme Narzo 20 Pro] --> T[Termux]
  T --> B[Termux:Boot script]
  B --> W[Wake-lock + thermal watcher]
  B --> P[proot-distro Ubuntu 26.04]
  P --> S[/root/start-services.sh<br/>bootstrap + watchdog]
  S --> N[nginx :8080]
  S --> F[FileBrowser :8081]
  S --> G[Glances :61208]
  S --> FTP[pyftpdlib :2121]
  S --> AR[aria2 :6800]
  S --> M[Navidrome :4533]
  S --> AS[AddSong :5001]
  N --> F
  N --> M
  N --> AS
```

The arrows represent observed startup and proxy relationships. They do not imply that every component is externally reachable.

## Service roles

| Service | Role | Startup command evidence |
|---|---|---|
| nginx | Single HTTP entry point and static dashboard/AriaNg server | `nginx` |
| FileBrowser | File interface for `/root/files` | `filebrowser -r /root/files -a 0.0.0.0 -p 8081 --baseurl /files` |
| Glances | Web monitoring | `glances -w` with network-related plugins disabled |
| pyftpdlib | FTP access to shared files | bound to the Tailscale interface |
| aria2 | Download engine with RPC | private configuration path omitted |
| Navidrome | Music streaming/catalogue | music directory and data directory supplied on command line |
| AddSong | Download/search/delete helper for the music library | Flask app bound to loopback |

## Architecture characteristics

- **DESIGN DECISION:** Single-binary/lightweight services are used instead of Docker, VMs, or systemd.
- **DESIGN DECISION:** nginx is the HTTP front door on port 8080; it proxies FileBrowser, Navidrome, and AddSong.
- **KNOWN LIMITATION:** This is a phone-based Android/Termux/proot environment, not conventional Ubuntu bare metal.
- **KNOWN LIMITATION:** The supervisor checks process presence, not application health.
