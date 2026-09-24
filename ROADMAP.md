# Narzo Server Roadmap

This roadmap records investigation and documentation work. It is not a commitment to redesign the server around conventional infrastructure tools.

## Evidence to collect

- Inspect router/AP IPv4 forwarding, IPv6 inbound policy, and UPnP/NAT-PMP state.
- Verify whether Tailscale Funnel or another public-sharing feature is enabled.
- Establish the Android-side path that provides Tailscale connectivity inside proot.
- Determine why the Termux SSH process has no reachable TCP listener.
- Verify FileBrowser authentication policy without exposing user records.
- Inspect ACL semantics on persistent data paths.

## Reliability and recovery evidence

- Capture pyftpdlib exit evidence or failure conditions; the watchdog currently records only respawns.
- Plan controlled tests for Android reboot, Termux restart, Wi-Fi loss, Tailscale loss, and storage exhaustion.
- Determine whether `oom_score_adj` writes succeed in proot.
- Decide whether `~/thermal.log` needs explicit retention or size control.

## Security and publication hygiene

- Perform a secret scan before each public release.
- Review AddSong access control and shell-command construction as an observed risk.
- Confirm whether a broader backup model exists and document its recovery boundary.

## Documentation

- Keep audit dates attached to capacity, process, and service-health observations.
- Add evidence when an unknown becomes verified; do not silently update it into a fact.
- Add a runbook only after its recovery steps have been tested or explicitly labeled untested.

