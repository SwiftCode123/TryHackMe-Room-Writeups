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

### Open up the `email2.txt` file to view the source of an attachment. What is the `Content-Type` of the attachment?

- I opened up `email2.txt` and the `Content-Type` is listed at the top
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/1e5b37b4-c2be-4ced-9ba3-379309c0d740" />
</p>

- Answer: `application/pdf`

### What is the name of the attachment from the previous question?

- The name of the attachment was listed right next to the `Content-Type`
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/c050bb50-653f-4f64-a45b-c59a4d0d876a" />
</p>

- Answer: `zmqpalgh.pdf`

### Decode the base64 string using either a PDF converter (opens in new tab) or CyberChef (opens in new tab). What is the hidden flag value?

- I first copied the entire base64 string
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/1a7242db-a89e-4cbd-a299-aba448498037" />
</p>

- Then, I opened up CyberChef and converted it to find the hidden flag
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/50ba71ab-1ace-4d90-8fd5-390ef24bb192" />
</p>

- Answer: `THM{BENIGN_PDF_ATTACHMENT}`

## Task 6

### Which reputable organization is being spoofed in this phishing attempt?
- The first line in the email header tells us this is from "`Home Depot`"
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/bd21ac71-70f8-4ccf-9682-c718bd29d834" />
</p>

- Answer: `Home Depot`

### What is the sender's email address?
- The sender's email address is right next to the spoofed organization's name
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/a149fb8d-e680-492c-9a8a-ac1c888139af" />
</p>

- Answer: `support@teckbe.com`

### Inspect the email message source. What is the defanged (opens in new tab) `X-Originating-IP`?

- The `X-Originating-IP` is here
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/d60e98cc-92ad-4e02-9ced-6675cb42853c" />
</p>

- Pasting into CyberChef, we get the answer
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/84f5473a-790b-4951-a858-7325f9d10a58" />
</p>

- Answer: `103[.]234[.]236[.]83`

- Note: Defanged means to replace special characters such as `@` and `.` in URLs, domains, and email addresses to make them unclickable to prevent accidental clicks

### Continue analyzing the email message source. Which mail server generated the `Authentication-Results` header?

- The mail server is highlighted below
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/3a596c32-0214-4bed-9ab6-5bcfdebf7a5b" />
</p>
