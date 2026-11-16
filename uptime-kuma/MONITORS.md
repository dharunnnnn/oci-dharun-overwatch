# Uptime Kuma monitors (rebuild reference)

Notifications: Telegram and ntfy, both "Default enabled".
Secrets (bot token, ntfy topic, push tokens) are NOT here. Get them from the Uptime Kuma backup export, which is stored outside git.

## HTTP(s), checked from Overwatch
| Name | URL | Accepted codes | Notes |
|---|---|---|---|
| Open WebUI | https://chat.dharun.cloud | 200-299 | Cert expiry on |
| WebDav-Vault | https://vault.dharun.cloud | 200-299, 403 | 403 = Caddy blocking non-VPN. 200 = vault exposed. Cert expiry on |
| AdGuard DOH | https://dns.dharun.cloud/dns-query | as configured | 502 = AdGuard down behind Caddy. Cert expiry on |

## Ping / TCP, checked from Overwatch
| Name | Target | Mode |
|---|---|---|
| Dharun-matrix | ping 140.245.199.119 | normal (ICMP allowed from Overwatch IP in Matrix security list) |
| Matrix SSH | 140.245.199.119:22 | normal, 60s |
| Guard 3000 Open WebUI | 140.245.199.119:3000 | Upside Down, 300s |
| Guard 53 DNS | 140.245.199.119:53 | Upside Down, 300s |
| Guard 51821 wg admin | 140.245.199.119:51821 | Upside Down, 300s |
| Guard 8081 AdGuard UI | 140.245.199.119:8081 | Upside Down, 300s |

## Push, sent by Matrix (/usr/local/bin/kuma-push.sh, root cron every 5 min)
All: heartbeat 360s, retries 1, retry interval 60s.
| Name | Check on Matrix |
|---|---|
| Hermes | container running and 127.0.0.1:9119 answers |
| WireGuard | wg-easy running and wg0 exists |
| AdGuard DNS | dig example.com @127.0.0.1 -p 5353 |
| VPN DNS | dnsmasq active and dig example.com @10.8.0.1 |
| Open Terminal | container running |
| WebDAV service | systemd webdav active |
| Tailscale | tailscale status |
| Matrix resources | disk < 85% and free RAM > 10% |
| Chennai Pi | ping 10.8.0.4 (paused while Pi is off) |

Matrix reaches Kuma over the private network: security list ingress TCP 3001 from 10.0.0.225/32 only.
