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

### Based on the sample header above, what is the reason for the `permerror`?
- The reason is listed to the right of the `dkim=permerror` message
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/a0bd93d6-6cda-4b2b-bff3-cd1af8f4b415" />
</p>

- `permerror` means there is a permanent failure and it could be because of a missing/incorrect DNS record, a forwarding server making a modification, or a misconfiguration in DKIM setup

- Answer: `no key for signature`

## Task 4

### Which DMARC policy provides the greatest amount of protection by blocking emails that fail the DMARC check?

- All emails that fail the DMARC check will be rejected based on the `p=reject` policy tag

- Answer: `p=reject`

## Task 5

### Which S/MIME component ensures that only the intended recipient can read the contents of an email message?

- Let's say we have two character's Bob and Mary. Bob decides to encrypt the message using Mary's public key and only Mary can decrypt it with her private key

- Answer: `Encryption`

## Task 6

### Which Wireshark filter can you use to narrow down your results based on SMTP response codes?

- Using the website TryHackMe gave me (SMTP Wireshark filters), we can see below what the specific Wireshark filter is
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/712d093f-d5b0-49f2-9f73-15c646f9bf4f" />
</p>

- Answer: `smtp.response.code`

### How many packets in the capture contain the SMTP response code 220 Service ready?

- I used the filter `smtp.response.code == 220` to filter down the packets I needed. At the bottom, there is a number for the total count after filtering
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/283bc760-5ff2-4cb7-a39b-210de28bef6f" />
</p>

- Answer: `19`

### One SMTP response indicates that an email was blocked by spamhaus.org. What response code did the server return?

- I filtered by just typing `smtp.response.code` in the search bar and looking down, the response code we see is listed right by `S:`
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/2c6029d4-2063-414a-ada1-16ad7f6157fc" />
</p>

- Answer: `553`

### Based on the packet from the previous question, what is the full `Response code:` message?

- I clicked on the specific packet and the `Response code:` is listed below
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/424de56c-1745-4a53-afa7-d79c8bc79f13" />
</p>

- Answer: `Requested action not taken: mailbox name not allowed (553)`

### Search for response code 552. How many messages were blocked for presenting potential security issues?

- Using the filter `smtp.response.code = 552`, we get the total packets displayed at the bottom
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/2696f857-2981-4afc-8ca9-c83ed0aeff9b" />
</p>

- Answer: `6`

## Task 7
