# objectives: 3_operate_running_systems

###Boot systems into different targets manually
- `systemctl list-units --type=target --all` - view full list of targets 
- `systemctl get-default`                    - get default run-level 
- `systemctl isolate multi-user.target`      - change to multi-user (RL 3) 
- `systemctl isolate graphical.target`       - switch to graphical  (RL 5) 
- `systemctl set-default graphical.target`   - set default to graphical 

###Identify CPU/memory intensive processes, adjust with renice & kill
- open up new tab
- `nice -n -10 yes` - start basic process with a nice level of -10
- `top` - view nice level of existing programs
`renice [-n] priority [-g|-p|-u] identifiers (group, pid, user)`

####Renice
- `renice -n 5 -p 1701`   - change priority to proc 
- `renice -n 5 -g wheel`  - change priority by group 
- `renice -n 5 -u johnny` - change priority by user 

####Start, stop and pause a process
- `kill (-SIGSTOP | -19)  pid` - suspend process to be continued later 
- `kill (-SIGCONT | -18)  pid` - continue a process that was suspended 
- `kill (-SIGTERM | -15)  pid` - kill a process, can be gracefully caught 
- `kill (-SIGKILL | -9)   pid` - kill a process, cannot be caught 

####Start, stop and check status of network services
- `systemctl status name.service` - check status 
  - same format for service: start, stop, restart, reload, enable
- `systemctl enable name.service` - enable service at boot 

