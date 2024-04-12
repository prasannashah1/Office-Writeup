# Office-Writeup

<img src=https://github.com/prasannashah1/Office-Writeup/assets/28432698/1fff009d-74c8-4d27-965b-cd4db5c8eabd height=400 width=700><br>

Office is a Windows machine, here you will find common Joomla CVE, hash cracking, reverse shell and user access.

First I started with adding IP to the hosts file (home.htb)

<h1>Nmap Scan</h1>

<img src=https://github.com/prasannashah1/Office-Writeup/assets/28432698/5a5d5c2d-1193-4a00-a0e5-2af5d431df61 height=500 width=1000> <br>

This is the home page <br>

<img src=https://github.com/prasannashah1/Office-Writeup/assets/28432698/c99f04a0-26fe-4520-8fdd-2d15a61b5ae0 height=500 width=1000><br>

<h1>Directory Enumeration</h1>

Here I am using Gobuster for directory enumeration 

<img src=https://github.com/prasannashah1/Office-Writeup/assets/28432698/b21c10bf-c2cf-4b5f-988d-ea768b2f4feb height=500 width=1000><br>

The administraor directory took me to the Joomla login page <br>

<img src=https://github.com/prasannashah1/Office-Writeup/assets/28432698/829db2c3-9e2e-4a4c-9ec4-a41544281919 height=500 width=1000><br>

After some research I found that the version of Joomla can be found with this url (home.htb/administrator/manifests/files/joomla.xml)<br>

<img src=https://github.com/prasannashah1/Office-Writeup/assets/28432698/84d371aa-121f-4694-9c80-372a87254abc height=500 width=1000><br>

So, this version of Joomla is vulnerable. This vulnerability causes to an unauthorized access to web services. (CVE-2023-23752) <br>

We can exploit this vulnerability to obtain username and password with the command (curl -v http://home.htb/api/index.php/v1/config/application?public=true) <br>

<img src=https://github.com/prasannashah1/Office-Writeup/assets/28432698/b5210da0-9786-4cf8-acd4-bad3cb031333 height=500 width=1000><br>

But I was not able to login to Joomla control panel with this username and password. So, now I need to try another way to login to Joomla. <br>

<h1>Username Enumeration</h1>

I am using Kerbrute for user enumeration on Active Directory

<img src=https://github.com/prasannashah1/Office-Writeup/assets/28432698/804b0822-d6b3-4976-8220-a8931fcf0dae height=50 width=1000> <br>

<img src=https://github.com/prasannashah1/Office-Writeup/assets/28432698/4b347624-0901-4794-80ca-f35e1050fcb8 height=300 width=1000> <br>

I saved the usernames in a text file and used the password obtained from Joomla vulnerability to try to login to the SMB.

<img src=https://github.com/prasannashah1/Office-Writeup/assets/28432698/97f52e57-f69b-4a1b-91e1-f363b1434c6c height=250 width=1000> <br>

Here, we can see that the user <b>dwolfe</b> can login to SMB and access it using smbclient.

<img src=https://github.com/prasannashah1/Office-Writeup/assets/28432698/69666f30-d48a-4cd5-8a4c-f8a2b428047b height=200 width=1000><br>

There's a pcap file inside. I downloaded it and opened it in wireshark. After spending some time, I found the packet number 1917 with protocol krb5. After reading this article https://vbscrub.com/2020/02/27/getting-passwords-from-kerberos-pre-authentication-packets/, I got pretty good understanding of converting this info to kerberos hash type.

<img src=https://github.com/prasannashah1/Office-Writeup/assets/28432698/04830c82-1767-483f-a6d4-f6239d8c398b height=500 width=1000><br>

<img src=https://github.com/prasannashah1/Office-Writeup/assets/28432698/25a8f2dd-74ae-4008-8acc-9c81289ebfa8 height=30 width=1000><br>

Now we can crack this hash using hashcat

<img src=https://github.com/prasannashah1/Office-Writeup/assets/28432698/cc5257a7-a6a7-4191-a715-3fb9e52e79a8 height=500 width=1000> <br>

I tried to login to the Joomla admin panel using username administrator and the cracked password and got logged in successfully.

<img src=https://github.com/prasannashah1/Office-Writeup/assets/28432698/3c287f5d-039c-4454-b738-409cc9882414 height=500 width=1000><br>

I then used pownyshell to get reverse shell by editing site template index.php file with pownyshell. This got me a reverse shell in the web application itself. I downloaed nc64.exe file and tried to get the reverse shell to my local machine.

<img src=https://github.com/prasannashah1/Office-Writeup/assets/28432698/015c1f47-1925-48d5-9f28-8825ace1766e height=500 width=1000><br>

<img src=https://github.com/prasannashah1/Office-Writeup/assets/28432698/6a24f481-29fa-45f4-9e19-f2fd519a9b98 height=500 width=1000><br>

<img src=https://github.com/prasannashah1/Office-Writeup/assets/28432698/ac7505b1-24b3-4a6c-b406-34b918f5b75f height=400 width=1000><br>

<img src=https://github.com/prasannashah1/Office-Writeup/assets/28432698/825c4210-baf2-4261-bb4e-46079370d6ad height=200 width=1000><br>

<img src=https://github.com/prasannashah1/Office-Writeup/assets/28432698/7d0b0f9a-7194-492e-a3ab-f53ccc0d5609 height=200 width=1000><br>

After I got the reverse shell to my local machine, I tried to enter into the user folder, but ther was no permission to do so. So, I tried to get another privileged reverse shell using RunasCs.

<img src=https://github.com/prasannashah1/Office-Writeup/assets/28432698/084097c0-cf29-4ecd-a3f0-e714dd66859a height=330 width=1000><br>

<img src=https://github.com/prasannashah1/Office-Writeup/assets/28432698/7688c791-0413-460d-bbe7-01241fb09544 height=160 width=1000><br>

<img src=https://github.com/prasannashah1/Office-Writeup/assets/28432698/a15f30a1-bb4f-423e-b3fa-bc1544c9a8a9 height=90 width=1000><br>


































































  

