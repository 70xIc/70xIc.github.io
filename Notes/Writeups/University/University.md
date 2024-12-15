#CTF #Red-Team #HackTheBox #Writeup 


## Foothold

#### Enumerate the website at port 80, pdf generation? -- why don't you check for some CVEs on it with decoding the Base64 encoding. Find the CVE and exploit it right with .ps1 (curl)




## User.txt / Root.txt

#### Since ur in the Machine enumerate for a clear-text password and get the user password to login with evil-winrm Download to your localmachine [[RunasCs.exe]] and then exploit the vulnerability with the unintended way, get at the end the User.txt and the Root.txt.