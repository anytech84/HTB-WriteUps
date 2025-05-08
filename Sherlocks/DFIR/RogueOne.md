# RogueOne

[RogueOne](https://app.hackthebox.com/sherlocks/RogueOne)

## Description

Your SIEM system generated multiple alerts in less than a minute, indicating potential C2 communication from Simon Stark's workstation. Despite Simon not noticing anything unusual, the IT team had him share screenshots of his task manager to check for any unusual processes. No suspicious processes were found, yet alerts about C2 communications persisted. The SOC manager then directed the immediate containment of the workstation and a memory dump for analysis. As a memory forensics expert, you are tasked with assisting the SOC team at Forela to investigate and resolve this urgent incident.

### Question

1. Please identify the malicious process and confirm process id of malicious process.

```bash
volatility -f dump windows.pslist
```

> `6812`

2. The SOC team believe the malicious process may spawned another process which enabled threat actor to execute commands. What is the process ID of that child process?

```bash
volatility -f dump windows.pstree | grep 6812
```

> `4364`

3. The reverse engineering team need the malicious file sample to analyze. Your SOC manager instructed you to find the hash of the file and then forward the sample to reverse engineering team. Whats the md5 hash of the malicious file?

```bash
volatility -f 20230810.mem windows.dumpfiles --pid 6812
md5sum dump/*svchost.exe.img
```

> `5bd547c6f5bfc4858fe62c8867acfbb5`

4. In order to find the scope of the incident, the SOC manager has deployed a threat hunting team to sweep across the environment for any indicator of compromise. It would be a great help to the team if you are able to confirm the C2 IP address and ports so our team can utilise these in their sweep.

```bash
volatility -f 20230810.mem windows.netscan
```

> `13.127.155.166:8888`

5. We need a timeline to help us scope out the incident and help the wider DFIR team to perform root cause analysis. Can you confirm time the process was executed and C2 channel was established?

```bash
volatility -f 20230810.mem windows.cmdscan
```

> 

6. What is the memory offset of the malicious process?

> `2023-08-10 11:30:03`

7. You successfully analyzed a memory dump and received praise from your manager. The following day, your manager requests an update on the malicious file. You check VirusTotal and find that the file has already been uploaded, likely by the reverse engineering team. Your task is to determine when the sample was first submitted to VirusTotal.

[VirusTotal](https://www.virustotal.com/gui/file/eaf09578d6eca82501aa2b3fcef473c3795ea365a9b33a252e5dc712c62981ea/detail)s

> `2023-08-10 11:58:10`

