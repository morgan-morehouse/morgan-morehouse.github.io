---
layout: post
title: "Team Task: Lockout (Team 3)"
---

First, we looked at the IP addresses of our 2 working computers. We ran the nmap command, but got confused because we didn’t understand how to use it yet. Then we ran Ifconfig to find our subnet mask and IP addresses, and did a couple of binary combinations to find that it was /22.

We ran the command nmap -v -sn 255.255.252.0/24 10.0.140.0/22 to find what IPs were online, but realized we couldn’t find stuff easily so instead ran nmap -v -sn 255.255.252.0/24 10.0.140.0/22 > nmap.txt. Then we ran less nmap to find the open connections on the baysplash network. Some people were on it with their Macbooks so the number of online IP addresses would change, but eventually we figured out which IPs belonged to the Raspberry Pis. 

To find the IP of our locked computer, we asked other students what IP addresses their working computers were, so we could limit the suspects to 4 computers. Then we turned off our locked computer and ran the nmap command again to see which IP address turned off, which was 10.0.140.7. We tried to use ssh to log in to the locked computer and realized we didn’t have a password. 

Using the process of elimination with information from the other groups, we found Corey’s (cseh-0) IP address (10.0.140.3), and used ssh to connect to Corey with an unchanged password (security0) to ask for a hint. From this, we found out we needed to use the program Hydra. So, we switched networks to guest so that we could access the internet, and installed Hydra with the command sudo apt-get install hydra-gtk. Then we looked up the syntax for Hydra and realized we needed a text file of common passwords. We tried to create one and realized that that's not how Hydra works, so we used ssh to connect to Corey’s computer and got the next hint, learning that we needed to find rockyou.txt somewhere on his computer.

So, we ran the command sudo find / -name rockyou.txt on Corey’s computer, and found it in one of his folders. We tried to copy it over to one of our computer using scp, but it wasn’t giving permission to copy over so we tried to use sudo, and also realized that's not how it works.

Then we realized that we didn’t need to move it to one of our computers, and ran Hydra on Corey’s computer: Hydra -l cseh-9 -P rockyou.txt 10.0.140.7 ssh -t 4. This found the password for the locked computer, which was 123456. We entered this into the computer and then referenced the man page for the passwd command. To change the password back, we ran sudo passwd cseh-9, entered the previous password (123456), and set the new password to security9.
