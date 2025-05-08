# Origins

[Origins](https://app.hackthebox.com/sherlocks/Origins)

## Description

A major incident has recently occurred at Forela. Approximately 20 GB of data were stolen from internal s3 buckets and the attackers are now extorting Forela. During the root cause analysis, an FTP server was suspected to be the source of the attack. It was found that this server was also compromised and some data was stolen, leading to further compromises throughout the environment. You are provided with a minimal PCAP file. Your goal is to find evidence of brute force and data exfiltration.

## Questions

1. What is the attacker's IP address?

> `15.206.185.207`

2. It's critical to get more knowledge about the attackers, even if it's low fidelity. Using the geolocation data of the IP address used by the attackers, what city do they belong to?

> `Mumbai`

3. Which FTP application was used by the backup server? Enter the full name and version. (Format: Name Version)

> `vsFTPd 3.0.5`

4. The attacker has started a brute force attack on the server. When did this attack start?

> `2024-05-03 04:12:54`

5. What are the correct credentials that gave the attacker access? (Format username:password)

> `forela-ftp:ftprocks69$`

6. The attacker has exfiltrated files from the server. What is the FTP command used to download the remote files?

> `RETR`

7. Attackers were able to compromise the credentials of a backup SSH server. What is the password for this SSH server?

> `**B@ckup2024!**`

8. What is the s3 bucket URL for the data archive from 2023?

> `https://2023-coldstorage.s3.amazonaws.com`

9. The scope of the incident is huge as Forela's s3 buckets were also compromised and several GB of data were stolen and leaked. It was also discovered that the attackers used social engineering to gain access to sensitive data and extort it. What is the internal email address used by the attacker in the phishing email to gain access to sensitive data stored on s3 buckets?

> `archivebackups@forela.co.uk`
