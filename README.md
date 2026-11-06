# oci-lab: dharun-overwatch

Monitoring box in the OCI lab. It watches dharun-matrix from outside.

## Server
- Shape: VM.Standard.E2.1.Micro (x86_64, 1 GB RAM, no swap), Always Free
- OS: Ubuntu 24.04 LTS
- Boot disk: 45 GB, about 4.5 GB used

## Services
| Service | How it runs | Port | Data |
| --- | --- | --- | --- |
| Uptime Kuma 1.x | Docker Compose in ~/uptime-kuma | 3001 on all interfaces | Docker volume uptime-kuma_uptime-kuma-data (SQLite) |

No extra host services. Firewall rules load from /etc/iptables through iptables.service. No cron jobs.

## Not in this repo (back up separately)
- The Uptime Kuma volume: monitors, history and notification settings, including any Telegram bot token.
- Uptime Kuma's JSON export, which also holds notification credentials.

## Rebuild
1. Create an E2.1.Micro instance with Ubuntu 24.04.
2. Install Docker: `curl -fsSL https://get.docker.com | sudo sh`
3. Restore the firewall: copy host/rules.v4 and rules.v6 to /etc/iptables/, check that port 22 is allowed, then `sudo systemctl restart iptables ip6tables`.
4. Restore the Uptime Kuma volume from backup, then `cd uptime-kuma && sudo docker compose up -d`.
5. Open http://<ip>:3001 and confirm the monitors.

## Known issues
- Port 3001 listens on 0.0.0.0. Check whether the OCI security list exposes it.
- No swap on a 1 GB box. Add a 2 GB swapfile.
- A reboot is pending and 45 package updates are waiting.
- Telegram alerts were never finished. ntfy is the planned replacement.
