Copy faillock, faillock_notify and pwhistory to /usr/share/pam-configs/,
append faillock.conf.append to /etc/security/faillock.conf, remove " nullok"
from /usr/share/pam-configs/unix, then run:
  sudo DEBIAN_FRONTEND=noninteractive pam-auth-update --package
Keep a root shell open in another session while testing.
