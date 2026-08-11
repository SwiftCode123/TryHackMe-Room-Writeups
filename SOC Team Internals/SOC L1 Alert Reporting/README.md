<p align="center">
<img src="https://assets.tryhackme.com/img/logo/tryhackme_logo_full.svg" width="150" alt="TryHackMe Logo">
</p>

# SOC L1 Alert Reporting
|  Room Name | SOC L1 Alert Reporting |
|----------|-------|
| Author | Dhruv Tripathi |
| Link | [SOC L1 Alert Reporting](https://tryhackme.com/room/socl1alertreporting) |

# Room Information
```bash Type: Walkthrough
Difficulty: Easy
Tags: - 
Meta Tags: Walkthrough, Walk-through, Write-up, Writeup
Subscription type: Premium
Description:
Learn how to properly report, escalate, and communicate about high-risk SOC alerts.
```
## Task 1
### I am ready to start!

- Answer: `No answer needed`

## Task 2
### What is the process of passing suspicious alerts to an L2 analyst for review?

- Answer: `Alert Escalation`

### What is the process of formally describing alert details and findings?

- Answer: `Alert Reporting`

## Task 3
### According to the SOC dashboard (opens in new tab), which user email leaked the sensitive document?

- If we look through the alerts, we can find one where this person attempts to share sensitive information outside the company by reading the alert information. We can then see the email of the user

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/e482ac76-609e-4294-a421-3f96d867346c" />
</p>

- Answer: `m.boslan@tryhackme.thm`

### Looking at the new alerts, who is the "sender" of the suspicious, likely phishing email?

- This is another alert and we can see the sender is someone we normally discard as not malicious

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/56db7060-a666-43f1-81a0-9967ba012476" />
</p>

- Answer: `support@microsoft.com`

### Open the phishing alert, read its details, and try to understand the activity. Using the Five Ws template, what flag did you receive after writing a good report? Note: Assign yourself, move the alert to In Progress, and fill in the Analyst Comment.

- I assigned myself to the alert and changed the `Status` to `In Progress`
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/e6469909-0c5e-4c53-9cd1-e6ebbfe6d5d5" />
</p>

- Now, it seems like this is a fake email that seems like it comes from Microsoft but actually doesn't. We can note that this fails both SPF and DKIM which means this was not sent from an authorized server for the domain `microsoft.com`, that the email did not actually come from `microsoft.com` and that is may have been possibly modified during transit. Furthermore, considering the urgency in the email strongly suggests this is a phishing email such as `urgent notice` or `download the email` as well as having a suspicious `.rar` attachment. We can write all of this information into a report

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/e9df6429-4c19-447d-aa33-dc19e605b72c" />
</p>

- After submitting, we get the flag

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/48246d6b-33a2-485f-a7a0-c79d77c78910" />
</p>

- Also, I remembered the close the ticket and mark it as a `True Positive`

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/f429e2a9-ba3a-4e10-9132-201c41bfd62c" />
</p>

- Answer: `THM{nice_attempt_faking_microsoft_support}`

## Task 4
### Who is your current L2 in the SOC dashboard (opens in new tab) that you can assign (escalate) the alerts to?

- There was only one L2 analyst here

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/812210ac-d2a4-4718-bf3d-ef90a091b779" />
</p>

- Answer: `E.Fleming`

### What flag did you receive after correctly escalating the alert from the previous task to L2? Note: Set the right assignee and add an intermediate verdict (TP or FP)

- We change the `Assignee` value to `E.Fleming (L2)`

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/9a38a25f-95fe-4aec-82d5-cbf74116155b" />
</p>

- We get the flag after we set the correct value

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/7dbf50b2-8a84-453d-bd35-47d8f61dcbc4" />
</p>

- Answer: `THM{good_job_escalating_your_first_alert}`

### Now, investigate the second new alert and provide a detailed alert comment. What flag did you get after escalating this alert according to the workflow?

- After providing a detailed alert comment, we get the flag

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/25a8e295-0b3e-48c7-8d3b-da9bcf77b2e1" />
</p>

- Here was the comment

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/e3d3156e-3464-456d-814a-5715bf7de7f5" />
</p>

- Answer: `THM{looks_like_webshell_via_old_exchange}`

## Task 5
### Should you first try to contact your manager in case of a critical threat (Yea/Nay)?

- Usually, its L2 to L3 and then the manager

- Answer: `Nay`

### Should you immediately contact your L2 if you think you missed the attack (Yea/Nay)?

- Answer: `Yea`

## Skills Learned

* Learned the role of alert reporting and how accurate documentation supports effective incident response and communication within a SOC
* Understood the difference between alert reporting (documenting findings) and alert escalation (transferring investigations to higher-tier analysts)
* Gained experience navigating a SOC dashboard to investigate alerts and identify relevant evidence
* Developed the ability to recognize phishing indicators such as SPF/DKIM failures, spoofed senders, suspicious attachments, and social engineering tactics
* Practiced using the Five Ws (Who, What, When, Where, and Why) framework to produce clear and structured incident reports
* Learned to document investigation findings through detailed analyst comments and supporting evidence
* Gained experience managing the SOC workflow by assigning alerts, updating statuses, and applying appropriate verdicts (True Positive/False Positive)
* Practiced escalating alerts to L2 analysts while providing sufficient context to support continued investigation
* Reinforced incident response best practices by following the proper escalation chain and established SOC procedures
* Improved technical communication by writing concise, actionable reports for other analysts

## Conclusion

This room introduced the operational responsibilities of a Tier 1 SOC analyst, emphasizing documentation, communication, and proper escalation alongside technical analysis. By investigating phishing and web shell alerts, writing structured reports, assigning verdicts, and escalating incidents appropriately, it demonstrated how effective reporting enables efficient incident response. Overall, the room highlighted that clear documentation and adherence to SOC workflows are essential skills for successful security operations
