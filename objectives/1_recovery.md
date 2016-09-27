# objectives: 1_recovery

The following objective is to Recover / Reset the root password on a RHEL 7 system.

###Rescue Process (lost root password)
- start or reboot the system
- at the grub menu, press the [ESC] key to interrupt the boot proc
- press 'e' to edit the boot record
- scroll down to the 'linux16' entry and press the [end] key
- add `rd.break console=tty1`
- press CTRL-X to boot
- remount in r/w mode: `mount -o remount,rw /sysroot`
- switch to chroot jail: `chroot /sysroot`
- reset pw: `passwd root`
- create SELinux blank file under root: `touch /.autorelabel`
- type `exit` twice to reboot system 

