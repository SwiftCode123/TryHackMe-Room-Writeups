<p align="center">
  <img src="https://assets.tryhackme.com/img/logo/tryhackme_logo_full.svg" width="150" alt="TryHackMe Logo">
</p>

# Data Exfiltration Detection
|  Room Name | Data Exfiltration Detection |
|----------|-------|
| Author | Dhruv Tripathi |
| Link | [Data Exfiltration Detection](https://tryhackme.com/room/dataexfildetection) |

# Room Information
```bash Type: Walkthrough
Difficulty: Medium
Tags: - 
Meta Tags: Walkthrough, Walk-through, Write-up, Writeup
Subscription type: Premium
Description:
Learn how to detect data exfiltration attempts in various network channels.
```
## Task 1

### Continue to the next task.

- Answer: `No answer needed`

## Task 2

### Connect with the lab.

- Answer: `No answer needed`

## Task 3
### Exfiltrating the data through HTTP comes under which technique?

- We can see which row exfiltrating data under HTTP comes under

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/37c88f94-6dad-4f63-ab08-ddbb48ca1454" />
</p>

- Answer: `Network-based`

## Task 4

## What is the suspicious domain receiving the DNS traffic?

- Using Wireshark, we can see a large number of suspicious DNS queries being sent to this particular domain. Furthermore, we use the filter below because malware may sometimes send DNS queries to nonexistent or unreachable domains, which can be an indicator of suspicious activity
```
dns && dns.flags.response == 0
```
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/f7ef7160-88a2-45dd-b558-05da410f4966" />
</p>

- Answer: `tunnelcorp.net`

### How many suspicious traffic/logs related to dns tunneling were observed?
- In Wireshark, since we know that the DNS queries are long, we can search for long queries specifically. We can then see the total number of displayed packets
```
dns && frame.len > 70
```
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/a3b592fc-06f9-49bb-be8b-5d8c0239a960" />
</p>

- Answer: `315`

### Which local IP sent the maximum number of suspicious requests?

- We can use Splunk to filter for this IP. However, we need to be careful because we need to remember that not all requests sent by these IP addresses are suspicious.  That's why I specified the length because other requests may be shorter than

```
index="data_exfil" sourcetype="DNS_logs" | where len(query) > 30
```

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/38628d2e-6ca4-4d1b-8688-00e8dccecf67" />
</p>

- Answer: `192.168.1.103`
