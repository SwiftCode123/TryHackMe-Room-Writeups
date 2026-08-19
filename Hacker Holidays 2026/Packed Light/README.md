<p align="center">
  <img src="https://assets.tryhackme.com/img/logo/tryhackme_logo_full.svg" width="150" alt="TryHackMe Logo">
</p>

# Packed Light

| Room Name | Packed Light |
|-----------|--------------|
| Platform | TryHackMe |
| Event | Hacker Holidays 2026 |
| Points | 60 |
| Category | 🔍 Forensics |
| Difficulty | Easy |
| Tags | PCAP Analysis, Network Forensics, Cryptography, Wireshark |
| Status | ✅ Completed |

## Room Information

```bash
Type: CTF Challenge
Difficulty: Easy
Category: Forensics

Description:
Tiny packets. Odd hours. Suspiciously regular. Someone's smuggling out the data equivalent of a hotel towel every night, folded neatly inside traffic that looks ordinary until you decode it.
```

## Challenge Overview

**Hacker Holidays: Day 4 — Packed Light**

A suspicious capture from the Byte Lotus Hotel guest network suggests that data is being quietly exfiltrated over what appears to be normal network traffic. Your task is to analyze the provided packet capture, uncover the covert communication channel, reconstruct the hidden data, and recover the challenge flag.

## Objectives

- Analyze the provided capture for a covert communication channel.
- Identify where the exfiltrated data is being hidden and reassemble it.
- Decode the recovered data and submit the flag.

## Solution

- This challenge had to do with `PCAP Analysis`, `Cryptography`, `Network Forensics` and so I needed to use Wireshark here 
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/17b297bb-8c1b-4558-9cab-e28bdc25b887" />
</p>

- Since this was downloaded locally onto my computer, I had to transfer them using `scp` to my Kali Linux machine on my VMware hypervisor
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/431c99c7-5d37-4e89-a375-a23a58e2397b" />
</p>

- Now, I was able to do some real packet analysis
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/feedf2f9-4ecc-4665-a272-ed99016c84f9" />
</p>

- First thing in Wireshark was to filter by `tcp.port == 8080` since Mia mentioned that in the comments
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/d154d5df-f11a-45e3-ab76-ee59401bcc9e" />
</p>

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/42af3f80-bca3-4bad-ad79-28bf4f78d786" />
</p>

- The challenge description mentions `odd hours`, `suspiciously regular`, and `every single second like clockwork` and here we can see requests happening sequentially. Every loop starts a new TCP connection on a new source port from `192.168.1.141` to the server at `34.41.103.191:8080`. As you can see it uses ports `50419`, `50436`, etc.

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/3908bec4-8cfa-4195-bd9a-60d4824dfe3d" />
</p>

- I can also see suspicious GET requests being made to the server (such as packets `16`, `19`, `391` and so on)
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/a8999d17-3148-46dc-b7e3-dbd99562c296" />
</p>

- I right clicked on packet `16` and followed the HTTP stream so I could see the headers and the payload clearly
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/c83aea20-0520-46c1-bf29-7387a1982ac1" />
</p>

- It seems like this is some sort of Python keylogging tool. It listens to all keyboard strokes using the `pynput.keyboard` module. Then, it takes every letter the person types and uses the key `H0t3lSt@ff0nlyK3epS3cr3t!` to mathematically scramble it into unreadable data bytes (XOR Encryption). Then it translates those bytes into standard letters and numbers using `Base64` encoding. The script then puts that `Base64` encoded text into the `hotel_sess_state` cookie

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/d1d06a04-58a0-4283-92a2-9a27dcb36831" />
</p>

- From what I understand, the victim’s computer is infected with this custom keylogger malware and as the victim types, the program intercepts every single key press in the background, encrypts it using the secret key, encodes it to `Base64`, and puts it into a cookie named `hotel_sess_state`. The victims computer makes a lot of `GET` requests for every character typed which sends that cookie to the attacker's server and the attacker returns a similar/fake looking webpage as denoted by the `200 OK` messages

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/62e56655-4932-43eb-8ee9-d6d977aab32f" />
</p>

- Knowing this, we can easily reverse the `Base64` encodings using a python script and just filter by `http.request` in Wireshark so we can get that cookie value

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/5d1bda9a-e459-4b33-a48f-c84104083906" />
</p>

- I clicked on packet `391` and we can see the cookie `hotel_sess_state` has a value of `HA==`. This value represents one letter that the victim typed which was sent to the attacker
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/531c34f1-d82c-4e55-bc09-826821cea7b3" />
</p>

- So, if we were to take this cookie value into CyberChef and put the `From Base64` first and then `XOR` because remember that the attacker's python script `XOR`'s it and then encodes it in `Base64` so we have to do the opposite in CyberChef, we get the value of `T` which is the beginning of the flag `THM{`

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/93cea486-e527-4d86-a297-e120c8d3ed63" />
</p>

- Now, as stated before, for me at least, this is time consuming so I just used tshark to get all the `Base64` values and then pipe that output into a Python script that decodes all the characters and prints the flag

- I used `tshark` to print all of the cookie values
```
tshark -r traffic.pcapng -Y "http.request" -T fields -e "http.cookie" | grep -oP "hotel_sess_state=\K[^;\s]+"
```
- This command uses `tshark -r traffic.pcapng` which is the CLI version of Wireshark and reads `traffic.pcapng`. It only reads HTTP requests as denoted by `-Y "http.request"`, `-T fields -e "http.cookie"` reads only the HTTP Cookie header and `grep -oP "hotel_sess_state=\K[^;\s]+"` extracts only the `hotel_sess_state` cookie

- These outputs are passed into the Python script which uses the key (`H0t3lSt@ff0nlyK3epS3cr3t!`) and it decodes the `Base64` value and then goes through XOR decoding and prints the character eventually printing the whole flag

```
tshark -r traffic.pcapng -Y "http.request" -T fields -e "http.cookie" | grep -oP "hotel_sess_state=\K[^;\s]+" | python3 -c '
import sys, base64
key = b"H0t3lSt@ff0nlyK3epS3cr3t!"
for i, line in enumerate(sys.stdin):
    line = line.strip()
    if line:
        decoded = base64.b64decode(line)
        print(chr(decoded[0] ^ key[0]), end="") 
print()'
```

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/327a7bd9-54f4-44ec-bb79-d07df45499c4" />
</p>

## Skills Learned

* PCAP analysis and network traffic investigation
* Identifying covert communication channels and data exfiltration techniques
* HTTP traffic analysis using Wireshark
* Analyzing TCP connections, ports, and request patterns
* Understanding malware behavior and cookie-based data theft
* XOR encryption and Base64 encoding/decoding
* Extracting and reconstructing hidden data from packet captures
* Automating forensic tasks with Python and command line tools
* Using multiple tools together for network forensics

## Tools Used

* Wireshark
* tshark
* CyberChef
* Kali Linux
* VMware
* Linux
  * `scp`
  * `grep`
* Python

## Flag

🚩 **Flag:** `THM{*********************}`
