# objectives: 3_operate_running_systems

###Boot systems into different targets manually
- `systemctl list-units --type=target --all` - view full list of targets 
- `systemctl get-default`                    - get default run-level 
- `systemctl multi-user.target`              - change to multi-user (RL 3) 
- `systemctl isolate graphical.target`       - switch to graphical  (RL 5) 
- `systemctl set-default graphical.target`   - set default to graphical 

###Identify CPU/memory intensive processes, adjust with renice & kill
####Renice
`renice [-n] priority [-g|-p|-u] identifiers (group, pid, user)`
- `renice -n 5 -p 1701`   - change priority to proc 
- `renice -n 5 -g wheel`  - change priority by group 
- `renice -n 5 -u johnny` - change priority by user 

####Start, stop and pause a process
- `kill (-SIGSTOP | -19)  pid` - suspend process to be continued later 
- `kill (-SIGKILL | -9)   pid` - kill a process, cannot be caught 
- `kill (-SIGTERM | -15)  pid` - kill a process, can be gracefully caught 
- `kill (-SIGCONT | -18)  pid` - continue a process that was suspended 

####Start, stop and check statufs of network services
- `systemctl status name.service` - check status 
  - same format for service: start, stop, restart, reload, enable
- `systemctl enable name.service` - enable service at boot 

