# objectives: 5_file_systems

###Create, mount and unmount filesystems (ext4, xfs, vfat)
- `lvcreate -L 100M -n <lv_name> /dev/<vgname>` - create new volume
- `mkfs.<ext4|xfs|vfat> /dev/<vgname>/<lvname>` - create ext4 filesystem
- `mount /dev/<vgname><lvname>`                 - manually mount the filesystem
- `vi /etc/fstab`                               - update fstab for each filesystem

```
# /etc/fstab
/dev/vg1/lv1 /mnt/ext4 ext4 defaults 1 2
/dev/vg1/lv1 /mnt/xfs   xfs defaults 0 0
/dev/vg1/lv1 /mnt/vfat vfat defaults 0 0
```

#####Mount and unmount file systems (NFS)
- `yum install -y nfs-utils`           - ensure required NFS services are installed
- `systemctl enable rpcbind.service`   - enable rpcbind
- `systemctl start  rpcbind.service`   - start rpcbind
- `systemctl enable nfs-client.target` - enable nfs-client target
- `systemctl start  nfs-client.target` - start nfs-client target
- `vi /etc/fstab`                      - add fstab entry for nfs4

```
nfsserver:/dir/share /mnt/nfs nfs4 defaults 0 0
```
- `mount -a` - automount fstab entries

#####Mount and unmount file systems (CIFS)
- `yum install -y cifs-utils samba-client` - ensure required CIFS services are installed
- `systemctl enable smb.service`           - enable samba
- `systemctl enable nmb.service`           - enable nmb
- `systemctl enable winbind.service`       - enable windbind
- `vi /etc/fstab`                          - add fstab entry for CIFS

```
//smbserver/share /mnt/cifs cifs rw,username=user,password=pw 0 0
```
- `mount -a` - automount fstab entries

#####Extend existing logical volumes
- `lvcreate -L 100M -n <lvname> /dev/<vgname>`        - create new volume if needed
- `mkfs.ext4 /dev/<vgname>/<lvname>`                  - make an ext4 filesystem
- `mount /dev/<vgname>/<lvname> /mnt/ext4`            - mount it somewhere logical
  - `lvextend -l +100%FREE -r /dev/<vgname>/<lvname>` - allocate ALL free space
  - `lvextend -L +50M -r /dev/<vgname>/<lvname>`      - allocate additional 50M or what have you

#####Create and configure set-GID directories for collaboration
- `groupadd -g 50000 <gname>`   - create a new group
- `mkdir /share`                - create a shared directory in root
- `chown nobody:<gname> /share` - change ownership of dir to *gname*
- `chmod 2770 /share`           - assign / set GID bit (SGID) to /share
  - (allows all members of group write privs, removes all for everyone else)
- `useradd -G <gname> <uname>`  - create new users and assign them this group

#####Create and manage ACLs
- `getfacl <file>`                      - view file ACLs
- `setfacl -Rm u:<username>:rwx <file>` - change file ACL to 7 for user
  - `setfacl g:<groupname>:rwx <file>`  - same but for group
- `setfacl -x u:<username> <file>`      - remove ACLs from file
- `setfacl -b u:<username> <file>`      - *completely* remove ACLs from file

