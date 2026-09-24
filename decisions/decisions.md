# Design decisions and trade-offs

## Phone + Termux + proot

**Decision:** use a non-rooted Android phone with Termux and Ubuntu proot as the server platform.

**Trade-off:** the platform is lightweight and accessible, but it lacks conventional init semantics, normal network/socket observability, normal firewall inspection, and bare-metal lifecycle guarantees.

## Shell watchdog instead of systemd

**Decision:** use `/root/start-services.sh` as bootstrap and supervisor.

**Trade-off:** simple deployment and automatic respawn; no dependency model, service health checks, exit-reason capture, or unit-level observability.

## nginx as a single HTTP entry point

**Decision:** put the dashboard, AriaNg, and selected web applications behind nginx at port 8080.

**Trade-off:** a simpler access path, but service availability and access-control assumptions must be evaluated at nginx plus each application.

## FileBrowser + media-player workflow instead of Jellyfin

**Historical decision:** Jellyfin was removed after .NET/runtime, CPU, and hardware-acceleration difficulties. The current media access pattern uses FileBrowser plus VLC/MX Player, with Navidrome for music.

## Discard nginx logs after incident

**Decision:** force nginx access/error logs to `/dev/null`.

**Trade-off:** protects scarce phone storage after a log-runaway incident, but loses normal nginx forensic evidence.

