#### Task 5 - Create a bash script called "container-stop.sh" under "/home/bob/" which should be able to stop the "myapp" container. It should also display a message "myapp container stopped!"
![PAM](/task5.png "PAM configuration")
---
##### 1. At first  switch to root user and Add a local DNS entry for the database hostname "mydb.kodekloud.com" so that it can resolve to "10.0.0.50" IP address
`# cat /etc/hosts`
###### 127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
###### ::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
###### 127.0.1.1 centos-host centos-host
###### 10.0.0.50    mydb.kodekloud.com

##### 2. Add an extra IP to "eth1" interface on this system: 10.0.0.50/24
`# ip address add 10.0.0.50/24 dev eth1`
`# ip a`

##### 3. Install "mariadb" database server on this server and "start/enable" its service.
`# yum install mariadb-server –y`
`# systemctl enable mariadb`
`# systemctl start mariadb`
`# systemctl status mariadb`

##### 4. Set a password for mysql root user to "S3cure#321"
`# mysqladmin -u root password 'S3cure#321'`

##### 5. The "root" account is currently locked on "centos-host", please unlock it.
###### Make user "root" a member of "wheel" group
`# usermod -U root`
`# usermod -G wheel root`

##### 6. Create and run a new Docker container based on the "nginx" image. The container should be named as "myapp" and the port "80" on the host should be mapped to the port "80" on the container
`# docker ps`
`# docker pull nginx`
`# docker run -d -p 80:80 --name myapp nginx`
`# docker ps`

##### 7. Create a bash script called "container-start.sh" under "/home/bob/" which should be able to "start" the "myapp" container. It should also display a message "myapp container started!"
`# cat /home/bob/container-start.sh`
###### #!/usr/bin/env bash
###### docker start myapp
###### echo "myapp container started!"
`# chmod +x /home/bob/container-start.sh`
`# cat /home/bob/container-stop.sh`
###### #!/usr/bin/env bash
###### docker stop myapp
###### echo "myapp container stopped!"
`# chmod +x /home/bob/container-stop.sh`

##### 8. Add a cron job for the "root" user which should run "container-stop.sh" script at "12am" everyday.
###### Add a cron job for the "root" user which should run "container-start.sh" script at "8am" everyday.
`# crontab -l`
`# crontab -e`
`# crontab -l`
###### 0 0 * * * /home/bob/container-stop.sh
###### 0 8 * * * /home/bob/container-start.sh

##### 9. Edit the PAM configuration file for the "su" utility so that this utility only accepts the requests from the users that are part of the "wheel" group and the requests from the users should be accepted immediately, without asking for any password.
`# cat /etc/pam.d/su`
###### #%PAM-1.0
###### auth            required        pam_env.so
###### auth            sufficient      pam_rootok.so
###### # Uncomment the following line to implicitly trust users in the "wheel" group.
###### auth            sufficient      pam_wheel.so trust use_uid
###### # Uncomment the following line to require a user to be in the "wheel" group.
###### auth            required        pam_wheel.so use_uid
###### auth            substack        system-auth
###### auth            include         postlogin
###### account         sufficient      pam_succeed_if.so uid = 0 use_uid quiet
###### account         [success=1 default=ignore] \
######                                 pam_succeed_if.so user = vagrant use_uid quiet
###### account         required        pam_succeed_if.so user notin root:vagrant
###### account         include         system-auth
###### password        include         system-auth
###### session         include         system-auth
###### session         include         postlogin
###### session         optional        pam_xauth.so
