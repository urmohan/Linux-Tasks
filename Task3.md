#### Task 3 - Some new developers have joined our team, so we need to create some users/groups and further need to setup some permissions and access rights for them.
![Permissions](/task3.png "Permissions")
---
##### 1. At first  switch to root user and create group devs & admins
`# groupadd devs`
`# groupadd admins`

##### 2. Create users ray & lisa  with sh login shell 
`# useradd -s /bin/sh ray`
`# useradd -s /bin/sh lisa`

##### 3. Make user  ray & lisa a member of  "devs" group
`# usermod -G devs ray`
`# usermod -G devs lisa`

##### 4. Set password for user called ray "D3vU3r321"
##### Set password for user called lisa "D3vUd3r123" 
`# passwd ray`
`# passwd lisa`

##### 5. Create users david & natasha with zsh login shell & set password
`# useradd -s /bin/zsh david`
`# useradd -s /bin/zsh natasha`
`# passwd david`
`# passwd natasha`

##### 6. Make user  david  & natasha a member of  "admins" group
`# usermod -G admins david`
`# usermod -G admins natasha`

##### 7. Make sure "/data" directory is owned by user "bob" and group "devs" and "user/group" owner has "full" permissions but "other" should not have any permissions.
`# ls -lsd /data`
`# chown bob:devs /data`
`# chmod 770 /data`
`# ls -lsd /data`

##### 8.  Give some additional permissions to "admins" group on "/data" directory so that any user who is the member the "admins" group has "full permissions" on this directory.
`# getfacl /data`
`# setfacl -m g:admins:rwx /data`
`# getfacl /data`

##### 9. Make sure all users under "devs" group can only run the "dnf" command with "sudo" and without entering any password.
`# cat /etc/sudoers |grep admins`
##### %admins ALL=(ALL) NOPASSWD:ALL
`# cat /etc/sudoers |grep dev`
##### %devs ALL=(ALL) NOPASSWD:/usr/bin/dnf

##### 10. Edit the disk quota for the group called "devs". Limit the amount of storage space it can use (not inodes). Set a "soft" limit of "100MB" and a "hard" limit of "500MB" on "/data" partition.
`# setquota -g devs 100M 500M 0 0 /dev/vdb1`
`#  quota -g -s  devs /data`

##### 11. Configure a "resource limit" for the "devs" group so that this group (members of the group) can not run more than "30 processes" in their session. This should be both a "hard limit" and a "soft limit", written in a single line.
`# cat /etc/security/limits.conf |grep dev`
##### @devs            -       nproc           30
