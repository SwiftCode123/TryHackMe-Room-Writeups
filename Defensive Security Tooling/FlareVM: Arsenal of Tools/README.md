<p align="center">
<img src="https://assets.tryhackme.com/img/logo/tryhackme_logo_full.svg" width="150" alt="TryHackMe Logo">
</p>

# FlareVM: Arsenal of Tools
|  Room Name | FlareVM: Arsenal of Tools |
|----------|-------|
| Author | Dhruv Tripathi |
| Link | [FlareVM: Arsenal of Tools](https://tryhackme.com/room/flarevmarsenaloftools) |

# Room Information
```bash Type: Walkthrough
Difficulty: Easy
Tags: - 
Meta Tags: Walkthrough, Walk-through, Write-up, Writeup
Subscription type: Premium
Description:
Learn the arsenal of investigative tools in FlareVM.
```
## Task 1

### I'm ready to learn more about FlareVM!

- Answer: `No answer needed`

## Task 2

### Which tool is an Open-source debugger for binaries in x64 and x32 formats?

- Answer: `x64dbg`

### What tool is designed to analyze and edit Portable Executable (PE) files?

- Answer: `CFF Explorer`

### Which tool is considered a sophisticated memory editor and process watcher?

- Answer: `Process Hacker`

### Which tool is used for Disc image acquisition and analysis for forensic use?

- Answer: `FTK Imager`

### What tool can be used to view and edit a binary file?

- Answer: `HxD`

## Task 3
- ### Which tool was formerly known as FireEye Labs Obfuscated String Solver?

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/fe8dcfb5-8922-4696-ab52-eec70d160327" />
</p>

Note: `FLOSS` stands for FireEye Labs Obfuscated String Solver

- Answer: `FLOSS`

### Which tool offers in-depth insights into the active processes running on your computer?

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/3553e60a-0d85-4075-b2aa-dda80fb52a5f" />
</p>

Note: `Process Explorer` is a windows tool that gives you detailed info on the active processes on the computer. You can see running processes, find PIDs, which user account started a process, identify a process's parent, and find out which process is using a file/folder

- Answer: `Process Explorer`

### By using the Process Explorer (procexp) tool, under what process can we find smss.exe?


- If we open `Process Explorer`, we can the see parent process of `smss.exe`

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/e1990f83-0fd2-4553-b5e0-5496c2814c92" />
</p>

- Answer: `System`

### Which powerful Windows tool is designed to help you record issues with your system's apps?

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/7bf39c6e-2d27-49ff-b1b2-d7e747dd11f0" />
</p>

Note: This is a windows tool that is helpful in showing what programs are doing on the computer in real time. Monitors things such as files that programs open, read, write, changes to windows registry, programs and processes starting/stopping and threat/process activity. It is used for trouble shooting software problems, malware analysis and digital forensics

- Answer: `Procmon`

### Which tool can be used for Static analysis or studying executable file properties without running the files?

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/7025064a-965e-4e88-98fa-d317ca9a2e0b" />
</p>

- Answer: `PEStudio`

## Using the tool PEStudio to open the file cryptominer.bin in the Desktop\Sample folder, what is the sha256 value of the file?

- First, I opened up the file `cryptominer.bin`

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/2a82bb90-a6a6-48e2-9959-56e5db522a99" />
</p>

- Now, we can take a look at the `sha256` value of this file

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/7bc8059e-5c6f-4153-b937-c56f67baa09a" />
</p>

- Answer: `E9627EBAAC562067759681DCEBA8DDE8D83B1D813AF8181948C549E342F67C0E`

### Using the tool PEStudio to open the file cryptominer.bin in the Desktop\Sample folder, how many functions does it have?

- We can take a look at the number of functions below

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/7f523007-cec5-42a7-92e7-3aef708bbb27" />
</p>

- Answer: `102`

### What tool can generate file hashes for integrity verification, authenticate the source of system files, and validate their validity?

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/1637c8e3-d18b-4aea-b586-c4796d3d338b" />
</p>

- Answer: `CFF Explorer`

### Using the tool CFF Explorer to open the file possible_medusa.txt in the Desktop\Sample folder, what is the MD5 of the file?

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/f7754a04-7dea-42ad-8d9a-5aadbbfb8105" />
</p>

- Answer: `646698572AFBBF24F50EC5681FEB2DB7`

### Use the CFF Explorer tool to open the file possible_medusa.txt in the Desktop\Sample folder. Then, go to the DOS Header Section. What is the e_magic value of the file?

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/ed641770-8ba8-4e5a-8245-2320e624158f" />
</p>

Note: The `e_magic` field is the first 2 bytes of the DOS header and normally contains the value `MZ` (this is the Magic Number). Note that it is translated from `0x5A4D` to `MZ` in ASCII. It acts as a signature that tells the operating system the file has a valid DOS header. If this signature is present, the operating system continues reading the rest of the executable file

- Answer: `5A4D`

## Task 5

### Using PEStudio, open the file windows.exe. What is the entropy value of the file windows.exe?

- When opening `windows.exe`, we can see the entropy level below

Note: A lower entropy means more readable code and higher means it is packed and encrypted making it harder to read

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/cee78e6c-2253-41d8-9579-6e3c1d0a4781" />
</p>

- Answer: `7.999`

### Using PEStudio, open the file windows.exe, then go to manifest (administrator section). What is the value under requestedExecutionLevel?

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/e068f02f-870c-41bb-a264-6daa7ab35db2" />
</p>

Note: `requestedExecutionLevel` is a setting in a Windows program's manifest that tells Windows what level of permissions the program wants when it starts

- Answer: `requireAdministrator`

### Which function allows the process to use the operating system's shell to execute other processes?

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/b73d9b81-bd4c-41b1-9788-75db2e10224a" />
</p>

- Answer: `set_UseShellExecute`

### Which API starts with R and indicates that the executable uses cryptographic functions?

- First, I ran `FLOSS.exe .\windows.exe > windows.exe` so I can get every single string (plaintext and hidden) from this executable

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/348c82ed-bd83-4319-8b0d-67ea177ace49" />
</p>

- BOOM!, we have found it

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/8196f5e9-f836-41df-a4b0-37aaa69efadb" />
</p>

Note: `RijndaelManaged` is a class in the Microsoft `.NET` framework that provides symmetric encryption and decryption using the Rijndael algorithm

- Answer: `RijndaelManaged`

### What is the Imphash of cobaltstrike.exe?

- I opened up `cobaltstrike.exe` in PEStudio to see the Imphash. Note that Imphash stands for import hash

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/1759be3e-0247-4a86-b25c-64725880c1e8" />
</p>

- Answer: `92EEF189FB188C541CBD83AC8BA4ACF5`

### What is the defanged IP address to which the process cobaltstrike.exe is connecting?

- First, I ran `cobaltstrike.exe` and then opened up Process Explorer which is a windows tool that gives you detailed info on the active processes on the computer

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/bc7376dc-6c78-418b-a2c9-66807c843b83" />
</p>

- Then, I right-clicked on `cobaltstrike.exe` > `Properties` and we can see the remote IP address it is connecting to

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/ab822ea1-1d25-49df-bb56-592211d6424d" />
</p>

Note: TryHackMe had you type the IP address with `[.]` in betweens

- Answer: `47[.]120[.]46[.]210`

### What is the destination port number used by cobaltstrike.exe when connecting to its C2 IP Address?

- The destination port is right besides the IP address. You can uncheck the box `Resolve IP addresses` to see the destination port

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/3966a671-4c21-4329-ac45-0433fae5ea24" />
</p>

- Answer: `81`

### During our analysis, we found a process called cobaltstrike.exe. What is the parent process of cobaltstrike.exe? 

- The child process (`cobaltstrike.exe`) was under `explorer.exe`

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/bc7376dc-6c78-418b-a2c9-66807c843b83" />
</p>

- Answer: `explorer.exe`

## Skills Learned

* Identified the purpose and practical use cases of core FlareVM tools, including x64dbg, CFF Explorer, Process Hacker, FTK Imager, HxD, FLOSS, Process Explorer, Procmon, and PEStudio
* Performed static analysis of Portable Executable (PE) files with PEStudio and CFF Explorer without executing the binaries
* Examined PE metadata including SHA-256 hashes, Imphashes, entropy, imported functions, manifests, DOS headers, and requested execution levels
* Generated and verified MD5, SHA-256, and Imphash values to validate file integrity and support malware identification
* Used FLOSS to recover plaintext, hidden, and obfuscated strings, including identifying cryptographic functionality such as the `RijndaelManaged` API
* Investigated running processes with Process Explorer by identifying parent-child process relationships, process properties, active network connections, and parent executables
* Monitored real-time file, registry, process, and thread activity using Process Monitor (Procmon) to observe system behavior
* Identified executable behaviors through imported functions and APIs, including shell execution (`UseShellExecute`) and cryptographic functionality
* Determined command-and-control (C2) communication details by identifying the remote IP address and destination port used by a suspicious executable
* Applied foundational static and basic dynamic malware analysis techniques to investigate suspicious Windows executables using the FlareVM toolkit

## Conclusion
This room introduced the core analysis tools available in FlareVM and demonstrated how they work together during malware investigations. Static analysis tools such as PEStudio, CFF Explorer, and FLOSS were used to inspect executables without running them, while dynamic analysis tools like Process Explorer and Procmon monitored process behavior and network activity. Overall, the room provides a strong foundation in using FlareVM's toolkit for Windows malware analysis and digital forensics
