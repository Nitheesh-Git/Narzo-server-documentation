# Narzo Server Homelab

Evidence-led documentation for a personal server built from a non-rooted Realme Narzo 20 Pro (RMX2161). This is a living record of the machine as audited on 2026-09-25, not a generic homelab guide.

## What this is

Android 12 hosts Termux; Termux starts an Ubuntu 26.04 environment through `proot-distro`; Ubuntu runs a lightweight shell supervisor and the application services. The design is intentionally constrained by phone hardware, Android lifecycle behaviour, and proot limitations.

```text
Android 12 → Termux / Termux:Boot → proot Ubuntu → start-services.sh → services
```

## Start here

| Need | Document |
|---|---|
| Current verified state and evidence boundaries | [audit/current-state.md](audit/current-state.md) |
| Network paths and exposure | [architecture/network-and-exposure.md](architecture/network-and-exposure.md) |
| Boot, supervision, thermals | [architecture/boot-and-supervision.md](architecture/boot-and-supervision.md) |
| Service topology | [architecture/overview.md](architecture/overview.md) |
| Storage and data movement | [architecture/data-flow.md](architecture/data-flow.md) |
| Day-to-day operation and recovery | [operations/operations.md](operations/operations.md) |
| Incidents and known failures | [incidents/incident-register.md](incidents/incident-register.md) |
| Security posture and disclosure rules | [security/security-posture.md](security/security-posture.md) |
| Facts still missing | [audit/known-unknowns.md](audit/known-unknowns.md) |

## Evidence convention

Each claim is intentionally classified:

- **OBSERVED** — directly shown by a current command, configuration, process list, or log.
- **HISTORICAL** — supplied build history that may no longer describe the running system.
- **INFERRED** — a reasonable conclusion, not a direct observation.
- **DESIGN DECISION** — an intentional implementation choice.
- **KNOWN LIMITATION** — a verified constraint or weakness.
- **UNKNOWN / NOT YET VERIFIED** — no adequate evidence yet.

Secrets, credentials, tokens, keys, and the aria2 RPC secret are never stored here.

