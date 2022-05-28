# Ignition VIP

# Table of Contents

- [Research](#research)
- [Reproduce](#reproduce)
- [Flag](#flag)

# Research

TASK 1

Which service version is found to be running on port 80?

    ┌──(kali㉿kali)-[~]
    └─$ nmap 10.129.1.27 -p 80 -A
    Starting Nmap 7.92 ( https://nmap.org ) at 2022-05-28 16:43 BST
    Nmap scan report for 10.129.1.27
    Host is up (0.016s latency).

    PORT   STATE SERVICE VERSION
    80/tcp open  http    nginx 1.14.2
    |_http-title: Did not follow redirect to http://ignition.htb/
    |_http-server-header: nginx/1.14.2

nginx 1.14.2

TASK 2

What is the 3-digit HTTP status code returned when you visit http://{machine IP}/?

    ┌──(kali㉿kali)-[~]
    └─$ curl -v 10.129.1.27
    *   Trying 10.129.1.27:80...
    * Connected to 10.129.1.27 (10.129.1.27) port 80 (#0)
    > GET / HTTP/1.1
    > Host: 10.129.1.27
    > User-Agent: curl/7.81.0
    > Accept: */*
    > 
    * Mark bundle as not supporting multiuse
    < HTTP/1.1 302 Found
    < Server: nginx/1.14.2
    < Date: Sat, 28 May 2022 15:47:04 GMT
    < Content-Type: text/html; charset=UTF-8
    < Transfer-Encoding: chunked
    < Connection: keep-alive
    < Set-Cookie: PHPSESSID=dd1v3scvje5qg0t0d821m285ip; expires=Sat, 28-May-2022 16:47:04 GMT; Max-Age=3600; path=/; domain=10.129.1.27; HttpOnly; SameSite=Lax
    < Location: http://ignition.htb/
    < Pragma: no-cache
    < Cache-Control: max-age=0, must-revalidate, no-cache, no-store
    < Expires: Fri, 28 May 2021 15:47:04 GMT
    < Content-Security-Policy-Report-Only: font-src data: 'self' 'unsafe-inline'; form-action secure.authorize.net test.authorize.net geostag.cardinalcommerce.com geo.cardinalcommerce.com 1eafstag.cardinalcommerce.com 1eaf.cardinalcommerce.com centinelapistag.cardinalcommerce.com centinelapi.cardinalcommerce.com 'self' 'unsafe-inline'; frame-ancestors 'self' 'unsafe-inline'; frame-src fast.amc.demdex.net secure.authorize.net test.authorize.net geostag.cardinalcommerce.com geo.cardinalcommerce.com 1eafstag.cardinalcommerce.com 1eaf.cardinalcommerce.com centinelapistag.cardinalcommerce.com centinelapi.cardinalcommerce.com www.paypal.com www.sandbox.paypal.com player.vimeo.com *.youtube.com 'self' 'unsafe-inline'; img-src assets.adobedtm.com amcglobal.sc.omtrdc.net dpm.demdex.net cm.everesttech.net widgets.magentocommerce.com data: www.googleadservices.com www.google-analytics.com www.paypalobjects.com t.paypal.com www.paypal.com fpdbs.paypal.com fpdbs.sandbox.paypal.com *.vimeocdn.com i.ytimg.com s.ytimg.com data: 'self' 'unsafe-inline'; script-src assets.adobedtm.com secure.authorize.net test.authorize.net www.googleadservices.com www.google-analytics.com www.paypalobjects.com js.braintreegateway.com www.paypal.com geostag.cardinalcommerce.com 1eafstag.cardinalcommerce.com geoapi.cardinalcommerce.com 1eafapi.cardinalcommerce.com songbird.cardinalcommerce.com includestest.ccdc02.com www.sandbox.paypal.com t.paypal.com s.ytimg.com www.googleapis.com vimeo.com www.vimeo.com *.vimeocdn.com www.youtube.com video.google.com 'self' 'unsafe-inline' 'unsafe-eval'; style-src getfirebug.com 'self' 'unsafe-inline'; object-src 'self' 'unsafe-inline'; media-src 'self' 'unsafe-inline'; manifest-src 'self' 'unsafe-inline'; connect-src dpm.demdex.net amcglobal.sc.omtrdc.net www.google-analytics.com geostag.cardinalcommerce.com geo.cardinalcommerce.com 1eafstag.cardinalcommerce.com 1eaf.cardinalcommerce.com centinelapistag.cardinalcommerce.com centinelapi.cardinalcommerce.com 'self' 'unsafe-inline'; child-src http: https: blob: 'self' 'unsafe-inline'; default-src 'self' 'unsafe-inline' 'unsafe-eval'; base-uri 'self' 'unsafe-inline';
    < X-Content-Type-Options: nosniff
    < X-XSS-Protection: 1; mode=block
    < X-Frame-Options: SAMEORIGIN
    < 
    * Connection #0 to host 10.129.1.27 left intact

HTTP/1.1 302 Found

TASK 3

What is the virtual host name the webpage expects to be accessed by?

    ignition.htb

TASK 4

What is the full path to the file on a Linux computer that holds a local list of domain name to IP address pairs?

    /etc/hosts

TASK 5

What is the full URL to the Magento login page?

    ┌──(kali㉿kali)-[~]
    └─$ gobuster dir -w /usr/share/wordlists/dirb/common.txt -u http://ignition.htb      
    ===============================================================
    Gobuster v3.1.0
    by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
    ===============================================================
    [+] Url:                     http://ignition.htb
    [+] Method:                  GET
    [+] Threads:                 10
    [+] Wordlist:                /usr/share/wordlists/dirb/common.txt
    [+] Negative Status codes:   404
    [+] User Agent:              gobuster/3.1.0
    [+] Timeout:                 10s
    ===============================================================
    2022/05/28 16:54:14 Starting gobuster in directory enumeration mode
    ===============================================================
    /0                    (Status: 200) [Size: 25803]
    /admin                (Status: 200) [Size: 7095] 


http://ignition.htb/admin


TASK 6

What password provides access as admin to Magento?

Due to brute-force mitigations this tasks involves trying common passwords and is quite **slow**

    admin123
    root123
    password1
    administrator1 
    changeme1 
    password123 
    qwerty123 
    administrator123 
    changeme123

Correct login
admin:qwerty123

# Reproduce

Edit `/etc/hosts` to include `10.129.1.27     ignition.htb`

Visit http://ignition.htb/admin using login credentials:

    Username: admin
    Password: qwerty123

<img width="1016" alt="image" src="https://user-images.githubusercontent.com/10171446/170833473-f6a7ca59-28fe-4dc8-8b7c-c836695f74aa.png">


# Flag

797d6c988d9dc5865e010b9410f247e0
