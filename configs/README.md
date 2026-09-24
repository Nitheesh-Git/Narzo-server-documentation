# Sanitized configuration examples

This directory is reserved for configuration examples that are safe to publish. It does not contain live service configuration.

## Never publish

- Passwords, API keys, tokens, cookies, or session secrets
- aria2 RPC secret values
- SSH or Tailscale private keys
- Wi-Fi credentials
- Private certificates
- Credential-bearing URLs
- Personal file names or personal media metadata
- Router administration credentials or sensitive firewall rules

## Before adding an example

- [ ] Replace real credentials with `<redacted>`.
- [ ] Replace real private endpoints with `<host>` or `<server-ip>` unless the exact value is required and approved for publication.
- [ ] Remove personal paths when a logical placeholder explains the design equally well.
- [ ] Check comments, shell history, and disabled configuration lines.
- [ ] Review the staged Git diff.
- [ ] Run a secret scan before pushing.

An example should explain the configuration pattern without exposing the live server.

