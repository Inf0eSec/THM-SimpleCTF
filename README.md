
**THM-Simple CTF write-up**

https://tryhackme.com/room/easyctf >> Created by  MrSeth6797

Attack machine : THM Attackbox via the browser

A Simple CTF it is just that!! A CTF for noobs like myself who can hone their skills and intuitions required for ethical hacking and CTF challenges.

**Task 1 -  Simple CTF**

The fisrt step I took in this challenge was to conduct a port scan to identify open ports and running services. I set the -A flag for an noisy aggressive scan and the -oA flag to output all data collated to a file for future reference.
 
**How many services are running under port 1000?**

     ~#nmap -A [IP Address] -oA scan.txt
  
  ![SimpleCTF](https://user-images.githubusercontent.com/100538982/165145721-a2365df7-3c9d-4350-8dd9-2d8e6feeba6a.png)

A. 2
 
**What is running on the higher port?** 

A. SSH


**What's the CVE you're using against the application?**

To further enumerate details from the target I ran a Gobuster scan to identify any directories on the URL [IP Address}.

    ~#gobuster dir -u http://[IP Address] -w /usr/share/wordlist/dirb/common.txt -x php, html, js, php3
    
**Flags**    
-u = denotes URL

-w = path to wordlist

-x = file extensions

![gobuster](https://user-images.githubusercontent.com/100538982/165145962-e846fdb2-986b-4019-ab10-c148b6ae26c1.png)


Entering the URL into the browser and navigating to the directories listed with 200,301 status I uncovered the website version.  Using this version ID I ran a check on the Exploit db to find any vulnerabilities associated with it.


![cms made simple](https://user-images.githubusercontent.com/100538982/165146935-5df2c9ab-ccf7-4af5-bae9-e6021d8bf67e.png)


A. CVE-2019-9053


![explioit db](https://user-images.githubusercontent.com/100538982/165150828-f1c6ce5a-833b-452f-aafe-31ebb59289ae.png)


**To what kind of vulnerability is the application vulnerable?**

A. SQLi

From the Exploit db I was able to download the exploit for this vulnerability.  Due to the imcompatiility of the downloaded file with the current version of python running on the THM Attack Box, I had to grab another script to convert it to somethihing python would understand.

    ~#pip install 2to3
   
   
![pip](https://user-images.githubusercontent.com/100538982/165153141-048d92cd-740f-4a28-9bcd-876e3003087f.png)

    ~#python lib2to3 -s 46635.py
    
**What's the password?**

Running the exploit code against the target machine I was able to find a **salt**, **email address**, **username** and **password hash**. This required a little research to uncover the correct syntax used.

    ~#python 46635.py -u http://10.10.176.184/simple/ --crack -w /usr/share/wordlists/dirb/common.txt
    
![exploit scr](https://user-images.githubusercontent.com/100538982/165153787-d3415fb5-c616-4dd3-a4f4-bcb10c507b7f.png)
 
 Uncovering a username I used hydra to crack the password for a user against **SSH**.
 
    
     ~#hydra -l user -P /usr/share/wordlists/rockyou.txt ssh://10.10.176.184:
 
 **Flags set for Hydra**

-l denotes username
-P is pathway to wordlsit

![hydra pass](https://user-images.githubusercontent.com/100538982/165156068-30edc714-7dbd-415e-8d75-68a20a3e0ee5.png)


**Where can you login with the details obtained?**

a. SSH


![ssh](https://user-images.githubusercontent.com/100538982/165157137-057c696a-acf7-4fd9-a198-033a49c7ae83.png)


    ~# ssh -p username@10.10.176.184

**What's the user flag?**


![user flaag](https://user-images.githubusercontent.com/100538982/165157111-34ae0f0d-70fb-4bed-8edf-dfadec92f0e3.png)


**Is there any other user in the home directory? What's its name?**

sunbath


**What can you leverage to spawn a privileged shell?**

    ~#sudo -l
 
A. VIM

**What's the root flag?**

A. Now if I told you that where would the fun be in finding out!

I hope you enjoyed reading this quick write-up and maybe you learnt something new from it.  I know I did when putting this together. It certainly gets you thinking about the processes involved when **Capturing the Flag!!!!**

Written by CyberRo0kie




