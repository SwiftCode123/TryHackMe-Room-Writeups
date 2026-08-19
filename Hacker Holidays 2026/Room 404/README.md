<p align="center">
  <img src="https://assets.tryhackme.com/img/logo/tryhackme_logo_full.svg" width="150" alt="TryHackMe Logo">
</p>

# Room 404

| Room Name | Room 404 |
|-----------|----------|
| Platform | TryHackMe |
| Event | Hacker Holidays 2026 |
| Points | 30 |
| Category | 🌐 Web |
| Difficulty | Very Easy |
| Tags | Web, Directory Enumeration, Source Code Disclosure, HTTP, Recon |
| Status | ✅ Completed |

## Room Information

```bash
Type: CTF Challenge
Difficulty: Very Easy
Category: Web

Description:
He booked the quiet room. It's not on the floor plan, not in the brochure, not on any door. But port 8080 is wide open, and the rooms it never lists are the ones worth finding.
```

## Challenge Overview

**Hacker Holidays: Day 2 — Room 404**

The Byte Lotus Hotel's guest platform has gone live in a hurry, leaving behind more than just the public website. An exposed service running on port 8080 reveals directories and files never intended for guests to discover.

Using basic web enumeration techniques, you'll uncover hidden resources, access the exposed source code, and retrieve the flag hidden within the application.

## Objectives

- Dump the exposed source code.
- Find the flag.

## Solution
- From the challenge description, I noticed it said `the rooms it never lists are the ones worth finding` which makes me think there are some hidden files/folders that we may need to find and that means using Gobuster for this challenge

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/cd8e3cec-126a-4d00-bfee-7bec23378c9c" />
</p>

- Also, we can see the category at the bottom which confirms my thoughts
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/5c48c6a4-544d-4a43-95df-f2a0b4d067c2" />
</p>

- Running the command, I get two folders which are `/.git` and `/.git/HEAD`
  - Note that we use the `-x` option to tell Gobuster to also check for common file extensions, since sensitive files are often left behind with extensions such as `.git`, `.bak`, `.zip`, `.tar.gz`, or `.env` 
```
gobuster dir -u "http://10.67.191.9:8080" -w /usr/share/wordlists/dirb/common.txt -x git,bak,zip,tar.gz,env
```

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/8c118cc3-c2d1-4356-b3b0-fd080accfc92" />
</p>

- We can reconstruct the Git repo from the `/.git` folder using a tool called `git-dumper` but first I had to install it
```
pip install git-dumper
```

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/b69bc369-7010-43b5-86ad-325385c2648a" />
</p>

- I ran the command below and then saved the output into a folder called `extracted_folder`
```
git-dumper http://10.67.191.9:8080/.git ./extracted_project
```

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/8578f882-9457-4339-b3bc-87af24b85c7e" />
</p>

- Then I checked the commit logs and found the flag
```
git log -p
```
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/c6166846-fb93-424a-b8f0-56728c523eca" />
</p>

## Skills Learned
- Basic web reconnaissance
- Directory enumeration with Gobuster
- Identifying exposed `.git` directories
- Reconstructing an exposed Git repository
- Viewing Git commit history to find sensitive information

## Tools Used
- Web Browser
- Gobuster
- Git-Dumper
- Git (`git log -p`)

## Flag
🚩 Flag: THM{*********************}
