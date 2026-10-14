# Scanner defects found in the Wazuh CIS Debian 12 SCA policy (v1.1.0)

Each check below still showed as failed after the control was correctly in place. I verified the
real state on the host, then recorded the check as an accepted risk.

| Check | What the scan expects | What is actually true |
|---|---|---|
| 33188 pam_pwquality enabled | `retry=(d+)` (literal `d`) | pam_pwquality installed and enabled |
| 33202, 33203 pwhistory | `remember` with no value | `remember=24 enforce_for_root use_authtok` (CIS-compliant) |
| 33216 only GID 0 group is root | `root:x:0:0` in `/etc/group` (passwd format) | root is the only GID 0 group |
| 33217 root access controlled | text "Password is status: P" | `passwd -S` prints a different format; root SSH login is off |
| 33248 audit_backlog_limit | rule negates every grub line | `audit_backlog_limit=8192` active |
| 33300 shadowed passwords | inverted rule | no account lacks `x` in `/etc/passwd` |
| 33254 actions as another user | `-C euid!=uid` | `auditctl` always prints `-C uid!=euid` |
| 33257 network environment | watch on `/etc/netplan` | Debian has no netplan; `/etc/network` is watched |
| 33266 MAC policy | trailing slash in path | `auditctl` strips it when listing |
| 33267 to 33270 chcon, setfacl, chacl, usermod | file rule with `-F auid!=unset` missing its `r:` prefix | audit rule is loaded and listed |
| 33205 pam_unix has no remember | fails on any `remember` text in `common-*` | conflicts with 33201, which needs `remember=24` in `common-password` |
| 33250 to 33252 audit log actions | `keep_logs`, `halt` or `single` when the disk fills | would take a headless VPN gateway offline; accepted with rationale |

## Method

1. Read the exact rule from the exported CSV (`Rules` and `condition` columns).
2. Reproduce what the rule looks at on the host (`auditctl -l`, `sshd -T`, `passwd -S`, file contents).
3. If a real gap exists, fix it. If the rule cannot match a correct configuration, record why.
