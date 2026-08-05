<p align="center">
  <img src="https://assets.tryhackme.com/img/logo/tryhackme_logo_full.svg" width="150" alt="TryHackMe Logo">
</p>

# Phishing Prevention
|  Room Name | Phishing Prevention |
|----------|-------|
| Author | Dhruv Tripathi |
| Link | [Phishing Prevention](https://tryhackme.com/room/phishingemails4gkxh) |

# Room Information
```bash Type: Walkthrough
Difficulty: Easy
Tags: - 
Meta Tags: Walkthrough, Walk-through, Write-up, Writeup
Subscription type: Premium
Description:
Learn how to defend against phishing emails.
```

## Task 1
### I understand the learning objectives and am ready to learn about phishing prevention!

- Answer: `No answer needed`

## Task 2
### Based on TryHackMe's SPF record above, how many domains are authorized to send email on its behalf?

- We can see how many domains there are below in THM's SPF record. There is `_spf.google.com`,
`email.chargebee.com`, and `7168674.spf05.hubspotemail.net`
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/5c9b5c84-9896-45f6-be01-a5e76cbef20c" />
</p>

- Answer: `3`

### What is the intended action of an email that returns a SoftFail verification result?

- The intended action for `SoftFail` is detailed below. Essentially, it means that the sending mail server is not listed as an authorized sender but the receiving server still accepts it but flags it as suspicious

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/313682eb-fad5-4372-a3ea-d726a9c2c1da" />
</p>

- Answer: `Flag`

## Task 3
