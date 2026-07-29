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

- As you can see I selected `sample1.exe` and clicked the button `Submit for Analysis`. Now, remembering from the `Pyramid Of Pain` room and general knowledge, we can block files from executing via their hashes. The hashes are at the bottom portion of the pyramid and so an attacker just change something in the file to create a different hash so its not really painful for the attacker

- I copied the hash and pasted it in the `Hash Blocklist` and successfully prevented the hash from running

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/75a55922-8f33-4472-8c0e-661f97b89348" />
</p>

- Here, I got the flag
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/c13c14eb-9ac0-4e37-af7e-ecc65913107e" />
</p>
