<p align="center">
  <img src="https://assets.tryhackme.com/img/logo/tryhackme_logo_full.svg" width="150" alt="TryHackMe Logo">
</p>

# After Hours

| **Room Name** | After Hours |
| ----------------------------- | ----------------------------------------------------- |
| **Platform** | TryHackMe |
| **Event** | Hacker Holidays 2026 |
| **Points** | 90 |
| **Category** | 🕵️ Forensics |
| **Difficulty** | Medium |
| **Tags** | Windows Forensics, Persistence, Reverse Engineering, Malware Analysis |
| **Status** | ✅ Completed |

## Room Information

```
Type: CTF Challenge
Difficulty: Medium
Category: Forensics

Description:
Bar closed. Guests asleep. Something on the network just clocked in for a shift off the rotation.
```

## Challenge Overview

**Hacker Holidays: Day 12 — After Hours**

The After Hours challenge focuses on analyzing Windows system artifacts to uncover a hidden persistence mechanism. Traditional persistence locations such as Startup folders, Scheduled Tasks, and registry Run keys do not reveal the malicious activity, requiring deeper inspection of raw system data.

## Objectives

- Parse the provided system artifacts for hidden custom configuration data
- Locate the malicious class and extract its embedded payload
- Decode the payload and submit the recovered flag

## Solution

- I read the challenge description and I noticed this was a forensics challenge and might require digging deep into the system. As denoted by `the resort's back-office machines keep humming` there is something running maybe a process when it should not be? Furthermore, `Someone, or something, has been logging in during the small hours, well after the night-shift technician has gone home` might suggest it could be a user or malware accessing the computer. The idea of `something running` hints it could be malware I would assume so

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/9c1f8889-a2a3-4247-9010-414a0db1aed0" />
</p>

- Instead of downloading the task files, I just used what was already present in the Attackbox
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/77b9e175-7777-41e2-aad5-4490bfd251c9" />
</p>

- I went to the directory and noticed there was a zipped file with the extension `.7z`. This is a 7-Zip file and I extracted it via `7z x after-hours.7z`. I then went into the `after-hours-forensics-hh` directory and the into the `challenge_attachments` directory

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/ae737320-dea5-40a5-a3a9-a6cfc6109c2f" />
</p>

- Inside the `challenge_attachments` directory, I found three files which were `INDEX.BTR`, `MAPPING1.MAP`, and `OBJECTS.DATA`. I searched on Google and found out that these files make up the Windows Management Instrumentation (WMI) repository, which stores WMI objects and configuration data. Since this challenge hinted that the persistence mechanism was hidden somewhere most tools do not normally check, the presence of the WMI repository indicated that this was likely where the malicious persistence was stored

- I inspected `OBJECTS.DATA` first because it stores the actual WMI objects and their associated data. Since `OBJECTS.DATA` is a binary file, I used `strings` to extract readable text from it. I then piped the output to `grep -i "powershell"` because PowerShell is commonly associated with malicious WMI event consumers, particularly `CommandLineEventConsumer` objects that invoke `powershell.exe` to execute commands or payloads. This provided me a way to identify potentially suspicious WMI persistence mechanisms without manually reviewing thousands of legitimate strings
```
strings OBJECTS.DATA | grep -i "powershell"
```
- Here, we found a lot of Base64-encoded PowerShell payload beginning with `JABmAGkAbABlAC`
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/0dccb25d-6e97-4704-b87d-682cbd34493b" />
</p>

```
echo "JABmAGkAbABlAC..." | base64 -d
```
- Decoding the Base64 data and interpreting the resulting bytes gives the original command, allowing us to see what the PowerShell process was actually instructed to execute
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/b1e87964-1953-497e-9d1d-7a228341d6a7" />
</p>

- As we can see above, this confirmed the malicious class per se hinted at in the task where there is a custom WMI class named `Win32_HardwareTelemetry`, disguised as a legitimate looking telemetry class. The payload is stored in a property called `ConfigData`. The script retrieves this value, Base64 decodes it, deflate decompresses it, and then loads the resulting data directly into memory as a .NET assembly. This fileless execution technique explains why nothing was written to disk for tools like Autoruns to detect

- Since `Win32_HardwareTelemetry` contains the hidden payload, the next step was to find its `ConfigData` value inside `OBJECTS.DATA`. Searching for the class name helped locate the Base64 encoded data
```
strings OBJECTS.DATA | grep -A 10 "Win32_HardwareTelemetry"
```
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/1c646478-4fcf-4e10-96d6-249d39455dc8" />
</p>

- After locating the encoded data, the next step was to decode and decompress it. The Base64 encoded data was first decoded, followed by decompression using Python’s `zlib` library. The `-15` parameter was used because the data is compressed using raw Deflate rather than the standard `zlib` format. The resulting binary was then saved as a `.dll` file named `payload.dll`

```
python3 -c "import re, base64, zlib; data = open('OBJECTS.DATA', 'rb').read(); matches = re.findall(b'[A-Za-z0-9+/]{100,}=*', data); [open('payload.dll', 'wb').write(zlib.decompress(base64.b64decode(m), -15)) for m in matches if b'MZ' in zlib.decompress(base64.b64decode(m), -15)[:2]]" 2>/dev/null; ls -la payload.dll
```
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/2187aad0-6187-4a08-926f-c091de4d76de" />
</p>

- I then went back and went into the `tools` directory to read `instructions.txt` and followed the instructions as stated
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/b8c61937-5ac3-41cb-92ba-bf217d487d7c" />
</p>

- I then executed the tool and it opened it up. I went to `File` and located my `payload.dll`
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/327fbefa-5b04-4c8c-a5c0-263f70d81e4d" />
</p>

- I clicked on `payload` > `AfterHours` > `Program` > `Main(): void` and we can see base64 encoded text there
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/c4c5778a-d959-4ec5-a633-3021360a89bc" />
</p>

- If we go to CyberChef, we can see I successfully retrieved the flag
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/244b33cf-138d-423b-9619-6b2c03488d87" />
</p>
  
## Skills Learned

* Windows WMI repository forensics and analysis of `OBJECTS.DATA`, `INDEX.BTR`, and `MAPPING1.MAP`
* Identifying WMI-based persistence hidden outside common Startup, Scheduled Task, and Run key locations
* Analyzing suspicious custom WMI classes such as `Win32_HardwareTelemetry`
* Investigating WMI properties such as `ConfigData` for embedded malicious payloads
* Extracting suspicious PowerShell commands and Base64-encoded payloads from binary files
* Decoding Base64 data and decompressing raw Deflate payloads using Python and `zlib`
* Identifying fileless malware techniques involving in-memory .NET assembly execution
* Extracting malicious DLL payloads from Windows system artifacts
* Performing basic .NET assembly reverse engineering to trace the `AfterHours` execution flow
* Recognizing obfuscated PowerShell activity commonly used to execute malicious payloads
* Using CyberChef to decode and analyze the final encoded data


## Tools Used

* 7-Zip
* AttackBox
* `strings`
* `grep`
* Python
* `zlib`
* CyberChef
* .NET reverse engineering tool

## Flag

🚩 **Flag:** `THM{**********************}`
