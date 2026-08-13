<p align="center">
<img src="https://assets.tryhackme.com/img/logo/tryhackme_logo_full.svg" width="150" alt="TryHackMe Logo">
</p>

# The Greenholt Phish
|  Room Name | The Greenholt Phish |
|----------|-------|
| Author | Dhruv Tripathi |
| Link | [The Greenholt Phish](https://tryhackme.com/room/phishingemails5fgjlzxc) |

# Room Information
```bash Type: Walkthrough
Difficulty: Easy
Tags: - 
Meta Tags: CTF, CTF Challenge, Capture The Flag
Subscription type: Premium
Description:
Use the knowledge attained to analyze a malicious email.
```

## Scenario

A sales executive at Greenholt PLC has reported a suspicious email received from a known customer. The message raised several red flags: a generic greeting, an unexpected request for a money transfer, and an unsolicited attachment. According to the employee, this behavior does not align with the customer’s usual communication style. Concerned that the email may be malicious, the message has been escalated to the SOC (Security Operations Center) for further investigation. Your goal is to analyze the provided email sample and determine whether it is legitimate or part of a phishing attempt.

## Objective
- Analyze the provided email to identify and extract key artifacts
- Investigate the message source to determine its origin and authenticity
- Use analysis tools to assess the potential maliciousness of the email

## Solution
### What is the Transfer Reference Number listed in the email's Subject line?

- I first opened the email and read all the content in it. Seemed like the email is saying some type of transfer of funds had been made to `webmaster@redacted.org` by ` Mr. James Jackson (info@mutawamarine.com)` and all other details are present in the email including the reference number, status, money sent and so on
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/50e69a46-f0bd-402a-87fc-362798c270f7" />
</p>

- The transfer reference number is listed at the top
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/c15c9fe5-4d8d-4dc3-a253-ad6f73f43aa4" />
</p>

- Answer: `09674321`

### What is the display name of the sender?
- The display name of the sender is listed at the top. This is an email from `Mr. James Jackson` to the specified email in the `To` part
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/295ced57-f8b3-42a7-922c-fffc7c2c6295" />
</p>

- Answer: `Mr. James Jackson`

### Continue investigating the email headers. What is the sender's email address?
- The sender's email address is listed right below the display name
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/2f4d6add-e82e-4565-bbac-6b87a5b48d13" />
</p>

- Answer: `info@mutawamarine.com`

### What email address will receive a reply to this email?
- The reply will made to the email listed next to the `Reply To`
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/65cbc541-093e-4bbd-b5ff-bd4ca0f59063" />
</p>

- Answer: `info.mutawamarine@mail.com`

### Begin analyzing the message source. What is the originating IP address of this email?
- I clicked on `View` and then `Message Source` to view the raw data of the email
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/6c97af46-8133-4219-8057-cd95f762ef0f" />
</p>

- We need to look at the earliest `Received` header to find the true origin IP address. The reason we chose this IP address is because email headers are read in reverse chronological order, so the bottom entry shows the earliest connection. `192.119.71.157` is the first public IP listed, making it the most likely originating IP

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/96973245-6b32-48ae-a516-00f5f12adc39" />
</p>

- Answer: `192.119.71.157`

### Investigate the IP address from the previous question. Who is the owner of the originating IP?
- Using an IP lookup tool on Google, I searched `IP lookup tool` and I typed in the IP address I got from the last question and we can see who the owner is below
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/d5f5d7b9-70c7-497c-82fb-66aa6f999215" />
</p>

- Answer: `HostPapa`

### Run an SPF record check on the Return-Path domain identified in the email headers. What is the full SPF record for this domain?
- I copied the return path here next to `Return-Path`. Note that we only copy the domain part which is `mutawamarine.com`
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/3c065fd1-de55-462a-89a9-fec1f00f4dad" />
</p>

- I went on Google and searched `spf record check` and I used dmarcian's `SPF Checker and Validator` but other people may use different ones. I typed in the domain and we can see the full SPF record below. Here we can that the mail servers authorized by Microsoft 365/Outlook are allowed to send emails claiming to be from this domain and `-all` means any other mail server is not authorized

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/647843b8-d7d5-401f-8fa8-08a7c258a818" />
</p>

- Answer: `v=spf1 include:spf.protection.outlook.com -all`

### Perform a DMARC lookup for the Return-Path domain found in the email headers. What is the complete DMARC record for this domain?
- Solving this task followed a similar process compared to the last task and I searched on Google for `DMARC checker`. I once again used dmarcian's `DMARC Record Checker`. We can see the DMARC record below and it even tells what each part means
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/74357b97-658b-42a8-b28d-83ab3e205452" />
</p>

- Answer: `v=DMARC1; p=quarantine; fo=1`


### What is the file name of the attachment found in the email?
- I scrolled down a little and I saw the name of the file attachment that was in the email
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/ae824b66-a6ed-4c40-9f28-c64e6ceb9dc1" />
</p>

- Answer: `SWT_#09674321____PDF__.CAB`

### Download the attachment to your virtual environment. Using the `sha256sum` command, what is the `SHA256` hash of the file?
- I first downloaded the attachment by clicking the `Save` button in the right corner
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/b3862e98-561f-41bf-81a3-9c8bd815e59a" />
</p>

- Then I ran the command `sha256sum SWT_#09674321____PDF__.CAB` to get the `SHA256` hash of this file
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/c5bcb481-8420-4c86-b905-417b9e39f424" />
</p>

- Answer: `2e91c533615a9bb8929ac4bb76707b2444597ce063d84a4b33525e25074fff3f`

### Investigate the file hash from the previous question using VirusTotal (opens in new tab). What is the attachment's file size in KB (e.g., 122.31 KB)?

- We clearly know this is a malicious RAR archive and the size is located on the right under `Size`
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/5970448c-dad6-4894-b0ec-c7a04af08a23" />
</p>

- Answer: `400.26 KB`

### Continue your research on the file. What is the actual file type of the attachment?

- The file type is a RAR archive as listed in the green blob under the file name
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/88bd8f34-50c9-43c2-8e01-ba1a3e0e44ff" />
</p>

- We can even confirm this by running `file` in the terminal of the Attackbox to double check our findings
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/1eaa7dbc-b546-4986-8022-7f1b6eb219e4" />
</p>

- Answer: `RAR`
