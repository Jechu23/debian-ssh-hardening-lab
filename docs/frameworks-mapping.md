# Framework mapping

## ASD Essential Eight

| Strategy | Evidence in this lab |
|---|---|
| Restrict administrative privileges | sudo only, root login disabled, `AllowUsers`, sudo logging, auditd rules on privilege use |
| Multi-factor authentication | SSH key (something you have) plus passphrase (something you know) |
| Regular backups | Config backups before every change (not a full backup strategy) |
| Patch operating systems | Debian security updates; vulnerability detection is a planned next step |

## ISO/IEC 27001:2022 Annex A (selected)

| Control | Evidence |
|---|---|
| A.5.15 Access control | Key-only SSH, `AllowUsers`, least privilege with sudo |
| A.5.17 Authentication information | PAM password history, ageing, hashing, lockout |
| A.8.5 Secure authentication | Key plus passphrase, `pam_faillock`, `MaxAuthTries` |
| A.8.9 Configuration management | CIS benchmark baseline and a decision register |
| A.8.15 Logging | auditd rules, sudo log, journald collection |
| A.8.16 Monitoring activities | Wazuh SIEM, FIM, SCA drift |
| A.8.20 Network security | `ufw` default deny, one forwarded port |

## MITRE ATT&CK

See the table in [threat-model.md](threat-model.md).
