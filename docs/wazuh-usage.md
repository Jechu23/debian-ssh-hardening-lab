# How Wazuh is used, and where it can go next

## Used in this project

| Capability | Use here |
|---|---|
| **Security Configuration Assessment (SCA)** | Runs the CIS Debian 12 v1.1.0 policy on the agent and produces the pass/fail score that drives the whole project |
| **Log data collection** | The agent reads the systemd journal, so SSH and sudo events reach the manager |
| **File integrity monitoring (FIM / syscheck)** | Watches system files; replaces AIDE, so CIS checks 33284 and 33285 are accepted as covered |
| **Dashboard and CSV export** | Before and after evidence; the CSV is the input for the decision register |

## Not enabled yet (natural next steps)

| Capability | Why it matters | Idea for this lab |
|---|---|---|
| **Vulnerability detection** | Matches installed package versions against known CVEs | Enable it, triage the findings, add them to the register |
| **Active response** | Blocks an attacker IP automatically | Block repeated SSH failures at the manager, compare with fail2ban |
| **auditd log collection** | Sends the 51 audit rules' events to the SIEM | Add `/var/log/audit/audit.log` as a monitored log file and write rules for `sudo` and identity changes |
| **Custom rules and decoders** | Detection engineering | Write a rule that raises severity when a failed login is followed by a success from the same IP |
| **MITRE ATT&CK view** | Maps alerts to techniques | Show T1110 detections after a controlled brute-force test |
| **Docker monitoring** | Visibility into the containers on the host | Collect container events |

## Controlled test ideas (own systems only)

1. From an authorised test machine, generate failed SSH logins and confirm Wazuh raises the authentication-failure alert, fail2ban bans the source and the dashboard maps the event to T1110.
2. Edit a watched file such as `/etc/sudoers.d/10-hardening` and confirm FIM and auditd both record the change.
3. Try five wrong sudo passwords and confirm `faillock` locks the account for 15 minutes, then unlock with `faillock --user <name> --reset`.
