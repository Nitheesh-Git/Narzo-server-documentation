# Known unknowns and evidence gaps

These are deliberate documentation boundaries, not omissions.

| Topic | Status | What is known | Evidence needed to close it |
|---|---|---|---|
| Public Internet exposure | **UNKNOWN** | LAN and tailnet paths are proven; global IPv6 connectivity was observed | Router/AP IPv4 port-forward, IPv6 inbound-firewall, UPnP/NAT-PMP, and Tailscale Funnel review |
| Android-side Tailscale implementation | **NOT YET VERIFIED** | Ubuntu sees `tun0`; an independent tailnet peer reaches nginx and FTP | Android/Termux-side Tailscale app/process/config inspection |
| Android firewall behaviour | **UNKNOWN** | proot firewall inspection is denied | Android/network policy evidence or controlled reachability tests |
| Exact SSH failure cause | **UNKNOWN** | Valid config and live process coexist with TCP refusal everywhere tested | Non-disruptive Termux socket/log evidence, then a separately approved repair investigation |
| FileBrowser authentication policy | **NOT YET VERIFIED** | Database is `/root/filebrowser.db`; config CLI timed out | Safe read-only application/admin configuration evidence |
| AddSong authentication | **OBSERVED absent in inspected nginx and application code** | No nginx control, global Flask hook, or route auth was observed | Full deployment boundary review if another middleware exists |
| Backups outside Navidrome | **UNKNOWN** | Navidrome periodic backup is disabled | Backup destination/schedule evidence |
| Wi-Fi loss, Tailscale loss, Termux restart, Android reboot recovery | **UNTESTED** | Bootstrap and watchdog logic are documented | Deliberately planned recovery tests; do not infer success |
| ACL semantics | **NOT YET VERIFIED** | `+` markers appear on several persistent files/directories | `getfacl` output and intended access model |
