# objectives: 8_selinux
####Diagnose and address routine SELinux policy violatios
- `sestatus` - show status
- `setenforce Enforcing (or 1)` - set SELinux to enforcing mode
- `vi /etc/selinux/config` - config file to set perm state
- `yum install -y settroubleshoot-server` - install SELinux troubleshooting tools
- `sealert -a /var/log/audit/audit.log` - displays SELinux policy violations
TODO: find more details
