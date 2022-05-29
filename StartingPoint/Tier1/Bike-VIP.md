# Table of Contents

- [Research](#research)
- [Reproduce](#reproduce)
- [Flag](#flag)

# Research

TASK 1

What TCP ports does nmap identify as open? Answer with a list of ports seperated by commas with no spaces, from low to high.

    ┌──(kali㉿kali)-[~]
    └─$ sudo nmap 10.129.97.64 -p- -Pn                                                                                                                         
    Starting Nmap 7.92 ( https://nmap.org ) at 2022-05-29 16:19 BST
    Nmap scan report for 10.129.97.64
    Host is up (0.025s latency).
    Not shown: 65533 closed tcp ports (reset)
    PORT   STATE SERVICE
    22/tcp open  ssh
    80/tcp open  http

    Nmap done: 1 IP address (1 host up) scanned in 16.09 seconds


22,80

TASK 2

What software is running the service listening on the http/web port identified in the first question?

    ┌──(kali㉿kali)-[~]
    └─$ sudo nmap 10.129.97.64 -p 22,80 -sC -sV -Pn
    Starting Nmap 7.92 ( https://nmap.org ) at 2022-05-29 16:23 BST
    Nmap scan report for 10.129.97.64
    Host is up (0.016s latency).

    PORT   STATE SERVICE VERSION
    22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.4 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   3072 48:ad:d5:b8:3a:9f:bc:be:f7:e8:20:1e:f6:bf:de:ae (RSA)
    |   256 b7:89:6c:0b:20:ed:49:b2:c1:86:7c:29:92:74:1c:1f (ECDSA)
    |_  256 18:cd:9d:08:a6:21:a8:b8:b6:f7:9f:8d:40:51:54:fb (ED25519)
    80/tcp open  http    Node.js (Express middleware)
    |_http-title:  Bike 
    Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

    Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
    Nmap done: 1 IP address (1 host up) scanned in 7.32 seconds

Node.js


TASK 3

What is the name of the Web Framework according to Wappalyzer?

[Wappalyzer Mozilla](https://addons.mozilla.org/en-US/firefox/addon/wappalyzer/)

<img width="492" alt="image" src="https://user-images.githubusercontent.com/10171446/170877416-b26e6d47-5f77-4314-b4f2-f0450bece22c.png">

Express

TASK 4

What is the name of the vulnerability we test for by submitting {{7*7}}?

Server Side Template Injection (SSTI)

TASK 5

What is the templating engine being used within Node.JS?

https://portswigger.net/research/server-side-template-injection



TASK 6

What is the name of the BurpSuite tab used to encode text?

TASK 7

In order to send special characters in our payload in an HTTP request, we'll encode the payload. What type of encoding do we use?

TASK 8

When we use a payload from HackTricks to try to run system commands, we get an error back. What is "not defined" in the response error?

TASK 9

What variable is the name of the top-level scope in Node.JS?

TASK 10

By exploiting this vulnerability, we get command execution as the user that the webserver is running as. What is the name of that user?



# Reproduce


# Flag
