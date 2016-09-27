# objectives: 7_manage_users_and_groups
####Create, delete and modify local user accounts
- `useradd <username>`                - create a new user
- `useradd -G <group> <username>`     - add existing user to new group
- `usermod -aG <supgroup> <username>` - add user to supplementary group 
- `userdel -r <username>`             - remove the user completely from the system

####Change passwords and adjust password aging for local user accounts
- `chage -M <max_days> <username>` - set the maximum password age
- `chage -I <username>`            - set the account / user to *inactive*
- `chage -d 0 <username>`          - force user to change password at next login
- `date -d "+180 days"`            - determine the date 180+ days from today
- `vi /etc/login.defs`             - update password policy configuration file

####Create, delete and modify local groups and group memberships
- `groupadd -r <groupname>`           - create a new system group
- `groupmod -n <newname> <groupname>` - rename existing group
- `groupmod -g <groupid> <groupname>` - assign a GID to group
- `groupdel <groupname>`              - delete existing group

####Configure a system to use an existing auth service for user/group info
- using LDAP server: `server.example.com`

#####LDAP Client Configuration
- `yum install -y openldap-clients nss-pam-ldapd` - install LDAP tools
- `authconfig-tui`                                - Text user interface wizard to setup LDAP
  - Cache Information
  - Use LDAP
  - Use MD5
  - Use Shadow
  - Use LDAP Auth
  - Local Auth
  - In LDAP Settings:
    - Use TLS: `ldap://server.example.com`, `dc=example,dc=com`
- `/etc/openldap/cacerts` - location of LDAP server cert
- `yum install -y autofs nfs-utils`   - ensure NFS/AUTOFS tools are installed
- `vi /etc/auto.master.d/home.autofs` - create the master entry
  - `/home /etc/auto.home`            - add the primary mountpoint, point to config
- `vi /etc/auto.home` - create *home* automount
  - `*-rw,sync --fstype=nfs4 instructor.example.com:/home/guests/&` - add this line to `/etc/auto.demo`
- `systemctl start autofs.service`    - start up autofs
- `systemctl enable autofs.service`   - enable it
- `su - <ldapuser>`                   - test the configuration

#####Kerberos Configuration
- `yum install authconfig-gtk krb5-workstation` - install Kerberos tools
- `system-config-authentication`                - run the CLI tool to connect to IPA
  - ensure *Kerberos* is checked, and *DNS* is unchecked
    - verify with `getent` and `ssh` (TODO: figure more out about this)
- ![kerberos](./.images/kerberos.png)
- `yum install ipa-client`                      - ensure IPA tools are installed
- `ipa-client-install --domain=server.example.com --no-ntp --mkhomdir` - connect to test IPA
  - Enter AD credentials provided for adding Linux computers

