# Debugging Interface

- **Category:** Hardware
- **Points:** 10
- **Difficulty:** Very Easy

[Debugging_Interface](https://app.hackthebox.com/challenges/207)

## Description

We accessed the embedded device's asynchronous serial debugging interface while it was operational and captured some messages that were being transmitted over it. Can you decode them?

## Attachments

- Attached files: `debugging_interface.sal`

## Solution

The attached file is a .sal file, which is a Saleae Logic Analyzer file. 
We can use the Saleae Logic software to open the file and see the captured messages.
As in the  description, we can see that the messages are being transmitted over a serial interface.
We must decode the signal as a asynchronous serial signal with the analyzer `Async Serial`.
For that, we must calculate the baud rate and the other parameters of the signal.

> Take the most rapid transitions in the signal, the width displayed is the time of one bit.
> The baud rate is approximately similar to this width.
> So, for a width of 31,211 kHz, the baud rate is 31211 bps.

## Flag

`HTB{d38u991n9_1n732f4c35_c4n_83_f0und_1n_41m057_3v32y_3m83dd3d_d3v1c3!!52}`
