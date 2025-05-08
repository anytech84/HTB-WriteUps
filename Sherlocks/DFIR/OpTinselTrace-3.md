# OpTinselTrace-3 

[OpTinselTrace-3](https://app.hackthebox.com/sherlocks/OpTinselTrace-3)

## Description

Oh no! Our IT admin is a bit of a cotton-headed ninny-muggins, ByteSparkle left his VPN configuration file in our fancy private S3 location! The nasty attackers may have gained access to our internal network. We think they compromised one of our TinkerTech workstations. Our security team has managed to grab you a memory dump - please analyse it and answer the questions! Santa is waiting… Please note - these Sherlocks are built to be completed sequentially and in order!

## Questions

1. What is the name of the file that is likely copied from the shared folder (including the file extension)?

```powershell
volatility 3 -f santaclaus.bin windows.filescan | grep Desktop
```

> `present_for_santa.zip` 

2. What is the file name used to trigger the attack (including the file extension)?

```powershell
volatility 3 -f santaclaus.bin windows.dumpfile --virtaddr 0xa48df8fb42a0
```

> `present.vbs`

3. What is the name of the file executed by click_for_present.lnk (including the file extension)?

```powershell
exiftool click_for_present.lnk
```

> `powershell.exe` 

4. What is the name of the program used by the vbs script to execute the next stage?

> `WrapPresent`

5. What is the URL that the next stage was downloaded from?

[VirusTotal](https://www.virustotal.com/gui/file/78ba1ea3ac992391010f23b346eedee69c383bc3fd2d3a125ede6cba3ce77243/behavior)

> `http://77.74.198.52/destroy_christmas/evil_present.jpg`

6. What is the IP and port that the executable downloaded the shellcode from (IP:Port)?

> `77.74.198.52:445`

7. What is the process ID of the remote process that the shellcode was injected into?

```powershell
volatility -f santaclaus.bin windows.netscan | grep 77.74.198.52
```

> `724` 

8. After the attacker established a Command & Control connection, what command did they use to clear all event logs?

9. What is the full path of the folder that was excluded from defender?

10. What is the original name of the file that was ingressed to the victim?
