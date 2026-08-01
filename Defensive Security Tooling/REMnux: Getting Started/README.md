<p align="center">
<img src="https://assets.tryhackme.com/img/logo/tryhackme_logo_full.svg" width="150" alt="TryHackMe Logo">
</p>

# REMnux: Getting Started
|  Room Name | REMnux: Getting Started |
|----------|-------|
| Author | Dhruv Tripathi |
| Link | [REMnux: Getting Started](https://tryhackme.com/room/remnuxgettingstarted) |

# Room Information
```bash Type: Walkthrough
Difficulty: Easy
Tags: - 
Meta Tags: Walkthrough, Walk-through, Write-up, Writeup
Subscription type: Premium
Description:
Learn how you can use the tools inside the REMnux VM.
```
## Task 1

### Proceed with the next tasks to learn more!

- Answer: `No answer needed`

## Task 2

### I'm excited to learn more about the tools inside the REMnux VM!

- Answer: `No answer needed`

## Task 3

### What Python tool analyzes OLE2 files, commonly called Structured Storage or Compound File Binary Format?

- Answer: `oledump.py`

Note: This is a Python tool that analyzes `OLE2` files called Structured Storage or Compound File Binary Format. `OLE` stands for Object Linking and Embedding

### What tool parameter we used in this task allows you to select a particular data stream of the file we are using it with?

- Answer: `-s`

### During our analysis, we were able to decode a PowerShell script. What command is commonly used for downloading files from the internet?

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/7dd09e26-62e3-4b5b-9b77-154d577e67bc" />
</p>

- Answer: `Invoke-WebRequest`

### What file was being downloaded using the PowerShell script?

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/919c74e6-171d-420b-b416-30796e408be4" />
</p>

- Answer: `Doc-3737122pdf.exe`

### During our analysis of the PowerShell script, we noted that a file would be downloaded. Where will the file being downloaded be stored?

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/09b448a3-579a-416b-a2f1-e06a2aa6748e" />
</p>

- Answer: `$TempFile`

### Using the tool, scan another file named possible_malicious.docx located in the `/home/ubuntu/Desktop/tasks/agenttesla/` directory. How many data streams were presented for this file?

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/34c41e07-0edf-4222-b626-d1f7cdea5c80" />
</p>

- Answer: `16`

### Using the tool, scan another file named possible_malicious.docx located in the `/home/ubuntu/Desktop/tasks/agenttesla/` directory. At what data stream number does the tool indicate a macro present?

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/a8802c3b-6eb4-45a3-8ac9-897cadb7190a" />
</p>

Note: The `M` means there is a Macro in this data stream

- Answer: `8`

## Task 4

### Download and scan the file named flag.txt from the terminal using the command `sudo wget https://10.64.183.222/flag.txt --no-check-certificate`. What is the flag?

- First, we have to start `inetsim`

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/af302d15-b5aa-478f-b1ec-4fc6de35cc5f" />
</p>

- Then, we have to download the file via the command given to us

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/ab401435-c8af-4cba-b201-b8421ea776d2" />
</p>

- Now, we just `cat` the flag

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/f6297644-18ad-4169-87f9-66da9d65ce93" />
</p>

- Answer: `Tryhackme{remnux_edition}`

### After stopping the inetsim, read the generated report. Based on the report, what URL Method was used to get the file flag.txt?

- We can see the method used below

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/90eed7ad-2e8b-44e4-aa1d-c3207a9d2b82" />
</p>

- Answer: `GET`

## Task 5
### What plugin lists processes in a tree based on their parent process ID?

- Answer: `PsTree`

Note: `windows.pstree.PsTree` lists all processes as a hierarchical family tree. It tells us which program started another program (parent-child relationship). For example, this can tell us if a child or parent process is suspicious because certain processes only start other certain processes

### What plugin is used to list all currently active processes in the machine?

- Answer: `PsList`

Note: `windows.pslist.PsList` shows all the current active running processes

### What Linux utility tool can extract the ASCII, 16-bit little-endian, and 16-bit big-endian strings?

- Answer: `strings`

### By running vol3 with the Malfind parameter, what is the first (1st) process identified suspected of having an injected code?

- If we open the text file, we can see the first process listed and under the `Protection Disasm`, it says `PAGE_EXECUTE_READWRITE` which is a little suspicious because this means a piece of memory can read from, write to, and execute code stored in that specific chunk of memory

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/eebbbf79-3568-4a7a-a914-86f1aafa4086" />
</p>

- Answer: `csrss.exe`

### Continuing from the previous question (Question 4), what is the second (2nd) process identified suspected of having an injected code?

- The second process also with `PAGE_EXECUTE_READWRITE` under the `Protection Disasm` was listed below

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/04edd388-d997-4df5-b84e-7592b1991bc7" />
</p>

- Answer: `winlogon.exe`

## By running vol3 with the DllList parameter, what is the file path or directory of the binary `@WanaDecryptor@.exe`?

- I used `grep` to search for the specific file path of the binary as seen below

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/565346fa-d914-4710-96e1-762ecd26f410" />
</p>

- Answer: `C:\Intel\ivecuqmanpnirkt615`

## Skills Learned

* Analyzed malicious Microsoft Office documents using oledump.py
* Identified OLE2 data streams and detected embedded VBA macros
* Decoded obfuscated PowerShell scripts to identify malware behavior
* Determined downloaded payloads, execution flow, and file storage locations
* Examined additional malicious documents to locate macro-containing streams
* Configured and used INetSim to safely emulate internet services during malware analysis
* Captured and reviewed simulated network activity, including HTTP request methods
* Utilized Volatility 3 plugins (PsTree, PsList, Malfind, and DllList) for memory analysis
* Identified suspicious processes exhibiting signs of code injection
* Located malware binaries within memory artifacts using Volatility and Linux command-line utilities
* Applied the strings utility to extract readable data from binary files and memory artifacts

## Conclusion

The REMnux: Getting Started room provided a practical introduction to the REMnux malware analysis environment and its core forensic tools. Through hands-on analysis of malicious Office documents, PowerShell scripts, simulated network traffic, and Windows memory dumps, I gained experience with malware triage, network emulation, and memory forensics. Overall, the room reinforced essential malware analysis techniques while demonstrating how REMnux can be used to safely investigate suspicious files and system artifacts
