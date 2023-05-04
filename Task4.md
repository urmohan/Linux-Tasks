#### Task 4 - Some of our apps generate some raw data and store the same in /home/bob/preserved directory. We want to clean and manipulate some data and then want to create an archive of that data.
![Archive ](/task4.png "Archive Data ")
---
##### 1. At first  switch to root user and list the /opt folders
`# ls -l /opt/`

##### 2. Make directories hidden & files under /opt/appdata
`# mkdir -p /opt/appdata/hidden`
`# mkdir -p /opt/appdata/files`
`# ls -l /opt/appdata/`
`# ls -l /opt/appdata/`

##### 3. Find the"non-hidden" files in "/home/bob/preserved" directory and copy them in "/opt/appdata/files/" directory
##### Find the "hidden" files in "/home/bob/preserved" directory and copy them in "/opt/appdata/hidden/" directory
`# find /home/bob/preserved -type f -not -name ".*" -exec cp "{}" /opt/appdata/files/ \;`
`# find /home/bob/preserved -type f -name ".*" -exec cp "{}" /opt/appdata/hidden/ \;`

##### 4. Find and delete the files in "/opt/appdata" directory that contain a word ending with the letter "t" (case sensitive) 
`# rm -f $(find /opt/appdata/ -type f -exec grep -l 't\>' "{}"  \; )`

##### 5. Change all the occurrences of the word "yes" to "no" in all files present under "/opt/appdata/" directory.
###### Change all the occurrences of the word "raw" to "processed" in all files present under "/opt/appdata/" directory. It must be a "case-insensitive" replacement, means all words must be replaced like "raw , Raw , RAW" etc
`# find /opt/appdata -type f -name "*" -exec sed -i 's/\byes\b/no/g' "{}" \;`
`# find /opt/appdata -type f -name "*" -exec sed -i 's/\braw\b/processed/ig' "{}" \;`

##### 6. Create a "tar.gz" archive of "/opt/appdata" directory and save the archive to this file: "/opt/appdata.tar.gz"
`# cd /opt`
`# tar -zcf appdata.tar.gz appdata`
`# ls`

##### 7. Add the "sticky bit" special permission on "/opt/appdata" directory (keep the other permissions as it is).

###### Make "bob" the "user" and the "group" owner of "/opt/appdata.tar.gz" file.

###### The "user/group" owner should have "read only" permissions on "/opt/appdata.tar.gz" file and "others" should not have any permissions.
`# chmod +t /opt/appdata`
`# ls -lsd /opt/appdata`
`# chown bob:bob /opt/appdata.tar.gz`
`# ls -lsd /opt/appdata`
`# chmod 440 /opt/appdata.tar.gz`

##### 8. Create a "softlink" called "/home/bob/appdata.tar.gz" of "/opt/appdata.tar.gz" file.
`# ln -s /opt/appdata.tar.gz /home/bob/appdata.tar.gz`

##### 9. Create a script called "/home/bob/filter.sh".
`# cat /home/bob/filter.sh`
###### #!/bin/bash
###### tar -xzOf /opt/appdata.tar.gz | grep processed > /home/bob/filtered.txt
`# chmod +x /home/bob/filter.sh`
`# ls -l /home/bob/`

##### 10. Execute the scrtip and filter the My processed data in filter.txt file
`# /home/bob/filter.sh`
`# ls -l /home/bob/`
`# cat /home/bob/filtered.txt`
###### My processed data
###### My processed data
###### My processed data
###### My processed data

