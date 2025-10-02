# TRYHACKME – Mr. Robot (CTF)

![MrRobot.png](MrRobot.png)

Use nmap to scan target ports

`nmap -T4 10.10.210.21`

![nmap-scan1.png](img/nmap-scan1.png)

Port:		Services:

443		https (Hypertext Transfer Protocol Secure)

80		http (Hypertext Transfer Protocol)

After that, we introduce the IP of the machine using the http protocol in our browser, we discover that there is a web page here 

![webpage2.png](img/webpage2.png)

Using gobuster tool to discover the directories that exists 

`gobuster dir -u [http://10.10.158.203](http://10.10.158.203) -w /usr/share/wordlist/dirbuster/directory-list-lowercase-2.3-medium.txt`

![gobuster3.png](img/gobuster3.png)

![gobuster-result4.png](img/gobuster-result4.png)

Navigate to robots.txt file 

![robots-file5.png](img/robots-file5.png)

Navigate to “key-1-of-3.txt”

![first-key6.png](img/first-key6.png)

Go to "fsocity.dic" and download it 

filter the content with this command

`sort -d fsocity.dic |  uniq -d > new_dic.txt`

![sort-dictionary7.png](img/sort-dictionary7.png)

Now with the Hydra tool we can bruteforce the user (wordpress username disclosure), but first we need to know how the information is being sent 

To do this we need to use the tool BurpSuite 

![burpsuite-interception8.png](img/burpsuite-interception8.png)

After enabling “Intercept” option, we can try login in the site to intercept the request 

![wordpress-login-try9.png](img/wordpress-login-try9.png)

![burpsuit-result10.png](img/burpsuit-result10.png)

`hydra -L /home/kali/Downloads/nfsocity.txt -p admin 10.10.160.233 http-post-form "/wp-login.php:log=^USER^&pwd=^PASS^:Invalid username" -t 30`

- L -> specify the wordlist/dictionary
- p -> specify the password to use

http-post-form -> to websites that use POST methos in their forms

- t 30 -> specifies the number of tasks (or threads) to use for the attack

![hydra-bruteforce11.png](img/hydra-bruteforce11.png)

Now we know that exists three users, but we need to discover their passwords too 

`hydra -l Elliot -P /home/kali/Downloads/fsocity.bk 10.10.104.220 http-post-form “/wp-login.php:log=^USER^&pwd=^PASS^:The password you entered for the username” -t 30`

![hydra-result12.png](img/hydra-result12.png)

We have the username and the password, we just need to log in to the page 

![wordpress-login13.png](img/wordpress-login13.png)

![dashboard14.png](img/dashboard14.png)

Now that we have access to the page with an account that have administrator privileges, we need to explore it to discover any possible vulnerability 

![edit-themes-page15.png](img/edit-themes-page15.png)

We find this page that we can upload .php files to edit the page themes, but we can upload malicious content to reverse shell it

To generate the payload we can access the [https://www.revshells.com/](https://www.revshells.com/) page to do it

![generate-reverse-shell-payload16.png](img/generate-reverse-shell-payload16.png)

Fill the IP field with the target machine and choose a port to connect through

We just need to copy and paste the content in the header.php file on the page

![upload-reverse-shell17.png](img/upload-reverse-shell17.png)

To execute the content in this file we need to reload the page, but first we need to put our pc to listen mod to wait for the connection 

`nc -lvnp 9001`

![listen-connection18.png](img/listen-connection18.png)

Now that the pc is listening, we can reload the page to execute the content 

![reload-webpage19.png](img/reload-webpage19.png)

As we can see, the code was executed successfully and now we have access to the victim 

![established-connection20.png](img/established-connection20.png)

Run a pseudo terminal with python to escaping from the restricted shell 

`python3 -c 'import pty; pty.spawn("/bin/bash")'` 

![python-pseudo-terminal21.png](img/python-pseudo-terminal21.png)

Going to the directory “/home/robot” we can see that exists two files, but the first one we don’t have permissions to read  

![mv-root-dir22.png](img/mv-root-dir22.png)

But we can see the content of the second file 

![cat-passwod-md5-file23.png](img/cat-passwod-md5-file23.png)

Now we pick the hash and go to [https://crackstation.net/](https://crackstation.net/) to see if this hash has already been cracked 

![crackstation24.png](img/crackstation24.png)

As we can see, this password hash has already been cracked

Now that we know the robot password, we can change to this user

![robot-login25.png](img/robot-login25.png)

And now we can see the content of the first file that have the second flag for this challenge  

![second-key26.png](img/second-key26.png)

Now the first and most common step that we need to do next is privilege escalation and to do it, the most common first command is to discover what binary files/applications we can execute with the privileges that we already have

And the command is...

`find / -perm +6000 2>/dev/null | grep ‘/bin/’`

- “find” - is a command that searches for files based on various criteria.
- “/” - is the root directory, which means the search will start from the root of the file system.
- “-perm +6000” - specifies that we're looking for files with permissions that include the execute bit set for the owner, group, or others (i.e., +x permissions). The +6000 is a symbolic representation of the permissions, where:

6 represents the execute bit set for the owner (4+2=6)

0 represents no permissions for the group

0 represents no permissions for others

In other words, we're searching for files that have execute permissions for anyone (owner, group, or others).

**2>/dev/null**

- “2>” - redirects the standard error stream (file descriptor 2) to a file or device.
- “/dev/null” - is a special file that discards any input sent to it, effectively suppressing error messages.

By redirecting the standard error stream to /dev/null, we're ignoring any error messages that might occur during the search process.

**| grep '/bin/'**

- “'/bin/'” - is the pattern we're searching for, which is the string /bin/. This is likely used to filter the results to only show files that are located in the /bin directory.

Now we need to analyze the output and try to find some unusual programs 

![unusual-programs27.png](img/unusual-programs27.png)

The binary file that stands out most is the nmap file 

Now we can search ways to do privilege escalation through nmap and to do this, we can go to **[https://gtfobins.github.io/gtfobins/nmap/](https://gtfobins.github.io/gtfobins/nmap/)**  

Doing these two commands here, we can verify that we are in root

**`nmap --interactive`**

**`!sh`**

![nmap-privilege-command28.png](img/nmap-privilege-command28.png)

Now we explore more to find the third flag

Going to the root directory we find the last flag for this challenge

![third-key29.png](img/third-key29.png)