# Research

TASK 1

What nmap scanning switch employs the use of default scripts during a scan?

-sC

TASK 2

What service version is found to be running on port 21?

-sC or -sV are both valid

    ┌──(kali㉿kali)-[~]
    └─$ nmap 10.129.19.63 -p 20,21 -sV   
    Starting Nmap 7.92 ( https://nmap.org ) at 2022-05-25 00:27 BST
    Nmap scan report for 10.129.19.63
    Host is up (0.022s latency).

    PORT   STATE  SERVICE  VERSION
    20/tcp closed ftp-data
    21/tcp open   ftp      vsftpd 3.0.3
    Service Info: OS: Unix


vsftpd 3.0.3

TASK 3

What FTP code is returned to us for the "Anonymous FTP login allowed" message?

Returned during -sC portscan

    ┌──(kali㉿kali)-[~]
    └─$ nmap 10.129.19.63 -p 20,21 -sC
    Starting Nmap 7.92 ( https://nmap.org ) at 2022-05-25 00:28 BST
    Nmap scan report for 10.129.19.63
    Host is up (0.021s latency).

    PORT   STATE  SERVICE
    20/tcp closed ftp-data
    21/tcp open   ftp
    | ftp-syst: 
    |   STAT: 
    | FTP server status:
    |      Connected to ::ffff:10.10.14.78
    |      Logged in as ftp
    |      TYPE: ASCII
    |      No session bandwidth limit
    |      Session timeout in seconds is 300
    |      Control connection is plain text
    |      Data connections will be plain text
    |      At session startup, client count was 3
    |      vsFTPd 3.0.3 - secure, fast, stable
    |_End of status
    | ftp-anon: Anonymous FTP login allowed (FTP code 230)
    | -rw-r--r--    1 ftp      ftp            33 Jun 08  2021 allowed.userlist
    |_-rw-r--r--    1 ftp      ftp            62 Apr 20  2021 allowed.userlist.passwd

    Nmap done: 1 IP address (1 host up) scanned in 0.51 seconds

FTP code **230**

TASK 4

What command can we use to download the files we find on the FTP server?

GET

TASK 5

What is one of the higher-privilege sounding usernames in the list we retrieved?

After logging into FTP server and issuing $ls we can FTP>GET ALL

    ┌──(kali㉿kali)-[~]
    └─$ ftp 10.129.19.63                                                                                                                                                        127 ⨯
    Connected to 10.129.19.63.
    220 (vsFTPd 3.0.3)
    Name (10.129.19.63:kali): anonymous
    230 Login successful.
    Remote system type is UNIX.
    Using binary mode to transfer files.
    ftp> ls
    229 Entering Extended Passive Mode (|||40314|)
    150 Here comes the directory listing.
    -rw-r--r--    1 ftp      ftp            33 Jun 08  2021 allowed.userlist
    -rw-r--r--    1 ftp      ftp            62 Apr 20  2021 allowed.userlist.passwd
    226 Directory send OK.
    ftp> get all
    allowed.userlist	allowed.userlist.passwd

The highest privilege user from allowed.userlist appears to be 'admin'

    ┌──(kali㉿kali)-[~]
    └─$ cat allowed.userlist
    aron
    pwnmeow
    egotisticalsw
    admin

    ┌──(kali㉿kali)-[~]
    └─$ cat allowed.userlist.passwd 
    root
    Supersecretpassword1
    @BaASD&9032123sADS
    rKXM59ESxesUFHAd

admin

TASK 6

What version of Apache HTTP Server is running on the target host?

    ┌──(kali㉿kali)-[~]
    └─$ nmap 10.129.19.63 -p 80 -sV
    Starting Nmap 7.92 ( https://nmap.org ) at 2022-05-25 00:37 BST
    Nmap scan report for 10.129.19.63
    Host is up (0.016s latency).

    PORT   STATE SERVICE VERSION
    80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))

2.4.41

TASK 7

What is the name of a handy web site analysis plug-in we can install in our browser?


Wappalyzer


TASK 8

What switch can we use with gobuster to specify we are looking for specific filetypes?

    ┌──(kali㉿kali)-[~]
    └─$ gobuster dir --help
    Uses directory/file enumeration mode

    Usage:
      gobuster dir [flags]

    Flags:
      -f, --add-slash                       Append / to each request
      -c, --cookies string                  Cookies to use for the requests
      -d, --discover-backup                 Upon finding a file search for backup files
          --exclude-length ints             exclude the following content length (completely ignores the status). Supply multiple times to exclude multiple sizes.
      -e, --expanded                        Expanded mode, print full URLs
      -x, --extensions string               File extension(s) to search for


-x


TASK 9

What file have we found that can provide us a foothold on the target?

    ┌──(kali㉿kali)-[~]
    └─$ gobuster dir -w /usr/share/wordlists/dirb/common.txt -u 10.129.19.63 -x php
    ==============================================================
    Gobuster v3.1.0
    by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
    ===============================================================
    [+] Url:                     http://10.129.19.63
    [+] Method:                  GET
    [+] Threads:                 10
    [+] Wordlist:                /usr/share/wordlists/dirb/common.txt
    [+] Negative Status codes:   404
    [+] User Agent:              gobuster/3.1.0
    [+] Extensions:              php
    [+] Timeout:                 10s
    ===============================================================
    2022/05/25 00:47:15 Starting gobuster in directory enumeration mode
    ===============================================================
    /.hta                 (Status: 403) [Size: 277]
    /.hta.php             (Status: 403) [Size: 277]
    /.htaccess            (Status: 403) [Size: 277]
    /.htpasswd            (Status: 403) [Size: 277]
    /.htaccess.php        (Status: 403) [Size: 277]
    /.htpasswd.php        (Status: 403) [Size: 277]
    /assets               (Status: 301) [Size: 313] [--> http://10.129.19.63/assets/]
    /config.php           (Status: 200) [Size: 0]                                    
    /css                  (Status: 301) [Size: 310] [--> http://10.129.19.63/css/]   
    /dashboard            (Status: 301) [Size: 316] [--> http://10.129.19.63/dashboard/]
    /fonts                (Status: 301) [Size: 312] [--> http://10.129.19.63/fonts/]    
    /index.html           (Status: 200) [Size: 58565]                                   
    /js                   (Status: 301) [Size: 309] [--> http://10.129.19.63/js/]       
    /login.php            (Status: 200) [Size: 1577]                                    
    /logout.php           (Status: 302) [Size: 0] [--> login.php]                       
    /server-status        (Status: 403) [Size: 277]                                     

    ===============================================================
    2022/05/25 00:47:31 Finished
    ===============================================================
                                          
login.php

# Reproduce

Visiting http://10.129.19.63/login.php

Username: admin
Password: rKXM59ESxesUFHAd

    Dashboard: Here is your flag: c7110277ac44d78b6a9fff2232434d16


# Flag

c7110277ac44d78b6a9fff2232434d16
