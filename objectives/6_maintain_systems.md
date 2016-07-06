# objectives: 6_maintain_systems

####Configure networking, update connections / devices

- Devices and connections are two distinct things
  - devices are actual interfaces on the system
  - connections can be bound & be turned on/off 
  - only 1 connection can be up at a time
- `nmcli con show` or `nmcli dev status` - display network connections or devices
- `nmcli con del <name|UUID>`       - remove a connection / interface
- `nmcli con add con-name <name> ifname <interface> type ethernet ip4 1.1.1.1/24 gw4 1.1.1.1` 
  - create a connection (provide ip / gateway)
- `nmcli con reload` - reload configs into network manager
- `ip address show` or `ip a`       - check configuration
  - `nmcli con show <name>`         - all information about a connection
- `nmcli con down <name>`           - stop a connection
- `nmcli con up   <name>`           - start a connection
- To modify a connection:
  - `nmcli con mod <name> ipv4.address 1.1.1.1/24` - ip address
  - `nmcli con mod <name> ipv4.gateway 1.1.1.1`    - gateway
  - `nmcli con reload`                             - reload configs into network manager
  - `nmcli con up <name>`                          - ensure connection is up
  - `nmcli con delete <name>`                      - delete the connection when through

#####Hostname updates: statically or dynamically
- `hostnamectl <--static|--transient|--pretty>` - view all current hostnames (static, transient, pretty)
- `hostnamectl set-hostname <name>`             - set the hostname
  - use `--static`, `--transient`, `--pretty` for individual hostnames
- hostname resolution relies on `/etc/nsswitch.conf`
  - `hosts: files dns` - entry resloves first through files (static) then dns (dynamic)
    - *static* comes from `/etc/hosts` file
    - *dynamic* comes from `/etc/resolve.conf` file
- `nmcli con mod <interface> +ipv4.dns 1.1.1.1` - add DNS server
  - `+ipv4.dns` - adds a server
  - `ipv4.dns`  - replaces it
  - `-ipv4.dns` - removes a server
- `nmcli con up <interface>` - restart connection

####Schedule tasks using at and cron

| Minute | Hour | Day Of Month | Month | Day Of Week | CMD             |
|:------:|:----:|:------------:|:-----:|:-----------:|:---------------:|
| 0-59   | 0-23 | 1-31         | 1-12  | 1-7         | /root/script.sh |

- `crontab -u <username> -e` - edit users crontab
- wildcards `*` can be used to match every value
- cron jobs that neet to run routinely can be placed in `/etc/cron.{daily,weekly,monthly}`
  - these must be executable

#####Configure time services using NTP
- `timedatectl`                         - get current config
- `timedatectl list-timezones`          - list available time zones
- `timedatectl set-timezone <timezone>` - set the timezone eg: `America/Chicago`
- `yum install -y ntp`                  - install ntp
- `systemctl enable ntpd.service`       - enable ntpd at boot
- `systemctl start ntpd.service`        - start ntpd
  - config file is at `/etc/ntp.conf` 

#####Configure time services using Chrony
- `yum install -y chrony`           - install chrony
- `systemctl enable cronyd.service` - enable chronyd at boot
- `systemctl start cronyd.service`  - start chronyd
- `ntpdate <timeserver>`            - synchronize server
  - config file is located at `/etc/chrony.conf`
  
####Install and update packages from Redhat network, remote repo or local file system
- `yum-config-manager --add-repo=http://myrepo.com` - leverage this tool to generate repo file 
- `vi /etc/yum.repos.d/<remote>.repo`    - modify the repository to 
```
[base]
[myrepo.com]
name=added from http://myrepo.com
baseurl=http://myrepo.com
enabled=1
gpgcheck=0                        # <----- add this line!
```

