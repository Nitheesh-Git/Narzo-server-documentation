# Network and exposure model

## Observed addresses

| Interface visible inside Ubuntu | Address | Evidence |
|---|---|---|
| `wlan0` | private Wi-Fi IPv4 and global IPv6 addresses | Ubuntu `ip -o addr`; exact values withheld |
| `tun0` | Tailscale IPv4 and IPv6 addresses | Ubuntu `ip -o addr`; exact values withheld |

The mechanism that makes Tailscale available inside this proot environment is **NOT YET VERIFIED**. The Android Tailscale app is a plausible historical explanation, not a confirmed implementation detail.

## Verified reachability

| Endpoint | Loopback | LAN peer | Tailnet peer | Interpretation |
|---|---:|---:|---:|---|
| nginx | HTTP 200 | TCP succeeded | TCP succeeded | LAN and tailnet reachable |
| FileBrowser | HTTP 200 historically | TCP succeeded | Not tested | LAN reachable |
| Glances | HTTP 200 historically | TCP succeeded | Not tested | LAN reachable |
| aria2 RPC | HTTP behaviour verified historically | TCP succeeded | Not tested | LAN reachable |
| Navidrome | HTTP redirect historically | TCP succeeded | Not tested | LAN reachable |
| FTP | N/A | refused (expected bind model) | TCP and FTP connection succeeded | tailnet reachable; unstable |
| AddSong via nginx | local backend open | inferred from nginx + LAN nginx | HTTP 200 | tailnet reachable |
| Termux SSH | refused | refused | refused | currently unavailable |

## nginx boundary

nginx listens on IPv4 and IPv6 wildcard addresses at `:8080` and has these routes:

| Path | Backend / content |
|---|---|
| `/` | static dashboard from `/var/www/html` |
| `/aria/` | static AriaNg content from `/var/www/html/aria` |
| `/files` | FileBrowser on its local application port |
| `/music` | Navidrome on its local application port |
| `/add-api/` | AddSong on its local application port |

## Exposure conclusion

**CONFIRMED:** nginx, FileBrowser, Glances, aria2, and Navidrome were reachable from a device on the Wi-Fi LAN. nginx, FTP, and AddSong through nginx were reachable from the tested tailnet peer. Exact network addresses are withheld from this public documentation.

**UNKNOWN:** Internet exposure. A wildcard listener or global IPv6 address does not prove public reachability. Router forwarding, IPv6 firewall policy, UPnP/NAT-PMP, and Tailscale Funnel remain uninspected.
