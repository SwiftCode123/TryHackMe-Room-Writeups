<p align="center">
<img src="https://assets.tryhackme.com/img/logo/tryhackme_logo_full.svg" width="150" alt="TryHackMe Logo">
</p>

# Introduction to Phishing
| SOC Simulation Name | Introduction to Phishing |
|----------|-------|
| Author | Dhruv Tripathi |
| Difficulty | Beginner |

## Scenario Overview

NEW? START HERE! Learn how to use SOC Simulator by completing your first scenario. Close all True Positive alerts to pass!

## Scenario Objectives
- Monitor and analyze real-time alerts.
- Identify and document critical events such as suspicious emails and attachments.
- Create detailed case reports based on your observations to help your team understand the full scope of alerts and malicious activity.

## Scenario Walkthrough
- Before starting the simulation, I chose the SIEM tool I wanted to use which was Splunk as I had the most experience with it and familiarity
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/601ba812-8549-4d3d-894e-60421309a0ab" />
</p>

- BOOM!, we are here and the first thing I looked at was the documentation to get a better understanding of what I was going to do
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/2908faa8-6f9c-437b-9cbb-25f2fc9fb6fc" />
</p>

- The `Alert Triage` told me how to review the alerts, investigate it in the SIEM, and the resolution and closure
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/bc28b79d-1aa2-452c-a0bd-f2856d6fdb02" />
</p>

- The `Alert Classification` told me the different scenarios about when to escalate and when to not as well as information about what a true/false positive can be
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/9c38affb-93ab-41bd-8ad2-9e15259676ac" />
</p>

- The `Alert reporting` told me how to write my reports which is very important so I particularly paid attention to this
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/a3356a80-0476-413b-bc0c-2158aceda33a" />
</p>

- I looked through the `Company Information` and it seemed like this may come in handy later
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/500351bb-8a15-4582-b835-ac21f2db71c0" />
</p>

- The last `Asset inventory` was just the network and subnets
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/8e792784-9f3a-40ae-935b-6551972e5875" />
</p>

- I went to the `Alert Queue` and picked the highest severity one first
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/13be1fbb-cb9d-4c91-8711-e97d7de2d43e" />
</p>

- I clicked the action button on the right of the alert to take ownership of this alert
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/a79e3f96-c2df-403e-a22c-5d8a4a4887c6" />
</p>

- Then I started to read the alert description. At this moment from what I understand, a user or device tried to access a URL that was blacklisted by the organization. It happened on `08/14/2026` at `23:52:29.036`. It was `blocked` and we can see the `SourceIP` and the `SourcePort` as well as the `DestinationIP` and `DestinationPort`. That URL does look a bit suspicious but we can't assume anything yet

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/72fb3a39-aaeb-46aa-a0ad-14123a58d317" />
</p>

- Before opening the SIEM, I was curious who that IP address belonged to in the `Company Information` we saw earlier and it was someone from HR named `Hannah Harris`
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/6bf9da45-2c9e-4cd3-adbd-17f0630e8e92" />
</p>

- After looking at the SIEM, I went back and clicked the `Analyst VM`
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/11e0902c-4550-437c-be21-494d8a55ab22" />
</p>

- Then, I did a reputation check of the URL and we can see it was marked as `MALICIOUS`. I digged in deeper using the SIEM
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/49e8d46b-1440-4b6f-881a-5640e22838fa" />
</p>

- Now, I opened the SIEM and typed the query and we can see the exact log and we know its this log because all of the information such as `timestamp` match as well
```
* datasource=firewall SourceIP=10.20.2.17
```
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/8da67c5d-48e7-408b-8f9f-0b862dbfcd84" />
</p>

- Dissecting the log further, it seemed like the connection was blocked fortunately to the destination IP/URL
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/e6076cf1-bd79-42db-b24d-de35fef7206b" />
</p>

- I went back to the dashboard and clicked on `Write case report`
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/98ce0f67-97cd-44e5-87f0-ef4966194f83" />
</p>

- I classified this as a `True Positive` because the user attempted to access a URL identified as malicious or blacklisted by the organization's security controls. Although the firewall successfully blocked the connection and prevented further harm, the malicious activity itself was genuine

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/890bbecf-0b0e-42e8-8d93-0bc3080cc87b" />
</p>

- I wrote my report according to the best practices in the documentation we read earlier and included all the relevant information. There was no escalation required for this as the firewall blocked the connection and no further suspicious activity was present. I then closed the alert. This was one alert done!
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/60ace5b6-69c9-402e-ba5f-06d4caa0384d" />
</p>

### Report #1 
> This activity is classified as a True Positive. The activity involved Hannah Harris from the Human Resources department, using workstation win-3457 with IP address 10.20.2.17. At 19:15 on August 14, 2026, the workstation attempted to access the blacklisted external URL http://bit.ly/3sHkX3da12340, communicating with destination IP 67.199.248.11 over TCP port 80.

> The activity is considered a True Positive because the requested URL was identified as a blacklisted destination by the organization's security controls. The firewall detected the request and successfully blocked the outbound connection under the Blocked Websites rule, preventing the workstation from reaching the destination.

> No escalation is required at this time, as the connection was successfully blocked and there is no evidence of successful communication or compromise. Recommended remediation includes monitoring workstation win-3457 for any repeated attempts to access blacklisted or suspicious URLs and reviewing the user's activity if similar alerts occur. The relevant indicators are source IP 10.20.2.17, source port 34257, destination IP 67.199.248.11, destination port 80, and the requested bit.ly URL.

- I then moved onto the next alert. I picked the alert with ID `8814` and took ownership of it by clicking the action button on the right side
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/a1cdaa29-03f7-4c9b-9d03-4ec944449267" />
</p>

- I read the alert details and it seemed like at first glance that the user received an email containing a URL

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/7f5c74a5-1469-4f9b-99fc-9a4a283f0599" />
</p>

- I first copied the URL and did a reputation check on it and the the status was clean so this was a bit of a relief
```https://hrconnex.thm/onboarding/15400654060/j.garcia```

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/ff5d3124-23be-4440-bcc9-87f4f7775b77" />
</p>

- I went into the SIEM and typed in my query to find the specific log
```
* datasource=email sender=onboarding@hrconnex.thm
```

- Now, out of these two logs, the `timestamp` in the alert detail matches the second one and so I investigated that one
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/de543740-e152-4e88-8021-cdbba3b8e407" />
</p>

- It seemed like there was nothing malicious happening here as we also confirmed this with the reputation check that this link was not malicious and there is no malicious indication anywhere. Based on the analysis, the email appears to be legitimate and was likely sent to assist the user with setting up their profile

- I wrote the report here and proceeded to mark this as a false positive as this was a legitimate email
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/fe2c526a-51d2-4bfa-9069-73756edaad38" />
</p>

### Report #2
> This activity is classified as a False Positive. The email was sent to Julia Garcia from j.garcia@thetrydaily.thm with the subject “Action Required: Finalize Your Onboarding Profile.” The email originated from onboarding@hrconnex.thm and contained a link to hrconnex.thm/onboarding/15400654060/j.garcia, which is consistent with an onboarding and profile setup process.

> The alert was triggered because the inbound email contained an external link. However, there were no attachments or other indicators of malicious content identified in the email, and the link is consistent with the stated HR onboarding purpose. No evidence of malicious activity or compromise was identified during the investigation.

> No escalation is required. Recommended remediation is to close the alert as a False Positive and continue monitoring for any related suspicious email or activity. The relevant indicators are the sender onboarding@hrconnex.thm, recipient j.garcia@thetrydaily.thm, and the URL https://hrconnex.thm/onboarding/15400654060/j.garcia.

- I now moved onto the next alert. Now this alert was similar to the last one as the user received an email with a link which was flagged but this one seemed a bit suspicious with the sender address and the link 
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/3af81d55-425b-4d92-9898-0f7d3fd2e856" />
</p>

- I did a reputation on the link and it flagged it as malicious
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/1cdc2708-41ec-4f03-94bc-70be77db1f36" />
</p>

- I searched in Splunk for this log and it seems like the user did in fact receive an email with from what seems like it is "Amazon" per se but with a malicious link
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/2edf609b-b879-43d1-9e1d-ae6f2d4ad7e6" />
</p>

- I then checked to see if the user did in fact click the link and we can see they did as the IP address `10.20.2.17` is associated with `h.harris@thetrydaily.thm` but the connection was blocked. We now know that alerts `8815` and `8816` are related to one another where `8815` is the receiving the email and `8816` was the firewall logs where the user did click on the malicious link 

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/7f961a19-a697-4414-9529-674c0c0855af" />
</p>

- I marked it as a true positive but no escalation required because while this was a phishing email, the firewall blocked the connection
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/144cced3-0e15-458a-8c10-70a994d8ac18" />
</p>

### Report #3
> This activity is classified as a True Positive. The email was sent to Hannah Harris from [urgents@amazon.biz](mailto:urgents@amazon.biz) with the subject “Your Amazon Package Couldn’t Be Delivered – Action Required” and contained a shortened bit.ly link requesting the recipient to confirm shipping information. The urgent delivery message, suspicious sender domain, and shortened external URL are consistent with phishing activity.

> The URL in the email was identified as suspicious, and the associated connection was blocked by the firewall, preventing access to the destination. No evidence of successful communication or compromise was identified, so the alert does not require escalation.

> Recommended remediation is to remove or quarantine the phishing email, ensure the URL remains blocked, and monitor Hannah Harris's workstation, win-3457 (10.20.2.17), for any further attempts to access suspicious links. The relevant indicators are the sender [urgents@amazon.biz](mailto:urgents@amazon.biz), recipient [h.harris@thetrydaily.thm](mailto:h.harris@thetrydaily.thm), URL http://bit.ly/3sHkX3da12340, and destination IP 67.199.248.11.

- Now I moved onto the next alert which was also another flagged email. This one contained a suspicious link and was overall more obviously suspicious because as we know the domain after the `@` symbol is clearly misspelled and trying to impersonate `Microsoft`

<p align="center">
<img width="1512" height="859" alt="image" src="https://github.com/user-attachments/assets/b9a96cdc-5863-43f7-a3d7-61d04b2754ba" />
</p>

- I did a reputation check on the link and it was flagged as malicious

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/5f88d226-5b94-4745-b1a7-6a044fa19158" />
</p>

- We can clearly see this log in the SIEM as there was indeed a inbound email
```
* datasource=email sender=no-reply@m1crosoftsupport.co
```
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/500bea32-b49a-42c2-a3cf-602d4ce5269c" />
</p>

- Now, I wanted to check if the user had clicked the link and it seemed like there was an allow connection from the firewall. We know now that the user did click on the link and unlike last time the connection wasn't blocked. We know this since the IP address `10.20.2.25` corresponds with the email `c.allen@thetrydaily.thm` which I got from the `Company Information` under `Documentation`. `Since this was a login page, it was completely possible that the user entered their credentials. This required immediate escalation
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/269f6764-263b-4a85-86d0-5e6c3b574bd4" />
</p>

- I checked it as true positive, wrote the report and escalated the alert
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/8f7c748c-ee54-46ab-acf4-d5341cd0794f" />
</p>

### Report #4
> This activity is classified as a True Positive. The phishing email was sent to Charlotte Allen from no-reply@m1crosoftsupport.co with the subject “Unusual Sign-In Activity on Your Microsoft Account.” Charlotte is a Web Development employee using workstation win-3463 with IP address 10.20.2.25. The email impersonates Microsoft, uses the lookalike domain m1crosoftsupport.co, and directs the recipient to a suspicious login page.

> The alert requires escalation because firewall logs show that the connection to the suspicious destination was allowed. This indicates that Charlotte's workstation was able to communicate with the malicious URL, creating a potential risk of credential theft or further compromise. The activity should therefore be investigated to determine whether the user accessed the page or submitted any credentials.

> Recommended remediation includes blocking the malicious domain and URL, reviewing activity from win-3463 (10.20.2.25), and checking for any additional connections to the suspicious infrastructure. The phishing email should also be removed or quarantined. If credentials were submitted, Charlotte's account should be secured and her password reset.

> The relevant indicators are no-reply@m1crosoftsupport.co, c.allen@thetrydaily.thm, win-3463, 10.20.2.25, m1crosoftsupport.co, https://m1crosoftsupport.co/login, and 102.89.222.143.
