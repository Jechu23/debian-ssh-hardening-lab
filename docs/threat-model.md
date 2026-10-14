# Threat model (lightweight)

**Asset:** the VPN server. If it falls, an attacker gets a foothold on the home network and can
read or redirect VPN traffic.

| Threat | Entry point | Controls in this lab | ATT&CK |
|---|---|---|---|
| Brute force or password guessing on SSH | SSH | Key-only login, `AllowUsers`, `MaxAuthTries 4`, fail2ban, `pam_faillock` | T1110 |
| Stolen SSH key | Admin device | Passphrase on every key, `LoginGraceTime 60`, `MaxStartups`, key revocation by removing it from `authorized_keys` | T1078 |
| Privilege escalation after a foothold | Local account | sudo with password, sudo log, 5 minute timeout, auditd rules on sudo and identity files, AppArmor | T1548 |
| Tampering with accounts or config | Local root | auditd watches on `passwd`, `shadow`, `sudoers`, PAM and AppArmor; Wazuh FIM | T1098, T1562 |
| Weak password reuse | Local account | `pam_pwhistory` (24), ageing (365 days), SHA512 hashing | T1110.004 |
| Exposed services | Router, Docker | Only UDP 1194 forwarded; `ufw` default deny for incoming and routed traffic | T1190 |
| Silent compromise | Anywhere | Wazuh agent, SCA drift detection, FIM | n/a |

## Residual risks (accepted)

- Docker-published ports are not governed by `ufw`. Mitigation: nothing but UDP 1194 is forwarded.
- SSH is open to the LAN and VPN in `ufw`. Mitigation: key-only, fail2ban, `AllowUsers`.
- Egress is not restricted. Mitigation: monitoring through Wazuh. Revisit with a proxy or egress allow-list.
- Single server, no high availability. A hardware failure takes the VPN down.
