#Red-Team #Writeup #CTF #HackTheBox


## Foothold

#### Port 80, Enumeration, Download file future = Local File inclusion (LFI)




## Shell as mysql

#### /etc/crontab, Found the source code of app.py, JWT token generation as admin with ChatGPT or do Crypto, close with ; and SQLi. Make a file and get reverse shell.




## Shell as www-data

#### Remove the file from /etc/crontab and remake it as reverse shell.




## Shell as qa

#### Enumeration of www-data files from /var/www and found the password of the user qa.




## Shell as dev

#### Sudo -l found a misconfiguration enumerated, googled and found the vulnerability.



## Shell as root 

#### Sudo -l get the help page on you and chmod 4777 the /bin/bash file with rsync.

#### `sudo /usr/bin/rsync -a --exclude=.hg /home/dev/app-production/* /opt/app/`  


#### Rsync copies all the files inside /app-production/ and paste them in the other directories.


#### So here’s what i did:  
#### Copy a script you want into the app-production folder, in my case i copied bash  
#### `cp /bin/bash /home/dev/app-production/bash`  


#### Set the file to be a SUID, meaning, whenever it is executed, will be executed with the privileges _**Of the owner of the file**_  
#### `chmod u+s /home/dev/app-production/bash`  


#### I used the `rsync` command with the `--chown` option to change the ownership of the `bash` file to `root`:  

#### `sudo /usr/bin/rsync -a --exclude=.hg /home/dev/app-production/* --chown=root:root /opt/app/`


#### This moves everything inside the app folder to the opt/app folder, _**and changes who owns it, now the owner is Root!**_  


#### And its done, you just have to execute the SUID and enjoy your root privileges:  
#### `/opt/app/bash -p`




## `DIRSEARCH`