# PermX

`PermX` is an Easy Difficulty Linux machine featuring a learning management system vulnerable to unrestricted file uploads via [CVE-2023-4220](https://nvd.nist.gov/vuln/detail/CVE-2023-4220). This vulnerability is leveraged to gain a foothold on the machine. Enumerating the machine reveals credentials that lead to SSH access. A `sudo` misconfiguration is then exploited to gain a `root` shell. 

- **Difficulty:** Easy
- **OS:** Linux

[PermX](https://app.hackthebox.com/machines/PermX)

## Recon

- `nmap`

```bash
Nmap scan report for permx.htb (10.10.11.23)
Host is up (0.20s latency).
Not shown: 997 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   256 4c:0f:3b:8d:5e:1a:2f:9c:6b:7d:3e:0f:4a:5b:8c (ECDSA)
|_  256 4c:0f:3b:8d:5e:1a:2f:9c:6b:7d:3e:0f:4a:5b:8c (ED25519)
80/tcp open  http    Apache httpd 2.4.52 ((Ubuntu))
````

## Enumeration

## Initial Access

## Post-Exploitation

## Privilege Escalation

