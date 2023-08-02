# Sau
#### Sau Hack The box

Hello Hackers, I'm [0xDigimon](https://www.linkedin.com/in/abdelmawla-elamrosy/)<br><img src ="https://github.com/0xDigimon/CyLert-Internship/blob/main/HTB-Machines/Sau/media/01.png?raw=true"><br>started nmap scan ```nmap -Pn 10.10.11.224``` and we got on ssh,http,unknow service <br><img src ="https://github.com/0xDigimon/CyLert-Internship/blob/main/HTB-Machines/Sau/media/02.png?raw=true"><br>i dicovered http port and nothing important or the port is clsed so i did nmap sccan ```nmap -Pn 10.10.11.224 -sV -sC --script vuln ``` and i got some reqs. from port 55555 <br><img src ="https://github.com/0xDigimon/CyLert-Internship/blob/main/HTB-Machines/Sau/media/03.png?raw=true"><br>after dicover website i got on request-basket version 1.2.1 <br><img src ="https://github.com/0xDigimon/CyLert-Internship/blob/main/HTB-Machines/Sau/media/04.png?raw=true"><br>after some search i got this [repo](https://gist.github.com/b33t1e/3079c10c88cad379fb166c389ce3b7b3) with CVE-2023-27163 and now we have a ssrf vulnerability <br><img src ="https://github.com/0xDigimon/CyLert-Internship/blob/main/HTB-Machines/Sau/media/05.png?raw=true"><br>now if we access the website we got Mailtrail page after some search about this service i got on report about [Mailtrail](https://huntr.dev/bounties/be3c5204-fbd9-448d-b97c-96a8d2941e87/) <= v0.54 <br><img src ="https://github.com/0xDigimon/CyLert-Internship/blob/main/HTB-Machines/Sau/media/06.png?raw=true"><br> now apply this report on my case <br>
- create reverce shell using bash script <br>
- open http server <br>
- run command <br><img src ="https://github.com/0xDigimon/CyLert-Internship/blob/main/HTB-Machines/Sau/media/07.png?raw=true"><br>
now we have shellsession on the machine <br>
upgrade our shell <br><img src ="https://github.com/0xDigimon/CyLert-Internship/blob/main/HTB-Machines/Sau/media/08.png?raw=true"><br>and discover our privilege using ```sudo -l``` i got ```/usr/bin/systemctl status trail.service``` i can use this script with root privillege without password after run this command <br><img src ="https://github.com/0xDigimon/CyLert-Internship/blob/main/HTB-Machines/Sau/media/09.png?raw=true"><br>using [GTFOBins](https://gtfobins.github.io/) i search on systemctl and i follow the website and i escalate my privillege to root
<br><img src ="https://github.com/0xDigimon/CyLert-Internship/blob/main/HTB-Machines/Sau/media/10.png?raw=true"><br>
<hr>

### Thanks For Reaading (:
<hr>
