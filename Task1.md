#### Task 1 - The database server called *centos-host* is running short on space! 
#### You have been asked to add an LVM volume for the Database team using some of the existing disks on this server.
![LVM](/images/task1.png "LVM Mount Volumes")
---
##### 1. At first  switch to root user and check any existing physical / logical volumes
`# pvs`
`# rpm -qa |grep lvm`

##### 2. Install the correct packages that will allow the use of "lvm" on the centos machine.
`# yum install -y lvm2`

##### 3. Create a Physical Volume for "/dev/vdb" & "/dev/vdc"
###### Create a volume group called "dba_storage" using the physical volumes  "/dev/vdb" and "/dev/vdc"
`# pvcreate /dev/vdb`
`# pvcreate /dev/vdc`
###### Create an "lvm" called "volume_1" from the volume group called "dba_storage". Make use of the  entire space available in the volume group.
`# vgcreate dba_storage /dev/vdb /dev/vdc`
`# lvcreate -n volume_1 -l 100%FREE dba_storage`
###### Verify volumes. 
`# pvs`
`# lvs`

##### 4. Format the lvm volume "volume_1" as an "XFS" filesystem
`# mkfs.xfs /dev/dba_storage/volume_1`
`# mkdir -p /mnt/dba_storage`
###### Mount the filesystem at the path "/mnt/dba_storage".
`# mount -t xfs /dev/dba_storage/volume_1 /mnt/dba_storage`
`# df -h /mnt/dba_storage/`

##### 5. Make sure that this mount point is persistent across reboots with the correct default options.
`# cat /etc/fstab`
###### /dev/mapper/dba_storage-volume_1 /mnt/dba_storage xfs defaults 0 0

##### 6. Create a group called "dba_users" and add the user called 'bob' to this group
`# groupadd dba_users`
`# usermod -G dba_users bob`
`# id bob`

##### 7. Ensure that the mountpoint "/mnt/dba_storage" has the group ownership set to the "dba_users" group
######   Ensure that the mount point "/mnt/dba_storage" has "read/write" and execute permissions for the owner and group and no permissions for anyone else
`# chown :dba_users /mnt/dba_storage`
`# chmod 770 /mnt/dba_storage`
`# ll -lsd /mnt/dba_storage/`
