TASK 1

What is considered to be one of the most essential skills to possess as a Penetration Tester?

dir busting

*** 

TASK 2

What switch do we use for nmap's scan to specify that we want to perform version detection

    -sV

*** 

TASK 3

What service type is identified as running on port 80/tcp in our nmap scan?

HTTP

*** 

TASK 4

What service name and version of service is running on port 80/tcp in our nmap scan?

nginx 1.14.2


*** 

TASK 5

What is a popular directory busting tool we can use to explore hidden web directories and resources?

gobuster (written in Go)


*** 

TASK 6

What switch do we use to specify to gobuster we want to perform dir busting specifically?

dir


*** 

TASK 7

What page is found during our dir busting activities?

    ┌──(kali㉿kali)-[~/HTB-CTF/tier0/Explosion]
    └─$ gobuster dir -w /usr/share/wordlists/dirb/common.txt -u 10.129.18.91
    
    ...
    
    ===============================================================
    2022/05/23 20:57:04 Starting gobuster in directory enumeration mode
    ===============================================================
    /admin.php            (Status: 200) [Size: 999]


admin.php

*** 

TASK 8

What is the status code reported by gobuster upon finding a successful page?

200 (SUCCESS)


*** 

# Reproduce

    http://10.129.18.91/admin.php
    
    Username: admin
    Password: admin
    
    Admin Console Login
    Congratulations! Your flag is: 6483bee07c1c1d57f14e5b0717503c73

# FLAG

6483bee07c1c1d57f14e5b0717503c73
