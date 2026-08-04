<p align="center">
  <img src="https://assets.tryhackme.com/img/logo/tryhackme_logo_full.svg" width="150" alt="TryHackMe Logo">
</p>

# Phishing Analysis Tools
|  Room Name | Phishing Analysis Tools |
|----------|-------|
| Author | Dhruv Tripathi |
| Link | [Phishing Analysis Tools](https://tryhackme.com/room/phishingemails3tryoe) |

# Room Information
```bash Type: Walkthrough
Difficulty: Easy
Tags: - 
Meta Tags: Walkthrough, Walk-through, Write-up, Writeup
Subscription type: Premium
Description:
Learn the tools used to aid an analyst to investigate suspicious emails.
```
## Task 1

### I am ready to learn about phishing analysis!

- Answer: `No answer needed`

## Task 2

### I understand the key indicators to look for when analyzing an email.

- Answer: `No answer needed`

## Task 3

### According to the Messageheader analysis shown in this task, what is the SPF result for the email?

- We can see the `SPF` result in green
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/de6522b4-3646-4046-a4e3-ccdffcb41539" />
</p>

- Answer: `pass`

## Task 4

### What command can you use in a Linux environment to obtain the SHA256 hash value of an attachment?

- Answer: `sha256sum`

## Task 5

### I understand the available sandbox environments for safely analyzing files and URLs.

- Answer: `No answer needed`

## Task 6

### According to the VirusTotal analysis from above, which vendor categorized the URLs as phishing?

- There was only one vendor who categorized it as phishing
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/a2adc398-452a-42ca-9892-d1cc5f6a0ac7" />
</p>

- Answer: `SafeToOpen`

## Task 7

### What reputable brand is this email tailored to impersonate?

- We can see this email is tailored to impersonate one of most famous video streaming services
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/83f30b1c-8c1c-445d-8e61-b2783f3f48fb" />
</p>

- Answer: `Netflix`

### Based on the email headers, who is the intended recipient of this message?

- The recipient is right below the display name `Netflix`
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/1abe8660-bb94-4304-9dd2-5214e15abaac" />
</p>

- Answer: `redacted@yahoo.com`

### In Thunderbird mail use `View` → `Message Source`. What is the `Received: from` IP address?

- The originating IP is listed at the top
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/bbe66f02-b4fd-4d7a-83f5-9cad34dc3bce" />
</p>

- Answer: `10.197.37.234`

### Check out the `Return-Path` field in the message source. What would you consider a domain of interest based on this field?

- Looking at the `Return-Path` field, the domain is after the `@` symbol
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/8644d89f-418d-400e-becb-155997d585e5" />
</p>

- Answer: `etekno.xyz`

### Investigate the UPDATE ACCOUNT NOW button. What is the shortened URL?

- I first hovered over the URL and right-clicked and selected `Copy Link Location`
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/903c122f-b03e-46cf-a8dd-5083275e9610" />
</p>

- I pasted it into `Notepad` and we can see the shortened URL
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/5fd666f9-b8f9-45b0-aa6e-78bcbd34f905" />
</p>

- Answer: `https://t.co/yuxfZm8KPg?amp=1`

## Task 8

### How does `ANYRUN` classify this suspected phishing email?

- In the right hand corner, we can see what `ANY.RUN` classifies this phishing email as
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/76a7fb74-0a6b-4ad9-9b43-093084510331" />
</p>

- Answer: `Suspicious activity`

### What is the name of the PDF attachment?

- The name is listed right below the classification
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/7f2d20e6-e58f-40b3-834b-ad381c0cc700" />
</p>

- Answer: `Payment-updateid.pdf`

### Investigate the email attachment. What is the SHA256 hash of the `PDF` file?

- I clicked on `Payment-updateid.pdf` and we can see below in the third row what the SHA256 hash is of this file

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/823daf65-12c4-4471-9abe-9060e59ab927" />
</p>

- Answer: `cc6f1a04b10bcb168aeec8d870b97bd7c20fc161e8310b5bce1af8ed420e2c24`

### Check out the `ANYRUN` text report on the phishing email. Which IP address associated with the process `AcroRd32.exe` is flagged as malicious?

- We can open up the `Text report` by clicking on `Results`

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/9f603bdb-694c-43be-9906-fb274e358628" />
</p>

- I scrolled down to the `Connections` section and there is only one IP address associated with `AcroRd32.exe` that is flagged as malicious

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/e35f9da6-fc52-4712-b3a6-8e615dda846d" />
</p>

- Answer: `2.16.107.24`

### Continue investigating the text report. Which Windows process is classed as `Potentially Bad Traffic`?

- I scrolled down to `Threats` and there is only one process that is marked as `Potentially Bad Traffic`

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/91871253-fda5-41e6-a516-e208581ceb02" />
</p>

- Answer: `svchost.exe`

## Task 9

### How does ANYRUN classify the .xlsx attachment?

- Like the previous task, the classification is on the top right hand corner
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/e5156577-05b9-4def-a58a-d3b1f8f6b090" />
</p>

- Answer `Malicious activity`

### What is the file name of the Excel attachment?

- The name is listed right below the classification
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/c4d19e9e-8578-43a7-be7d-62da072ca782" />
</p>

- Answer: `CBJ200620039539.xlsx`

### Investigate the Excel attachment. What is the SHA256 hash value?

- We can see the SHA256 hash value below
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/0a02b12f-cb21-48b6-8d8d-da222b404615" />
</p>

- Answer: `5f94a66e0ce78d17afc2dd27fc17b44b3ffc13ac5f42d3ad6a5dcfb36715f3eb`

### Check out the `ANYRUN` text report. What IP address is associated with the malicious domain `biz9holdings.com`?

- Scrolling down to the `DNS requests` sections, we can see there is one other domain below `biz9holdings.com` that is also flagged as malicious

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/9128438a-5515-4fc1-9e18-c16986ccde2d" />
</p>

- Answer: `findresults.site`

### What vulnerability does this malicious attachment attempt to exploit?

- The vulnerability is located here
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/9d69350a-b874-4f8a-acc2-44e19801d965" />
</p>

- Answer: `CVE-2017-11882`
