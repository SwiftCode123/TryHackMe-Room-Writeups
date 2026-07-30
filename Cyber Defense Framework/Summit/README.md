<p align="center">
<img src="https://assets.tryhackme.com/img/logo/tryhackme_logo_full.svg" width="150" alt="TryHackMe Logo">
</p>

# Summit
|  Room Name | Summit |
|----------|-------|
| Author | Dhruv Tripathi |
| Link | [Summit](https://tryhackme.com/room/summit) |

# Room Information
```bash Type: Walkthrough
Difficulty: Easy
Tags: - 
Meta Tags: CTF, CTF Challenge, Capture The Flag
Subscription type: Premium
Description:
Can you chase a simulated adversary up the Pyramid of Pain until they finally back down?
```
## Solution

- First thing I did was go to the website that TryHackMe had given me and read the email

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/102c6ab4-cf57-428f-bfcd-156703ad20e1" />
</p>

- I noticed it said to use the `Malware Sandbox` to scan the file, review the report and find a way to add a detection rule to block it and so that is what I did

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/082be722-3206-4b52-8933-aabbe1979116" />
</p>

- As you can see I selected `sample1.exe` and clicked the button `Submit for Analysis`. Now, remembering from the `Pyramid Of Pain` room and general knowledge, we can block files from executing via their hashes. The hashes are at the lowest tier in the pyramid and so an attacker can just change something in the file to create a completely different hash and so its not really frustrating for an attacker to evade this defense. We can not just rely on hashes

- I copied the hash and pasted it in the `Hash Blocklist` and successfully prevented the hash from running

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/75a55922-8f33-4472-8c0e-661f97b89348" />
</p>

- Here, I got the flag
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/c13c14eb-9ac0-4e37-af7e-ecc65913107e" />
</p>

- However, we notice in the email that the attacker now recompiled the malware and generated a new file hash. If we keep blocking the new hash, the attacker can just simply create another one and so on. We have to find another way to block the file from running. I went back to `Malware Sandbox` and uploaded the `sample2.exe` and it generated the report

- I scrolled down to the bottom and here we can see the `Network Activity` section. I noticed that it makes a `GET` request to that particular IP address with that port. Therefore, I would assume we can just block that network connection from happening in the first place. It is most likely making a connection to a C2 server somewhere

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/48af02ab-63e4-4aa3-a2d5-a5131ac65fcd" />
</p>

- I created the firewall rule. Here, `Egress` means exiting so a outbound connection and `Source IP` can be set to `Any` because any computer could be infected and we do not want any connections from any port and since its connecting to that particular IP address we saw in the `Network Activity`, I put the `Destination IP` as that IP address and `Action` was to `Deny`. As we can see when I saved the rule, it was successful in blocking the connection to the C2 server

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/0c582b4f-019c-4cbd-a81f-bfa30a5032df" />
</p>

- Here, I got the flag
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/5d8efeb3-befe-45ca-ba44-c8a35f434124" />
</p>

- However, remembering from the `Pyramid of Pain`, an attacker can just get a new public IP address as stated in the email above. It is not hard for the attacker to evade this defense but it was a bit harder for the attacker considering `IP addresses` are above `Hash Values` in the pyramid
