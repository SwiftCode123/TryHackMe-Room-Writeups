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
## Task 9
