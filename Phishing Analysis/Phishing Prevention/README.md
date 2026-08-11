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

### How many SMTP packets are available for analysis?

- Just typing in `smtp` gives us the total number of packets

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/216a85f4-3a96-4249-aa6f-e7f7df68fbc0" />
</p>

- Answer: `512`

### What is the name of the attachment in packet `270`?

- I isolated packet `270` via the query `frame.number == 270 && smtp`
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/401a7289-24b4-4cda-bbd8-141cc6b6a647" />
</p>

- I clicked on the packet and scrolled all the way down under `Simple Mail Transfer Protocol` and saw the attachment name
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/a73ceebb-e441-4eda-91c2-fa9c69597c76" />
</p>

- Answer: `document.zip`

### According to the message in packet 270, which Host IP address is not responding, making the message undeliverable?

- Looking up, we can see the host IP address that is not responding

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/83e3c019-2eb4-4e1a-8d0b-1fab497aa494" />
</p>

- Answer: `212.253.25.152`

### By filtering for `imf`, which email client was used to send the message containing the attachment `attachment.scr`?

- We can filter down for `imf` and which packet contains `attachment.scr` via the query `imf contains attachment.scr`
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/8f01c320-f3ca-433d-8f2b-7375ab658b98" />
</p>

- We can see the specific email client as indicated by `X-Mailer`
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/a9eb6bfb-dbdc-4196-87a1-308f1eb243aa" />
</p>

- Answer: `Microsoft Outlook Express 6.00.2600.0000`

### Which type of encoding is used for this potentially malicious attachment?

- We can see the type of content encoding in `Content-Transfer-Encoding`
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/dd74a866-d81d-4da7-976b-a1408fba8cef" />
</p>

- Answer: `base64`

## Task 7
### A security team wants to implement a control to detect hidden malware inside email attachments. They need a way to analyze suspicious files without risking infection on real systems. Which protective technique would allow them to observe a file's behavior safely?

- With sandboxing, we can isolate and test suspicious links/attachments to check for malicious behavior

- Answer: `Sandboxing`

## Skills Learned

* Understood how SPF, DKIM, and DMARC help authenticate email senders and detect unauthorized messages
* Interpreted SPF SoftFail and DKIM `permerror` results to identify email authentication failures
* Identified the `p=reject` DMARC policy as a method for blocking emails that fail authentication checks
* Understood how S/MIME encryption uses public and private keys to protect email contents from unauthorized access
* Used Wireshark filters such as `smtp.response.code` to analyze SMTP response codes and identify blocked messages
* Analyzed SMTP packets and email headers to identify attachments, email clients, delivery errors, and message details
* Identified Base64 as an encoding method used to transfer email attachments
* Learned the purpose of sandboxing for safely analyzing suspicious attachments without risking infection on real systems

## Conclusion
This room provided a better understanding of how phishing emails can be identified and prevented through email authentication, header analysis, and network traffic inspection. I learned how SPF, DKIM, and DMARC help verify legitimate senders, how Wireshark can be used to investigate SMTP traffic and suspicious attachments, and how sandboxing can safely analyze potentially malicious files. Overall, the room helped strengthen my understanding of the different techniques used to detect and prevent phishing attacks
