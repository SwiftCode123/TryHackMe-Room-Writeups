<p align="center">
<img src="https://assets.tryhackme.com/img/logo/tryhackme_logo_full.svg" width="150" alt="TryHackMe Logo">
</p>

# Snapped Phish-ing Line
|  Room Name | Snapped Phish-ing Line |
|----------|-------|
| Author | Dhruv Tripathi |
| Link | [Snapped Phish-ing Line](https://tryhackme.com/room/snappedphishingline) |

# Room Information
```bash Type: Walkthrough
Difficulty: Easy
Tags: - 
Meta Tags: CTF, CTF Challenge, Capture The Flag
Subscription type: Premium
Description:
Apply learned skills to probe malicious emails and URLs, exposing a vast phishing campaign.
```

## Scenario

As a member of the IT department at SwiftSpend Financial, you are responsible for assisting employees with technical concerns. What initially appeared to be a routine day quickly escalated when multiple employees across different departments reported receiving a suspicious email. Several users noted unusual characteristics in the message, and unfortunately, some had already submitted their credentials and were no longer able to access their accounts. With the potential for a wider compromise, the incident has been escalated for investigation. Your task is to analyze the available evidence, determine the scope of the attack, and uncover how the adversary operated.

## Objective
- Analyze the provided email samples to identify key artifacts
- Investigate phishing URLs to understand redirection
- Retrieve and examine the phishing kit used in the attack
- Use CTI tools to gather intelligence on the adversary
- Analyze the phishing kit to uncover additional indicators

## Solution
### Begin reviewing the emails in the `phish-emails` folder on your desktop. Which individual received the email regarding a Quote for Services Rendered?

- I opened the email `Quotes for Services Rendered` in the folder
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/fdba92d0-38ac-472f-9e7c-5aa2f8a37cf8" />
</p>

- We can see who it was addressed to next to the `To` in the email header
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/31f2443e-030a-471b-b5dc-c4196acaaef0" />
</p>

- Answer: `William McClean`

### What email address was used by the adversary to send the phishing emails?

- The email address is next to the `From` in the email header next to the display name
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/62986ed4-26d8-4b0f-a9f5-ef620a99e639" />
</p>

- Answer: `Accounts.Payable@groupmarketingonline.icu`

### Investigate the attachment in the email addressed to Zoe Duncan. What is the root domain of the redirection URL found within the file?
- This was the file addressed to `Zoe Duncan`
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/22dc078e-8335-4234-8d2b-42a281709e17" />
</p>

- I saved the attachment by clicking the `Save` button in the bottom right corner
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/cfbdca6e-2f2e-4e74-96cb-09f2089d47c6" />
</p>

- I opened up the HTML file and we can see a fake login page with the exact email address of the user and the root domain is listed at the top
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/75bc0304-84a9-4806-88a4-01bf9d7127a0" />
</p>

- Answer: `kennaroads.buzz`
