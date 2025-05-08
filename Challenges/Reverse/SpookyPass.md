# SpookyPass

- **Category**: Reverse 
- **Points**: 0
- ** Difficulty**: Very Easy

[SpookyPass]((https://app.hackthebox.com/challenges/SpookyPass)

## Description

All the coolest ghosts in town are going to a Haunted Houseparty - can you prove you deserve to get in?

## Attachments

- Attached Files : `pass`

## Solution

Let's look at the file.

```bash
% file pass
pass: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=3008217772cc2426c643d69b80a96c715490dd91, for GNU/Linux 4.4.0, not stripped
```

It's an ELF file in 64-bit. Maybe we can find strings in the file.

```bash
% strings pass
...
s3cr3t_p455_f0r_gh05t5_4nd_gh0ul5
...
```

> We found the password `s3cr3t_p455_f0r_gh05t5_4nd_gh0ul5`. Let's try it.

```bash
admin@kali:/Users/admin/rev_spookypass$ ./pass
Welcome to the SPOOKIEST party of the year.
Before we let you in, you'll need to give us the password: s3cr3t_p455_f0r_gh05t5_4nd_gh0ul5
Welcome inside!
```


## Flag

`HTB{un0bfu5c4t3d_5tr1ng5}`
