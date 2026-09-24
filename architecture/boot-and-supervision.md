# Boot, supervision, power, and thermal model

## Boot chain

```text
Android boot
  → Termux:Boot (inferred execution mechanism)
    → ~/.termux/boot/start-server.sh
      → writes boot-last-ran.txt
      → termux-wake-lock
      → sshd
      → thermal-watch.sh
      → proot-distro Ubuntu → /root/start-services.sh
```

The boot script backgrounds Ubuntu. It does not wait for the services to become ready.

## Supervisor behaviour

`/root/start-services.sh` starts nginx, FileBrowser, Glances, pyftpdlib, aria2, Navidrome, and AddSong in that order. It then checks matching processes every 30 seconds and restarts missing ones.

- nginx daemonizes; its master is reparented to PID 1 but remains monitored with `pgrep`.
- Other audited service processes were direct children of the supervisor.
- Low-memory-killer protection is attempted through `oom_score_adj`; success is **NOT YET VERIFIED** because errors are suppressed in proot.
- Process restart is not a health check. A running process may still fail its protocol or application function.

## Logging protection

At every supervisor boot, nginx access and error logs are removed and linked to `/dev/null`. The loop also truncates ordinary files above 500 MB under `/var/log` and root-level `*.log` files.

This is an intentional response to the nginx log-explosion incident. It also means normal nginx forensic logs are intentionally unavailable.

## Thermal / power loop

`~/thermal-watch.sh` is an infinite loop. It samples Android battery status every 30 seconds, renews the wake-lock, writes a dashboard widget, and appends to `~/thermal.log`.

| State | Threshold | Action |
|---|---:|---|
| OK | below 45°C | removes thermal notification |
| WARN | 45°C or higher | Android high-priority warning |
| CRIT | 48°C or higher | Android maximum-priority notification |

**KNOWN LIMITATION:** it does not throttle workloads, stop services, or power down at critical temperature. `~/thermal.log` is outside the Ubuntu log-size guard.

