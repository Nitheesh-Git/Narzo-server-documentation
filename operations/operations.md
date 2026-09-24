# Operations and recovery

## Normal startup

Normal startup is Android boot followed by the Termux Boot script. Ubuntu services are started once by `/root/start-services.sh`, then monitored by its 30-second loop. There is no systemd unit set or conventional init inside Ubuntu.

## What the supervisor does

- Restarts a missing matching process.
- Writes a terse respawn line to `/root/supervisor.log`.
- Attempts OOM-score adjustment.
- Forces nginx logs to `/dev/null` and truncates oversized ordinary logs.

## What it does not do

- It does not record exit status or root cause.
- It does not prove a protocol/service is healthy.
- It does not provide dependency readiness checks.
- It does not test network reachability after recovery.
- It does not protect Termux-side `~/thermal.log` from growth.

## Current operational checks

Use the existing `health` command for a compact status view, but interpret it as a process-and-artifact check. For an exposure check, use an independent LAN or tailnet peer; proot socket enumeration is restricted and incomplete.

## Recovery status

Automatic process recovery is **OBSERVED**. Recovery from Android reboot, Wi-Fi loss, Tailscale loss, Termux restart, and storage exhaustion is **UNTESTED**. Do not claim those scenarios are resilient until deliberate tests are planned and recorded.

