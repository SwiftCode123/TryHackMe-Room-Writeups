<p align="center">
  <img src="https://assets.tryhackme.com/img/logo/tryhackme_logo_full.svg" width="150" alt="TryHackMe Logo">
</p>

# Overheard at Breakfast

| **Room Name** | Overheard at Breakfast |
|---------------|------------------------|
| **Platform** | TryHackMe |
| **Event** | Hacker Holidays 2026 |
| **Points** | 60 |
| **Category** | 🕵️ OSINT |
| **Difficulty** | Easy |
| **Tags** | OSINT, Social Media, Hashing, Digital Footprinting |
| **Status** | ✅ Completed |

## Room Information
```
Type: CTF Challenge  
Difficulty: Easy  
Category: OSINT

Description:
Two strangers. One conversation. One profile they never meant to reveal.
```

## Challenge Overview

**Hacker Holidays: Day 6 — Overheard at Breakfast**

The challenge revolves around Open Source Intelligence (OSINT). A screenshot of a conversation contains multiple identifying details that appear insignificant individually but, when combined, uniquely identify a person's online presence.

The objective is to investigate the conversation, identify the important clues, locate the hidden account, and retrieve the flag.

## Objectives

- Analyze the provided conversation for identifying details
- Extract the relevant clues
- Locate the hidden account
- Submit the flag

## Solution

- I downloaded the task files and opened up the screenshot to analyze. The biggest clue here is that some service that `Lambo!` used that started with a `G`. I also noted down his email `lambobytelotushotel@gmail.com`
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/b2925c1a-1604-41b6-8e4f-a68f830ec582" />
</p>

- I searched on Google for this service and it strongly suggests that the service is `Gravatar`
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/7bd2ec49-bf1b-44ef-871f-4d6da15813e8" />
</p>

- Now that I know to use this service, I needed a way to find his hidden account although I did not not how so I searched on Google for ways. Looking at `Method 1`, we do have the email address and so I clicked on that link
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/1c7aacb5-d44a-4878-ab63-346e02842ed3" />
</p>

- I inputted the email and I got the `Profile URL`, `Email Hash`, and `Avatar URL`
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/6fd7cd3e-9dd4-4f8e-aaa7-0bf93638a065" />
</p>

- I visited the `Profile URL` and ended up getting a base64 encoded string
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/76d185ae-3729-4301-8224-c5cdb9db0f28" />
</p>

- I inputted the string into CyberChef and retrieved the flag
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/dd3f9e42-5239-4164-aecd-ad01b1ec586a" />
</p>

## Skills Learned

- OSINT investigation using publicly available information
- Extracting valuable clues from screenshots and conversations
- Identifying online services from contextual hints
- Using an email address to discover associated public profiles
- Understanding Gravatar email hashes and profile lookups
- Identifying and decoding Base64-encoded data
- Correlating multiple OSINT artifacts to locate hidden information

## Tools Used
- Google
- Gravatar
- CyberChef
- Web browser

## Flag
🚩 Flag: `THM{******************************}`
