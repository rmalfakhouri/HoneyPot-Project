# HoneyPot-Project

---------------------------------------------------------------------

This project shows how I set up a Cowrie SSH honeypot inside a Kali Linux virtual machine using Docker. 
Cowrie acts like a vulnerable SSH server and records everything an attacker tries. Such as usernames, passwords, and commands. 
After generating activity, I reviewed the logs to understand how brute-force attempts work and how attackers try to break into exposed systems.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------

#Lab Goals#
- Deploy a safe, isolated SSH honeypot using Cowrie.
- Expose the honeypot on a custom SSH port (2222).
- Generate fake attacker activity (brute-force attempts, commands).
- View and analyze Cowrie logs.
- Understand how real attackers might abuse SSH and how defenders can monitor it.

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

#Environment#
- Host OS: Windows
- Virtual Machine: VirtualBox - Kali Linux
- Container Platform: Docker
- Honeypot: Cowrie (SSH/Telnet honeypot)
  
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

#Commands#

(Part 1- The Attack) 

NOTE: This isn’t a full script. There were additional commands and steps in between, but these are just the main commands.
1) sudo apt install docker.io : Docker is a container, I installed it inside my kali virtual machine so I could safely run the Cowrie honeypot in an isolated environment.

2) sudo docker run -d -p 2222:2222 cowrie/cowrie  : I mapped port 2222 on my kali to port 2222 inside the container , making the fake SSH servide accessible for attackers to locate and brake into! cowrie/cowrie is the honeypot im running

3) sudo docker ps : we use it to shows all running Docker containers AND to get the container ID . We'll use the container id later to see the log folder


<img width="1038" height="143" alt="HoneyPot_Container" src="https://github.com/user-attachments/assets/fcae323f-2183-461d-96ce-009af6f3d1e9" />


4) ip a  : This acts like ifconfig, it shows kali VM's IP addresses. After you find the correct one you will later plug it in the MAIN attack command


<img width="869" height="698" alt="HoneyPot_IPAddress" src="https://github.com/user-attachments/assets/5bf4e335-e897-41ab-b757-b1448a8ca6da" />
   


5) ssh root@<kali-ip> -p 2222  :  This is the attack command. It attempts to connect to the honeypot I craeted. Every login attempt and command is logged by Cowrie.

<img width="749" height="118" alt="HoneyPot_AttackCode" src="https://github.com/user-attachments/assets/f7ce7df7-7b06-44ab-9982-97e80c674c0a" />


Here, it will accept ANY password. Cowrie accepted any password you typed because it is a honeypot, not a real SSH server. It is designed to pretend to be vulnerable so attackers think they successfully logged in ! 





(Part 2- Checking Logs/ Results)
1) -mkdir cowrie-logs                               
   -sudo docker cp <CONTAINER_ID>:/cowrie/var/log/cowrie/cowrie.log cowrie-logs   :  I created a directory and copied logs into it, instead of accessing it directly because it wouldnt let me. Notice the container id spot I mentioned earlier.


2)  cat cowrie.log    : OPENED THE LOG FILEEEEE

<img width="980" height="770" alt="HoneyPot_EventLog" src="https://github.com/user-attachments/assets/32caedf8-660f-4439-9629-59550fc3714d" />








-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
#Trouble Shooting I went through :

1. “Connection timed out” when connecting from Windows to the honeypot to try it out : Kali VM was using NAT mode, which blocks outside devices.To fix it, I changed the VM’s network setting to Bridged Adapter. This
   allows me to try to break in using any outside devide and not just from my local kali! you can have your friends break in using their OWN computer, and see their action logs after! 

-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

#Real Life Scenario / How Real Hackers Actually Do This:

Hackers scan networks using tools like Nmap to discover which machines are online and what ports are open.
When they find an open SSH port (usually port 22), they assume it might allow remote login.
After identifying the target, hackers try to connect using SSH
Hackers then perform brute-force attacks by trying thousands of username/password combinations using tools like Hydra or Medusa.
If attackers guess the right password, they gain access and begin running commands to explore the system, steal data, or install malware.





