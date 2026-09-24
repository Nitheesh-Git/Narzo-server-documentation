# Security posture and publication rules

## Current exposure facts

- nginx is reachable on tested LAN and tailnet paths.
- FTP is reachable from the tested tailnet peer and is intentionally bound to the Tailscale address.
- AddSong is tailnet-reachable through nginx.
- Public Internet reachability is **UNKNOWN**.
- nginx has no observed TLS listener, HTTP auth, or IP allow/deny control.

## Material limitations

| Finding | Status |
|---|---|
| aria2 RPC listens on all local interfaces | **OBSERVED**; secret is configured |
| AddSong exposes unauthenticated download and delete actions | **OBSERVED** |
| AddSong has unsafe handling of user input in a shell operation | **OBSERVED command-execution risk; implementation detail withheld** |
| AddSong returns absolute paths after deletion | **OBSERVED information disclosure** |
| nginx allows unlimited request-body size | **OBSERVED** (`client_max_body_size 0`) |
| nginx advertises build information | **OBSERVED** (`server_tokens build`) |
| normal nginx logs are discarded | **KNOWN LIMITATION** |

This file describes present risks; it does not authorize or imply a redesign.

## Disclosure rules

Never commit passwords, API keys, private keys, tokens, cookies, authentication databases, RPC secrets, or private endpoints. The aria2 configuration may state only: **“RPC secret configured; value redacted.”** Run a secret scan before publishing any future revision.
