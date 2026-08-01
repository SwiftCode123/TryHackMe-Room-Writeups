<p align="center">
<img src="https://assets.tryhackme.com/img/logo/tryhackme_logo_full.svg" width="150" alt="TryHackMe Logo">
</p>

# CAPA: The Basics
|  Room Name | CAPA: The Basics |
|----------|-------|
| Author | Dhruv Tripathi |
| Link | [CAPA: The Basics](https://tryhackme.com/room/capabasics) |

# Room Information
```bash Type: Walkthrough
Difficulty: Easy
Tags: - 
Meta Tags: Walkthrough, Walk-through, Write-up, Writeup
Subscription type: Premium
Description:
Learn to use CAPA to identify malicious capabilities.
```
## Task 1
### I'm excited to learn more about CAPA!

- Answer: `No answer needed`

## Task 2
### What command-line option would you use if you need to check what other parameters you can use with the tool? Use the shortest format.

- Answer: `-h`

### What command-line options are used to find detailed information on the malware's capabilities? Use the shortest format.

- Answer: `-v`

Note: `-v` enables verbose output or in other words, gives more details in the output

### What command-line options do you use to find very verbose information about the malware's capabilities? Use the shortest format.

- Answer: `-vv`

### What PowerShell command will you use to read the content of a file?

- Answer: `Get-Content`

### What is the sha256 of cryptbot.bin?

- Let's take a look at the output that was generated via the command `cat .\cryptobot.txt`. We can see the `sha256` value

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/0008c992-f5b8-4950-af50-84be37fae25f" />
</p>

- Answer: `ae7bc6b6f6ecb206a7b957e4bb86e0d11845c5b2d9f7a00a482bef63b567ce4c`

### What is the Technique Identifier of Obfuscated Files or Information?
- Looking in the `ATT&CK Technique` section, we can see the answer

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/cc89c9d3-1163-42eb-8fcb-a785c4e40735" />
</p>

- Answer: `T1027`

### What is the Sub-Technique Identifier of Obfuscated Files or Information::Indicator Removal from Tools?

- Let's look at the line just below it

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/44a0152d-d878-4176-9074-3c06cf6b8237" />
</p>

- Answer: `T1027.005`

### When CAPA tags a file with this MAEC value, it indicates that it demonstrates behaviour similar to, but not limited to, Activating persistence mechanisms?

- Answer: `Launcher`

Note: MAEC values are a standard way of describing and sharing information about malware. There are two MAEC values: `Launcher` or `Downloader`

### When CAPA tags a file with this MAEC value, it indicates that the file demonstrates behaviour similar to, but not limited to, Fetching additional payloads or resources from the internet?

- Answer: `Downloader`

## Task 4
### What serves as a catalogue of malware objectives and behaviours?
- We know this to be as MBC
- Answer: `Malware Behavior Catalogue`

###  Which field is based on ATT&CK tactics in the context of malware behaviour?
- Answer: `Objective`

Note: In the context of malware, the objectives and behaviors are often mapped to the tactics and techniques of the MITRE ATT&CK framework

### What is the Identifier of "Create Process" micro-behavior?

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/67727141-f660-4154-95ac-6e528f3e6359" />
</p>

- Answer: `C0017`

### What is the behaviour with an Identifier of B0009?

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/2b96b452-28b4-41ba-9f08-8d2faa8ec55e" />
</p>

- Answer: `Lab Machine Detection`

### Malware can be used to obfuscate data using base64 and XOR. What is the related micro-behavior for this?

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/2c139c68-c137-4463-a9d5-a4edb97db62c" />
</p>

- Answer: `Encode Data`

Note: `base64` and `XOR` are the specific methods under the `Encode Data` micro-behavior

### Which micro-behavior refers to "Malware is capable of initiating HTTP communications"?

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/87c19c15-211d-4a96-bcea-53a167569a9b" />
</p>

- Answer: `HTTP Communication`

## Task 5
### Which top-level Namespace contains a set of rules specifically designed to detect behaviours, including obfuscation, packing, and anti-debugging techniques exhibited by malware to evade analysis?

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/4e0f6c04-4a85-4088-954c-1a15ff0cc1bd" />
</p>

- Answer: `anti-analysis`

Note: TLNs are the largest category that groups similar kinds of malware behavior. A namespace is a subcategory inside a TLN. Then, there are rule YAML files which are used for detections. Namespaces are a hierarchical way to organizing CAPA rules into categories based on the type of behavior they detect

### Which namespace contains rules to detect lab machine (VM) environments? Note that this is not the TLN or Top-Level Namespace.

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/f9c692ce-073a-444c-9967-60f9c54fd399" />
</p>

- Answer: `anti-vm/vm-detection`

### Which Top-Level Namespace contains rules related to behaviours associated with maintaining access or persistence within a compromised system? This namespace is focused on understanding how malware can establish and maintain a presence within a compromised environment, allowing it to persist and carry out malicious activities over an extended period.

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/c801cc87-b21e-4130-8c1e-0a9f98d1baa2" />
</p>

- Answer: `persistence`

### Which namespace addresses techniques such as String Encryption, Code Obfuscation, Packing, and Anti-Debugging Tricks, which conceal or obscure the true purpose of the code?

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/c3f94011-4cd3-44e6-8a69-9af91536819c" />
</p>

- Answer: `obfuscation`

### Which Top-Level Namespace Is a staging ground for rules that are not quite polished?

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/8fcbfa85-eec0-45e0-b052-6993913e34d3" />
</p>

- Answer: `nursery`

### Proceed to the next task for the 2nd part of the discussion!

- Answer: `No answer needed`

## Task 6
### What rule yaml file was matched if the Capability or rule name is check HTTP status code?

- Answer: `check-http-status-code.yml`

Note: The Capability is the rule name but without the dashes

### What is the name of the Capability if the rule YAML file is reference-anti-vm-strings.yml?

- Answer: `reference anti-VM strings`

### Which TLN or Top-Level Namespace includes the Capability or rule name run PowerShell expression?

- We can see in the output where `run Powershell expression` is located

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/1a10ad45-03d3-4312-b993-bbd763cfe54d" />
</p>

- Answer: `load-code`

### Check the conditions inside the check-for-windows-sandbox-via-registry.yml rule file from this link (opens in new tab). What is the value of the API that ends in Ex is it looking for?

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/1e3e8f69-433d-4316-882a-9bb50a339138" />
</p>

- Answer: `RegOpenKeyEx`

## Task 7
### Which parameter allows you to output the result of CAPA into a .json file?
- Answer: `-j`

### What tool allows you to interactively explore CAPA results in your web browser?
- Answer: `CAPA Web Explorer`

### Which feature of this CAPA Web Explorer allows you to filter options or results?
- Answer: `Global Search Box`

## Summary
* Learned the fundamentals of CAPA, a malware capability analysis tool that identifies behaviors without requiring full reverse engineering
* Explored common CAPA command-line options, including `-h` for help, `-v` for verbose output, `-vv` for very verbose output, and `-j` for exporting results as JSON
* Analyzed CAPA output to interpret metadata such as SHA-256 hashes, MITRE ATT&CK techniques, and MAEC malware classifications
* Examined how CAPA maps malware behaviors to the Malware Behavior Catalogue (MBC), including objectives, behaviors, and micro-behaviors such as process creation, HTTP communication, and data encoding
* Learned how CAPA organizes detection rules using Top-Level Namespaces (TLNs and namespaces to categorize behaviors like persistence, anti-analysis, obfuscation, and virtual machine detection)
* Understood the relationship between Capabilities and their corresponding YAML rule files making it easier to trace how specific detections are implemented
* Discovered additional features such as exporting results in JSON format and using CAPA Web Explorer to interactively analyze findings in a web browser

# Conclusion
This room provided a solid introduction to CAPA and how it can be used to quickly identify the capabilities of a malware sample. Rather than focusing on reverse engineering every instruction, CAPA highlights higher-level behaviors by mapping them to frameworks like MITRE ATT&CK, MAEC, and MBC, making analysis more efficient and standardized. Overall, this room serves as a great foundation in malware analysis, threat hunting, or defensive security tooling
