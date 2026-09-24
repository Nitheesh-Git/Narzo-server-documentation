# Service catalogue

## nginx

Listens on `:8080` over IPv4 and IPv6. It serves the dashboard and static AriaNg, and proxies FileBrowser, Navidrome, and AddSong. TLS is not configured on this listener. See [network exposure](../architecture/network-and-exposure.md).

## FileBrowser

Serves the shared data root on its application port, configured with base URL `/files`. It is reachable from a tested LAN peer. The exact authentication policy is **NOT YET VERIFIED**. Private filesystem paths are omitted.

## Navidrome

Runs on port 4533, address `0.0.0.0`, base URL `/music`. It reads the configured personal music directory and stores its catalogue in a private application data directory. Audit evidence recorded version 0.62.0 on arm64 and a library of roughly 600 tracks. Filesystem watching is enabled; periodic scan and periodic backup are disabled. Artwork/path warnings exist but did not prevent streaming. Personal paths and detailed library counts are omitted from this public repository.

Unauthenticated Subsonic `ping` was rejected with a missing-user error. This establishes that the tested API call is not anonymously successful; it does not describe every Navidrome authorization path.

## aria2 and AriaNg

aria2 RPC is enabled and configured to listen on all local interfaces. Its RPC secret is configured and intentionally redacted. AriaNg is static nginx content at `/aria/`; it is not an aria2 reverse proxy. Exact private configuration paths and network addresses are omitted.

## FTP / pyftpdlib

pyftpdlib serves the shared data root through the Tailscale interface. A tailnet peer successfully connected during audit. The process has a severe, repeated crash/respawn history; see [incident register](../incidents/incident-register.md).

## Glances

Glances runs in web mode on `:61208` with `network`, `ip`, `connections`, `ports`, and `wifi` plugins disabled deliberately. Its HTTP endpoint was successful during audit and a LAN TCP connection succeeded.

## AddSong

AddSong is a Flask development-server application bound to loopback and exposed through nginx at `/add-api/`. It provides music management operations and is tailnet-reachable through nginx. No authentication control was observed. A security review identified unsafe handling of user input in a shell operation; implementation details are withheld from this public repository. This is a known security limitation.
