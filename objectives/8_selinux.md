# objectives: 8_selinux
####Firewalls
- *Configure firewall settings using firewall-config, firewall-cmd, or iptables*
- `firewall-cmd --permanent <cmd>` - must add `--permanent` for changes to persist
- `firewall-cmd --list-all` - list configuration
- `firewall-cmd --list-services --zone=<zone|default>` - list added services to default or specific zone
- `firewall-cmd --get-services` - available services to enable 
- `firewall-cmd --add-service=<service>` - enable service in default zone
- `firewall-cmd --reload` - reload changes
- `firewall-cmd --get-default-zone` - see context of default zone

####Diagnose and address routine SELinux policy violatios
- `sestatus` - show status
- `setenforce Enforcing (or 1)` - set SELinux to enforcing mode
- `vi /etc/selinux/config` - config file to set perm state
- `yum install -y settroubleshoot-server` - install SELinux troubleshooting tools
- `sealert -a /var/log/audit/audit.log` - displays SELinux policy violations
TODO: find more details
