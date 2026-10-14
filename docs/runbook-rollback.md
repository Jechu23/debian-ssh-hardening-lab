# Change procedure and rollback

## Before every phase

1. Open a second SSH session (root shell for PAM work: `sudo -s`) and keep it open.
2. Back up what you will touch, for example `cp -a /etc/ssh/sshd_config.d /root/hardening-backup/`.
3. Validate syntax before reloading (`sshd -t`, `visudo -c`).
4. Test from a **new** connection before closing the safety session.
5. Restart the Wazuh agent, wait 5 to 10 minutes, rescan, update the register.

## Rollback by phase

| Phase | Rollback |
|---|---|
| SSH (`20-cis.conf`) | `sudo rm /etc/ssh/sshd_config.d/20-cis.conf && sudo systemctl reload ssh` |
| Audit rules | `sudo rm /etc/audit/rules.d/50-cis.rules && sudo augenrules --load` |
| GRUB parameters | Restore `/etc/default/grub` from backup, `update-grub`, reboot |
| PAM profiles | Restore `/etc/pam.d` from backup, remove the three files in `/usr/share/pam-configs/` |
| Locked account | From a root shell: `faillock --user <name> --reset` |

## Lessons that shaped this runbook

- A removal list in `sshd_config` silently did nothing. Always confirm with `sshd -T`.
- Password ageing applied to an old password would have locked the account. Change the password first, then run `chage`.
- A pasted example output is not a command. Run commands one block at a time and read the output before the next step.
