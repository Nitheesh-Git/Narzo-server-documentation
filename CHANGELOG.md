# Changelog

This file tracks meaningful public documentation changes and verified infrastructure changes. It does not attempt to reconstruct every historical edit from logs with unreliable timestamps.

## 2026-09-25

### Added

- Initial evidence-led Narzo server documentation.
- Architecture, network, boot, supervision, storage, service, operation, incident, security, decision, and audit pages.
- Sanitized logical diagrams and publication guidance.
- Current known-unknowns register and investigation roadmap.

### Verified

- nginx reachability through tested LAN and tailnet paths.
- FTP reachability through the tested tailnet path, alongside its ongoing instability.
- AddSong tailnet reachability through nginx.
- Actual Termux Boot script, shell supervisor, thermal loop, nginx configuration, aria2 configuration shape, and persistent storage paths.

### Documented limitations

- Public Internet exposure remains unknown.
- SSH TCP access is currently unavailable despite a valid configuration and active process.
- AddSong has no observed authentication and has an observed shell-command construction risk.
- nginx normal logs are intentionally discarded after a historical log-growth incident.

