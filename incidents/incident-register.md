# Incident and known-error register

## nginx log explosion

**Status:** historical incident; mitigation active.

nginx logs reportedly grew to about 33 GB over several days, including a period of about 8 GB in nine minutes. The documented trigger was repeated proxy errors producing excessive logging.

**Mitigation now observed:** nginx access logging is disabled, the error log points to `/dev/null`, and each supervisor boot forcibly recreates those `/dev/null` links. The supervisor also truncates large ordinary log files.

**Trade-off:** routine nginx logs are deliberately unavailable for future diagnosis.

## FTP instability

**Status:** active known reliability problem.

Supervisor logs show many pyftpdlib deaths and restarts across July and September 2026. Several bursts recur at the watchdog cadence, demonstrating that automatic respawn masks a continuing failure.

The current instance accepted tailnet FTP connections, so the correct state is: **available at audit point, but unstable over time**. The supervisor log does not capture the process exit reason, so root cause is **UNKNOWN**.

## Timestamp anomaly

**Status:** active audit finding.

Termux boot/process and supervisor records include dates in 2010 among 2026 records. The thermal log and a Navidrome HTTP date were coherent at audit time. Root cause is **UNKNOWN**. Historical order and file modification times must therefore be treated carefully.

## Termux SSH listener unavailable

**Status:** active known error.

Termux `sshd -t` accepted the configuration and an `sshd`/session process was present. Nevertheless, port 8022 was refused on IPv4 loopback, IPv6 loopback, tested LAN, and tested tailnet paths. The daemon is present but no reachable TCP listener was established during audit. Root cause is **UNKNOWN**.

