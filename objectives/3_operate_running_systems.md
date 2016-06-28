# objectives: 3_operate_running_systems

###Boot systems into different targets manually
- view full list of targets `systemctl list-units --type=target --all`
- get default run-level `systemctl get-default`
- change to multi-user (RL 3) `systemctl multi-user.target`
- switch to graphical  (RL 5) `systemctl isolate graphical.target`
- set default to graphical `systemctl set-default graphical.target`

###Identify CPU/memory intensive processes, adjust with renice & kill
####Renice
`renice [-n] priority [-g|-p|-u] identifiers (group, pid, user)`
- change priority to proc `renice -n 5 -p 1701`
- change priority by group `renice -n 5 -g wheel`
- change priority by user `renice -n 5 -u johnny`

####Start, stop and pause a process
- suspend process to be continued later `kill (-SIGSTOP | -19) pid`
- kill a process, cannot be caught `kill (-SIGKILL | -9) pid`
- kill a process, can be gracefully caught `kill (-SIGTERM | -15 ) pid`
- continue a process that was suspended `kill (-SIGCONT | -18) pid`

####Start, stop and check statufs of network services
- check status `systemctl status name.service`
  - same format for service: start, stop, restart, reload, enable
- enable service at boot `systemctl enable name.service`

