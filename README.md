<div align="center">

# 📱 Narzo Server Homelab

### Evidence-led documentation for a self-hosted Android server built with Termux, proot Ubuntu, Tailscale, nginx, and lightweight services.

![Android](https://img.shields.io/badge/Android-12-3DDC84?style=for-the-badge&logo=android&logoColor=white)
![Termux](https://img.shields.io/badge/Termux-Userspace-111111?style=for-the-badge&logo=gnometerminal&logoColor=white)
![Ubuntu](https://img.shields.io/badge/Ubuntu-26.04-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)
![Tailscale](https://img.shields.io/badge/Tailscale-Overlay_Network-242424?style=for-the-badge&logo=tailscale&logoColor=white)
![nginx](https://img.shields.io/badge/nginx-Reverse_Proxy-009639?style=for-the-badge&logo=nginx&logoColor=white)

**[Architecture](architecture/overview.md) · [Network](architecture/network-and-exposure.md) · [Services](services/services.md) · [Storage](architecture/data-flow.md) · [Operations](operations/operations.md) · [Incidents](incidents/incident-register.md) · [Security](security/security-posture.md) · [Audit](audit/current-state.md) · [Roadmap](ROADMAP.md)**

</div>

---

## 📖 About

This repository documents a real personal homelab built from a non-rooted Realme Narzo 20 Pro. It is an evidence-led record of the machine as it exists, including its architecture, operational constraints, incidents, limitations, and unresolved questions.

The goal is not to present a hypothetical or enterprise-style platform. The goal is to show how this specific phone-based server is built, what it depends on, what can fail, and what the evidence actually supports.

This is a living homelab. Claims are updated when new audit evidence contradicts or replaces an earlier historical note.

---

## 🖥️ Platform

| Component | Current state |
|---|---|
| Physical device | Realme Narzo 20 Pro |
| Host OS | Android 12, non-rooted |
| Hardware | MediaTek Helio G95, 6 GB RAM |
| Entry point | Termux and Termux:Boot |
| Linux runtime | Ubuntu 26.04 through `proot-distro` |
| Service control | `/root/start-services.sh` shell bootstrap and watchdog |
| Remote overlay | Tailscale, visible inside Ubuntu on `tun0` |
| Shared data root | `/root/files` on Android userdata |

---

## 🧩 Current services

| Service | Role | Port / path | Audit state |
|---|---|---|---|
| nginx | HTTP front door, dashboard, static AriaNg, reverse proxy | `:8080` | Functional on tested LAN and tailnet paths |
| FileBrowser | File interface for `/root/files` | `:8081`, `/files` | LAN reachable at audit point |
| Glances | Web monitoring | `:61208` | LAN reachable at audit point |
| aria2 | Download engine with authenticated RPC | RPC and `/aria/` UI | LAN reachable at audit point |
| Navidrome | Music catalogue and streaming | nginx music route | Functional |
| pyftpdlib | FTP access to shared files | Tailscale interface | Tailnet reachable, known unstable |
| AddSong | Music download, search, and delete helper | loopback, proxied by nginx | Functional, security-limited |
| Termux SSH | Remote shell service | configured SSH port | Process present, TCP listener unavailable at audit point |

Service details and evidence boundaries are in [services/services.md](services/services.md).

---

## 💾 Storage overview

| Location | Role | Audit size |
|---|---|---:|
| Shared downloads directory | aria2 downloads | about 2.7 GB |
| Personal media directory | music and other personal data | about 1.8 GB |
| Navidrome data directory | catalogue data | about 133 MB |
| aria2 state directory | configuration and session state | about 20 KB |

The proot-visible root filesystem is Android userdata ext4. At audit time it was 50 GB total, 20 GB used, and 30 GB available. See [storage and data flow](architecture/data-flow.md).

---

## 🧱 Architecture

```text
Realme Narzo 20 Pro
└── Android 12
    └── Termux
        ├── Termux:Boot
        ├── wake-lock and thermal monitoring
        ├── SSH daemon process
        └── proot-distro Ubuntu 26.04
            └── /root/start-services.sh
                ├── nginx
                ├── FileBrowser
                ├── Glances
                ├── pyftpdlib
                ├── aria2
                ├── Navidrome
                └── AddSong
```

The detailed view separates observed relationships from inference and unknowns. See [architecture overview](architecture/overview.md), [boot and supervision](architecture/boot-and-supervision.md), and [network exposure](architecture/network-and-exposure.md).

---

## 🛠️ Operations and troubleshooting

The platform uses a shell watchdog instead of systemd. It restarts missing processes every 30 seconds, but process presence is not treated as proof of service health.

Current documented operational findings include:

- nginx log growth was mitigated by intentionally discarding normal nginx logs.
- pyftpdlib repeatedly crashes and is respawned by the watchdog.
- Termux SSH has a valid configuration and active process, yet TCP 8022 was refused from loopback, LAN, and tailnet tests.
- The thermal loop alerts at 45°C and 48°C, but does not throttle or stop services.
- System and service logs contain an unresolved timestamp anomaly.

See [operations](operations/operations.md) and the [incident register](incidents/incident-register.md).

---

## 📚 Documentation map

| Page | What it covers |
|---|---|
| [Architecture](architecture/overview.md) | Physical stack, service topology, and design characteristics |
| [Network and exposure](architecture/network-and-exposure.md) | Observed addresses, confirmed reachability, and public-exposure boundary |
| [Boot and supervision](architecture/boot-and-supervision.md) | Boot chain, watchdog, logging safeguards, thermal and power behaviour |
| [Storage and data flow](architecture/data-flow.md) | Persistent paths, size observations, and service data movement |
| [Services](services/services.md) | Current roles, bindings, and per-service limitations |
| [Operations](operations/operations.md) | Startup, watchdog scope, and recovery boundaries |
| [Incidents](incidents/incident-register.md) | nginx log incident, FTP instability, timestamp anomaly, SSH issue |
| [Security posture](security/security-posture.md) | Exposure facts, known risks, and disclosure rules |
| [Design decisions](decisions/decisions.md) | Why this stack exists and its accepted trade-offs |
| [Current audit](audit/current-state.md) | Evidence-based service snapshot |
| [Known unknowns](audit/known-unknowns.md) | Questions deliberately not presented as facts |
| [Diagrams](diagrams/README.md) | Sanitized logical diagrams |
| [Sanitized configs](configs/README.md) | Publication rules for future examples |

---

## 🚧 Status and roadmap

**Status: active, audited, and still evolving.**

The current architecture is documented, but public Internet exposure, Android-side Tailscale implementation, FileBrowser authentication policy, complete backup model, and several recovery scenarios remain unverified. Those gaps are tracked rather than hidden. See [ROADMAP.md](ROADMAP.md) and [known unknowns](audit/known-unknowns.md).

---

## 🔐 Security and privacy

This repository must never contain passwords, API keys, RPC secrets, session secrets, tokens, private keys, cookies, or credential-bearing URLs. The aria2 RPC secret is documented only as configured and redacted.

The public repository describes the real architecture, but it must not become a source of credentials or unnecessary attack detail. See [security posture](security/security-posture.md) and [sanitized configuration rules](configs/README.md).

<div align="center">

### Building, auditing, breaking, recovering, and documenting a real phone-based homelab.

</div>
