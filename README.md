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






















  

