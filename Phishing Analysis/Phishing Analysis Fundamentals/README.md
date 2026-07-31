<p align="center">
<img src="https://assets.tryhackme.com/img/logo/tryhackme_logo_full.svg" width="150" alt="TryHackMe Logo">
</p>

# Phishing Analysis Fundamentals
|  Room Name | Phishing Analysis Fundamentals |
|----------|-------|
| Author | Dhruv Tripathi |
| Link | [Phishing Analysis Fundamentals](https://tryhackme.com/room/phishingemails1tryoe) |

# Room Information
```bash Type: Walkthrough
Difficulty: Easy
Tags: - 
Meta Tags: Walkthrough, Walk-through, Write-up, Writeup
Subscription type: Premium
Description:
Learn all the components that make up an email.
```
## Task 1

### I am ready to learn about phishing analysis!

- Answer: `No answer needed`

## Task 2

### Identify the domain used in the following email address: `hatsalesman@tryhatme.com`

- The domain comes after the `@` symbol and specifies the mail server responsible for receiving the message

- Answer: `tryhatme.com`

## Task 3

### Which protocol is responsible for sending an email from a client to a mail server?

- Email clients use `SMTP` (Simple Mail Transfer Protocol) to send mails to the outgoing mail server (SMTP server)

- Answer: `SMTP`

### Which service is used to look up the recipient domain’s mail server?

- The SMTP server queries `DNS` and it looks up the MX records for that domain which contain information about the incoming mail servers that accept mails for that domain

- Answer: `DNS`

### Bob wants to access his email from multiple devices, including his phone and laptop. Which protocol should he use?

- When `IMAP` is used, emails are stored on the mail server and synced across all devices

- Answer: `IMAP` 

## Task 4

### What is the full subject line of `email1.eml`?

- I clicked on `View` > `Message Source`
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/019dee1e-ec96-4128-8f04-a15d30d432cd" />
</p>

- After scrolling a bit, we can see the full subject line
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/74a08f0f-6379-40bb-bc9e-ec7d363c9642" />
</p>

- Answer: `Help protect your budget by protecting your home`

### View the message source of `email1.eml` using Thunderbird in your VM. What the IP address listed as the `X-Originating-Ip`?

- The originating IP address is listed here
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/c6e41edf-a32d-4f74-b676-e9dd60bcc36c" />
</p>

- Answer: `43.255.56.161`

## Task 5

### 

## Task 6
