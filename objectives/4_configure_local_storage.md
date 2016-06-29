# objectives: 4_configure_local_storage

####List, create, delete partions on MBR and GPT disks
- `fdisk -l` - list all partitions
- `fdisk /dev/vda` - create a new primary partition 
- `[n]` - for new partition
- `[p]` - primary
- `(1-4)` - partition number
- `<size/sector>` - choose appropriate sizing
- `[t]` - filesystem type, `8e` for LVM, `82` for swap
- `[w]` - save the partition changes
- `partprobe` - force kernel to read updated partition table
- `mkfs.ext4 /dev/vda1` - format the disk for ext4
- `mkfs.xfs /dev/vda2` - format the disk for xfs
- `mkfs.vfat /dev/vda3` - format the disk for vfat
- `mkdir /mnt/{ext4,xfs,vfat}` - create mount points for each filesystem
- `blkid /dev/vda[n]` - retrieve UUID for appropriate partition
- update `/etc/fstab` with appopriate entry
```
#/etc/fstab
UUID='...' /dev/vda1 /mnt/ext4 ext4 defaults 0 2
UUID='...' /dev/vda2 /mnt/xfs xfs defaults 0 0
UUID='...' /dev/vda3 /mnt/vfat vfat defaults 0 0
```
- `mount -a` - mount all filesystems

####Create and remove physical volumes, assign physical volumes to volume groups, and create and delete logical volumes
- `lsblk -a`                             - display current block devices and configuration
- `pvcreate /dev/vdb1`                   - create a physical volume
- `vgcreate <vgname> -s 8m /dev/vdb1`    - create volume group, set extant size (4m default, set to 8m)
- `lvcreate -n <lvname> -L 25G <vgname>` - create logical volume, assign name and size
- `mkfs.xfs /dev/<vgname>/<lvname>`      - create filesystem (xfs) on logical volume
- `mkdir /mnt/lvm`                       - create mountpoint
- `blkid /dev/<vgname>/<lvname`          - display UUID
- `vi /etc/fstab`                        - update fstab

```
UUID="..." /mnt/lvm xfs defaults 0 0
# or
/dev/vg1/lv1 /mnt/lvm xfs defaults 0 0
```

####Add swap to a system non-destructively
- `lvcreate -L 1G -n <lv_swapname> <vgname>` - create new logical volume for swap space 
- `mkswap /dev/<vgname>/<lv_swapname>`       - create swap 
- `swapon /dev/<vgname>/<lv_swapname>`       - activate swap
- `swapon -s`                                - verify swap summary
- `vi /etc/fstab`                            - update fstab
- `/dev/vg/swap swap swap defaults 0 0`      - entry 

