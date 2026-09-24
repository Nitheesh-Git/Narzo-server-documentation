# Storage and data flow

## Persistence layout

| Path | Purpose | Observed state |
|---|---|---|
| `/root/files` | Shared server data root | 4.5 GB at audit |
| `/root/files/Aria Downloads` | aria2 downloads | 2.7 GB |
| `/root/files/personal` | personal data and music tree | 1.8 GB |
| `/root/navidrome/navidrome.db` | Navidrome catalogue/state | 4.6 MB |
| `/root/filebrowser.db` | FileBrowser state | 64 KB |
| `/root/.aria2/aria2.session` | aria2 resumable session state | zero bytes at audit |

## Flow

```mermaid
flowchart LR
  A[aria2] --> D[/root/files/Aria Downloads]
  GS[AddSong / getsong] --> M[/root/files/personal/.../music]
  FB[FileBrowser] --> F[/root/files]
  FTP[FTP] --> F
  N[Navidrome] --> M
  N --> DB[/root/navidrome/navidrome.db]
  AS[AddSong] --> DB
```

FileBrowser and FTP expose the shared file root. Navidrome reads the configured music directory and stores catalogue state in its own data directory. AddSong reads Navidrome's SQLite database to search or find a media file for deletion, and invokes `getsong.sh` for downloads.

## Backup and retention

**KNOWN LIMITATION:** Navidrome periodic backup is disabled. No broader backup destination or schedule has been verified. Do not interpret aria2 session persistence as backup.

Persistent files and directories show extended ACL markers in the audit output. Their exact ACL semantics are **NOT YET VERIFIED**.

