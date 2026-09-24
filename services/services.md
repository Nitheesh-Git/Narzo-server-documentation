# Service catalogue

## nginx

Listens on `:8080` over IPv4 and IPv6. It serves the dashboard and static AriaNg, and proxies FileBrowser, Navidrome, and AddSong. TLS is not configured on this listener. See [network exposure](../architecture/network-and-exposure.md).

## FileBrowser

Serves `/root/files` on `:8081`, configured with base URL `/files`. It is reachable from a tested LAN peer. It uses `/root/filebrowser.db`; the exact authentication policy is **NOT YET VERIFIED**.

## Navidrome

Runs on `:4533`, address `0.0.0.0`, base URL `/music`. It uses `/root/files/personal/nitheesh/downloads/music` and `/root/navidrome`. Audit evidence recorded version 0.62.0 on arm64, 639 tracks, 575 albums, 559 artists, 6 playlists, and 1 library. Filesystem watching is enabled; periodic scan and periodic backup are disabled. Artwork/path warnings exist but did not prevent streaming.

Unauthenticated Subsonic `ping` was rejected with a missing-user error. This establishes that the tested API call is not anonymously successful; it does not describe every Navidrome authorization path.

## aria2 and AriaNg

aria2 RPC is enabled at `:6800`, configured to listen on all local interfaces. Its RPC secret is configured and intentionally redacted. AriaNg is static nginx content at `/aria/`; it is not an aria2 reverse proxy.

## FTP / pyftpdlib

pyftpdlib serves `/root/files` on `100.70.24.74:2121`. A tailnet peer successfully connected during audit. The process has a severe, repeated crash/respawn history; see [incident register](../incidents/incident-register.md).

## Glances

Glances runs in web mode on `:61208` with `network`, `ip`, `connections`, `ports`, and `wifi` plugins disabled deliberately. Its HTTP endpoint was successful during audit and a LAN TCP connection succeeded.

## AddSong

AddSong is a Flask development-server application at `127.0.0.1:5001`, exposed through nginx at `/add-api/`. It provides download, stream, playlist, search, and delete endpoints. It is tailnet-reachable through nginx and has no observed authentication control. It is a known security limitation.

