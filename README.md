
# THM-RootMe write-up
**CTF >_ RootMe Write-up**

https://tryhackme.com/room/rrootme


RootMe>_ is a CTF for beginners to carry out all the basic steps in an attack using a linux machine as a target. The aim of this exercise is to gain root privileges and grab the flag > **root.txt** 

This CTF allows the player to practice their skills using several tools and techniques that can be found on Kali or the THM AttackBox along with resources found online. 

**Task 1 - Deploy the machine**

  Without further a-do let's deploy that machine!
  
  
  
 ![deploy](https://user-images.githubusercontent.com/100538982/164940287-dc7117d9-8ad2-4a05-abb2-600f6494ab63.png)


**Task 2 - Reconnaissance**

The first stage in the **attack** is to gather as much information on the target through reconnaissance. Reading through the first question this points directly to conducting a port scan on the target machine to discover any open ports and live services running. 

  *Q. Scan the machine, how many ports are open?*
  
      ~#nmap -sV [IP Address]
        
   *A. 2*

![nmap](https://user-images.githubusercontent.com/100538982/164941609-a051a910-658c-4227-a1c6-65bdc1c4e6dc.png)

  *Q. What version of apache is running?*
  
  *A. 2.4.29*
  
  *Q. What service is running on port 22?*
  
  *A. SSH*
  
  Conducting further enumeration this will uncover the directories on this web server by running a Gobuster scan:
  
      ~#gobuster dir -u http://[IP Address] -w /usr/share/wordlists/dirb/common.txt -x php,phtml,php3 
   
  *Q. Find directories on the web server using the GoBuster tool.* 
  
  *A. NA*

  *Q. What is the hidden directory?*

  *A. /panel (Status: 301)*
  
![dirb output](https://user-images.githubusercontent.com/100538982/164942723-6b9254f8-012d-46e5-be9d-d48953d3e802.png)

**Tak 3 - Getting a shell**

  **Find a form to upload and get a reverse shell and find the flag.**
  
From the gobuster scan we can see this web-server uses php code therefore we need a **php-reverse-shell** to gain a foothold. Conducting a quick search on github our good friend the **pentestmonkey** has kindly provided this for us to download already:  https://github.com/pentestmonkey/php-reverse-shell

![php reverse shell](https://user-images.githubusercontent.com/100538982/164943135-21d0283f-0bc4-43ab-9100-bd6d2681073d.png)

Before uploading this payload to the web server we need to append the attack machine **IP address** into the code:

![shell code](https://user-images.githubusercontent.com/100538982/164943162-f410ade5-4300-4888-8309-2d79ebf6e541.png)

Having saved our reverse-shell code, we navigate to the web site IP address via the browser:  http://[IP Address/]

![web page](https://user-images.githubusercontent.com/100538982/164943641-e183a488-beb6-460d-bf66-afbce237bda6.png)

Appending the hidden sub-directory (/panel) to the IP Address we discover there is an upload function for documents to this web server. Using "browse" we can locate our shell file on the attack machine, and upload it to the target. If the upload doesn't work first time, then maybe try a different file extension for the reverse-shell file instead... 

![panel upload](https://user-images.githubusercontent.com/100538982/164943710-6a79e564-64fb-4f5e-bbad-9aca31f3eea7.png)

Prior to executing our payload to gain a reverse shell, we first need to set up a netcat listener in the terminal window:

    ~# nc -lnvp [port]

I used the default port in the reverse-shell code: "1234". Navigating to the uploads directory: http://[IP Address/uploads/] from the browser, click on the shell file to run the code. This should establish the reverse-shell with a prompt: **"$"**. To further test the shell run the following commands:

     ~#whoami
  
  
     ~#pwd

![get shell](https://user-images.githubusercontent.com/100538982/164944149-f0f1c4c3-6237-4057-9eb1-a216b3cb9ec6.png)

Finding the first flag can be achieved either by conducting a search using the "find" command or manually trawling through the directories.

  *Q. User.txt?*
  
    ~#find / -type f -name user.txt 2>/dev/null

![first flag](https://user-images.githubusercontent.com/100538982/164944425-e17e4510-0ae2-4a16-9611-922944ba2036.png)


**Task 4 - Privilege escalation**

  **Now that we have a shell, let's escalate our privileges to root.**
  
  To escalate to root privileges use the find command:
  
      ~#find / -user root -perm 4000 -exec ls -ldb {}, > /tmp/suidout.txt
      ~#cat /tmp/suidout.txt

  *Q. Search for files with SUID permission, which file is weird?*
  
   *A.*
  
 <img width="412" alt="suidbit" src="https://user-images.githubusercontent.com/100538982/164944910-c1b8da5a-fb82-4a8b-851d-96096c5849da.png">

 Having identified some usr/bin files with the SUID bit set, conduct a search at the gtfobins github page to find a method of escalating privileges from this directory.
 
 
  *Q. Find a form to escalate your privileges*
  
  *A.*
  
  ![gtfobins](https://user-images.githubusercontent.com/100538982/164945069-8e976c50-37c0-4640-b22b-63fa51efbe2e.png)
  
  
   ~#cd /usr/bin
   
  
  Run the command found on gtfobins to escalate privileges:
  
  ![escalation](https://user-images.githubusercontent.com/100538982/164945147-0a1f6b8e-8fbe-4590-892e-1c53c26b1052.png)

Either run a search for root.txt using "find" or manually trawl to find the flag...

  *Q. root.txt*
  
   *A.*
  
![rootflag](https://user-images.githubusercontent.com/100538982/164945154-7b5faee0-71cb-4156-adc4-07b30f50afdc.png)

Congratulations on finding the flag!!!!!!

I hope you enjoyed this CTF write-up. It's my first attempt at compliling a walk-through for "Capture the Flag!".

A big thankyou goes out to Reddyyz https://tryhackme.com/p/ReddyyZ for creating this room. 

https://twitter.com/CyberRo0kie

