# Current state — audit snapshot

**Audit date:** 2026-09-25 (Asia/Kolkata). Some machine logs contain inconsistent timestamps; individual log dates must not be treated as a reliable chronology without corroboration.

## Platform

| Item | Status |
|---|---|
| Physical device | **OBSERVED/HISTORICAL:** Realme Narzo 20 Pro, RMX2161; MediaTek Helio G95; 6 GB RAM; non-rooted Android 12 |
| Runtime stack | **OBSERVED:** Termux → `proot-distro` → Ubuntu → `/root/start-services.sh` |
| Ubuntu root filesystem | **OBSERVED:** Android userdata ext4, 50 GB total / 20 GB used / 30 GB free at audit |
| Conventional init | **OBSERVED:** absent; the shell supervisor is the service-control mechanism |

## Functional state

| Component | Current evidence | Classification |
|---|---|---|
| nginx | HTTP 200 locally, through LAN address, and from a separate Tailscale peer | Functional |
| FileBrowser | Process present; LAN TCP connection to `:8081` succeeded | Functional at audit point |
| Glances | Process present; LAN TCP connection to `:61208` succeeded | Functional at audit point |
| aria2 | Process/config present; LAN TCP connection to `:6800` succeeded | Functional at audit point |
| Navidrome | Process present; LAN TCP connection to `:4533` succeeded; prior stream succeeded | Functional |
| AddSong | Loopback listener present; tailnet request through nginx returned HTTP 200 | Functional, security-limited |
| pyftpdlib / FTP | Current tailnet TCP and FTP session evidence, but extensive crash/respawn history | Functional but unstable |
| Termux SSH | `sshd` process/config observed; TCP `8022` refused locally, on LAN, and over tailnet | Currently non-functional for TCP access |

## Audit constraints

`ss`, `netstat`, `/proc/net/tcp*`, and firewall inspection inside proot were empty, restricted, or permission-denied. They must not be used as evidence that sockets or firewall rules do not exist. Listener reachability was instead tested from the local stack and independent LAN/tailnet clients.

