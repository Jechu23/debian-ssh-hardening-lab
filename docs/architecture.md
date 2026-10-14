# Architecture and data flows

![Architecture](img/architecture.png)

## Components

| Component | Role | Notes |
|---|---|---|
| Debian 12 server | OpenVPN gateway, hardening target | Also runs Docker and Tailscale for other uses |
| Wazuh all-in-one | SIEM: indexer, manager, dashboard | Ubuntu 24.04 VM on a desktop PC |
| Wazuh agent | Sends logs, SCA results and FIM events | Reads the systemd journal (Debian 12 has no `auth.log`) |
| Home router | DHCP reservations, single port forward | UDP 1194 to the VPN server only |
| Admin devices | Phone and laptop | SSH keys with passphrase, used inside the VPN tunnel |

## Flows

| Flow | Protocol | Purpose |
|---|---|---|
| Admin device to server | OpenVPN, UDP 1194 | Only service reachable from the internet |
| Admin device to server | SSH (key only), inside the tunnel or from the LAN | Administration |
| Agent to Wazuh manager | TCP 1514 (events), 1515 (enrolment) | Logs, SCA results, FIM events |
| Analyst to Wazuh dashboard | HTTPS, LAN only | Review alerts and compliance score |

## Trust boundaries

1. **Internet to home network.** One port forward, no management interface exposed.
2. **VPN tunnel to server.** Authenticated by OpenVPN, then by SSH key.
3. **Server to SIEM.** Agent-initiated, authenticated by the Wazuh enrolment key.

## Services published through Docker

Docker publishes container ports through its own iptables chains, which bypass `ufw` rules.
The exposure of those ports is controlled at the router (not forwarded) rather than in `ufw`.
