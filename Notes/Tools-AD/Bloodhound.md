#Red-Team #Exploit #Tool #Generation #Payload #Vulnerability 



## What is Bloodhound?

#### Bloodhound is basically god in AD. It can search for you a lot of vulnerabilitys that you can abuse based on your inputs and targets.



## Usage:

#### 1. `Apt-get install bloodhound neo4j`
#### 2. `bloodhound && neo4j console &`
#### 3. Change the password and log you in the bloodhound
#### 4. Get the .json files based on your access-point
#### ----> `bloodhound-python -d <DOMAIN-NAME> -u <USER> {--hashes (LM):NTLM or -p <PASSWORD>} -ns <IP> -c All {--zip --use-ldap -dc dc01.DOMAIN -v}"` then upload the file in bloodhound interactive session.
#### ----> `SharpHound.exe -c All --zip -d certified.htb -f bloodhound_data` and get the .zip file for the interactive session.
#### 5. Enumerate the users, set targets and plan. Use google and Linux/Windows abuse section.