# objectives: 8_selinux
####Firewall Management (firewall-cmd, firewalld, iptables)
- *Configure firewall settings using firewall-config, firewall-cmd, or iptables*
- `firewall-cmd --permanent <cmd>`                     - must add `--permanent` for changes to persist
- `firewall-cmd --list-all`                            - list configuration
- `firewall-cmd --list-services --zone=<zone|default>` - list added services to default or specific zone
- `firewall-cmd --get-services`                        - available services to enable 
- `firewall-cmd --add-service=<service>`               - enable service in default zone
- `firewall-cmd --add-port=<port/protocol>`            - add a port if not defined
- `firewall-cmd --reload`                              - reload changes
- `firewall-cmd --get-default-zone`                    - see context of default zone

####Diagnose and address routine SELinux policy violations
- `sestatus`                              - show status
- `setenforce Enforcing (or 1)`           - set SELinux to enforcing mode
- `vi /etc/selinux/config`                - config file to set perm state
- `chcon -t <type_t> <file>`              - test changing type label context
- `setsebool -P <boolean> 1|0`            - turn an SELinux boolean on or off
- `ausearch -m avc`                       - audit failures and review
- `grep AVC /var/log/audit/audit.log`     - secondary way to get errors
- `audit2allow -wa`                       - generate steps to make the AVC failure allowed
- `audit2allow -aM <name>.local`          - create a new module/policy package
- `restorecon <file>`                     - restore contexts: `/etc/selinux/targeted/contexts/files/`
- `semanage fcontext -l`                  - view all file contexts (grep if needbe)
- `yum install -y settroubleshoot-server` - install SELinux troubleshooting tools
- `sealert -a /var/log/audit/audit.log`   - displays SELinux policy violations

