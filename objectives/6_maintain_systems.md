# objectives: 6_maintain_systems

####Configure networking and hostname resolution statically or dynamically

#####Networking
- `nmcli con show` or `nmcli dev status` - display network config
- `nmcli con del <interface|UUID>` - remove a connection / interface
- `nmcli con add con-name <name> ifname <interface> type ethernet ip4 1.1.1.1/24 gw4 1.1.1.1` - create a connection (provide ip / gateway)
- `nmcli con reload` - reload configs into network manager
- `ip address show` or `ip a` - check configuration
  - `nmcli con show <interface>` - all information about a connection
- `nmcli con down <interface>` - stop a connection
- `nmcli con up   <interface>` - start a connection
- To modify a connection:
  - `nmcli con mod <interface> ipv4.address 1.1.1.1/24` - ip address
  - `nmcli con mod <interface> ipv4.gateway 1.1.1.1` - gateway
  - `nmcli con reload` - reload configs into network manager
  - `nmcli con up <interface>` - ensure connection is up

#####Hostname
- `hostnamectl <--static|--transient|--pretty>` - view all current hostnames (static, transient, pretty)
- `hostnamectl set-hostname <name>` - set the hostname
  - use `--static`, `--transient`, `--pretty` for individual hostnames
- hostname resolution relies on `/etc/nsswitch.conf`
  - `hosts: files dns` - entry resloves first through files (static) then dns (dynamic)
    - *static* comes from `/etc/hosts` file
    - *dynamic* comes from `/etc/resolve.conf` file
- `nmcli con mod <interface> +ipv4.dns 1.1.1.1` - add DNS server
  - `+ipv4.dns` adds a server
  - `ipv4.dns` replaces it
  - `-ipv4.dns` removes a server
- `nmcli con up <interface>` - restart connection

####Schedule taks using at and cron

#####Cron Fields

| Minute | Hour | Day Of Month | Month | Day Of Week | CMD             |
|:------:|:----:|:------------:|:-----:|:-----------:|:---------------:|
| 0-59   | 0-23 | 1-31         | 1-12  | 1-7         | /root/script.sh |

- `crontab -u <username> -e` - edit users crontab
- wildcards `*` can be used to match every value
- cron jobs that neet to run routinely can be placed in `/etc/cron.{daily,weekly,monthly}`
  - these must be executable

####Configure a system to use time services

#####Using NTP
- `timedatectl` - get current config
- `timedatectl list-timezones` - list available time zones
- `timedatectl set-timezone <timezone>` - set the timezone eg: `America/Chicago`
- `yum install -y ntp` - install ntp
- `systemctl enable ntpd.service` - enable ntpd at boot
- `systemctl start ntpd.service` - start ntpd
  - config file is at `/etc/ntp.conf` 

#####Using Chrony
- `yum install -y chrony` - install chrony
- `systemctl enable cronyd.service` - enable chronyd at boot
- `systemctl start cronyd.service` - start chronyd
- `ntpdate <timeserver>` - synchronize server
  - config file is located at `/etc/chrony.conf`
  
####Install and update packages from Redhat network, remote repo or local file system
- `vi /etc/yum.repos.d/<remote>.repo` - create new remote repo file, add following
- TODO: get more details for this and practice practice! 
```
[base]
name=RedHat-$relesever
baseurl=http://mirror.centos.org/centos/$relesever/os/$basearch/
gpgcheck=1
gpgkey=/etc/pki/rpm-gpg/..
enabled=1
```

