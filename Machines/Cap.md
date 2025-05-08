# Alert

`Cap` is a Linux machine that is rated as easy. It is a great box for beginners and is a good way to get introduced to the platform. 

- **Difficulty:** Easy
- **OS:** Linux

[Cap](https://app.hackthebox.com/machines/Cap)

## `nmap`

We will begin by enumerating open services using `nmap`. 
We will find that there is a web server running on port 80 so a FTP server is also running on port 21. 

```bash
PORT    STATE SERVICE
21/tcp open  ftp
22/tcp open  ssh
80/tcp open  http
```

## `IDOR`

Let's open the web server in our browser. It's a kinda network monitoring tool.
In Security Snapshot, we can retrieve a pcap of our current interaction with the web server. 
Url is : `http://cap.htb/data/xx`
Before we download the pcap, let's see if we can change the `xx` to something else. 
Let's try `http://cap.htb/data/0` and we can see that we can download the pcap file. I contains packets !

## Wireshark

Opening the ID 0 capture file in Wireshark reveals FTP traffic, including the user authentication.
The traffic is not encrypted, allowing us to retrieve the user credentials i.e. nathan /
Buck3tH4TF0RM3!. These are found to be valid not only for FTP but can be used to login via SSH.

## User Flag

Let's now connect to the machine using the `nathan` user.

```bash
ssh albert@alert.htb
```

> We can now read the `user.txt` file in the home directory of the `albert` user.

## Root Flag

Let's see what Albert can do. There is LinPeas script and its results in the home directory.
The report contains an interesting entry for files with capabilities. The /usr/bin/python3.8 is
found to have `cap_setuid` and `cap_net_bind_service`, which isn't the default setting.
According to the documentation, CAP_SETUID allows the process to gain setuid privileges without
the SUID bit set. This effectively lets us switch to UID 0 i.e. root. The developer of Cap must have
given Python this capability to enable the site to capture traffic, which a non-root user can't do.
The following Python commands will result in a root shell:

```bash
python3.8 -c 'import os; os.setuid(0); os.system("/bin/bash")'
> id
uid=0(root) gid=0(root) groups=0(root)
```

> We can now read the `root.txt` file in the root directory.
