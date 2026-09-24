# Network and exposure model

## Observed addresses

| Interface visible inside Ubuntu | Address | Evidence |
|---|---|---|
| `wlan0` | `10.248.33.58/24` | Ubuntu `ip -o addr` |
| `tun0` | `100.70.24.74/32` | Ubuntu `ip -o addr` |
| `tun0` IPv6 | `fd7a:115c:a1e0::1601:18b9/128` | Ubuntu `ip -o addr` |
| `wlan0` IPv6 | global IPv6 addresses observed | Ubuntu `ip -o addr` |

The mechanism that makes Tailscale available inside this proot environment is **NOT YET VERIFIED**. The Android Tailscale app is a plausible historical explanation, not a confirmed implementation detail.

## Verified reachability

| Endpoint | Loopback | LAN peer | Tailnet peer | Interpretation |
|---|---:|---:|---:|---|
| nginx `:8080` | HTTP 200 | TCP succeeded | TCP succeeded | LAN and tailnet reachable |
| FileBrowser `:8081` | HTTP 200 historically | TCP succeeded | Not tested | LAN reachable |
| Glances `:61208` | HTTP 200 historically | TCP succeeded | Not tested | LAN reachable |
| aria2 `:6800` | HTTP behaviour verified historically | TCP succeeded | Not tested | LAN reachable |
| Navidrome `:4533` | HTTP redirect historically | TCP succeeded | Not tested | LAN reachable |
| FTP `:2121` | N/A | refused (expected bind model) | TCP and FTP connection succeeded | tailnet reachable; unstable |
| AddSong via nginx `/add-api/` | local backend open | inferred from nginx + LAN nginx | HTTP 200 | tailnet reachable |
| Termux SSH `:8022` | refused | refused | refused | currently unavailable |

## nginx boundary

nginx listens on IPv4 and IPv6 wildcard addresses at `:8080` and has these routes:

| Path | Backend / content |
|---|---|
| `/` | static dashboard from `/var/www/html` |
| `/aria/` | static AriaNg content from `/var/www/html/aria` |
| `/files` | FileBrowser at `127.0.0.1:8081` |
| `/music` | Navidrome at `127.0.0.1:4533` |
| `/add-api/` | AddSong at `127.0.0.1:5001` |

## Exposure conclusion

**CONFIRMED:** service reachability exists on the Wi-Fi LAN for ports 8080, 8081, 61208, 6800, and 4533; nginx, FTP, and AddSong-through-nginx are reachable from the tested tailnet peer.

**UNKNOWN:** Internet exposure. A wildcard listener or global IPv6 address does not prove public reachability. Router forwarding, IPv6 firewall policy, UPnP/NAT-PMP, and Tailscale Funnel remain uninspected.

