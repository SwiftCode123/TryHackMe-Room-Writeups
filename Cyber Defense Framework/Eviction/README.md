<p align="center">
<img src="https://assets.tryhackme.com/img/logo/tryhackme_logo_full.svg" width="150" alt="TryHackMe Logo">
</p>

# Eviction
|  Room Name | Eviction |
|----------|-------|
| Author | Dhruv Tripathi |
| Link | [Eviction](https://tryhackme.com/room/eviction) |

# Room Information
```bash Type: Walkthrough
Difficulty: Easy
Tags: - 
Meta Tags: CTF, CTF Challenge, Capture The Flag
Subscription type: Premium
Description:
Unearth the monster from under your bed.
```

## Scenario

Sunny is a SOC analyst at E-corp, which manufactures rare earth metals for government and non-government clients. She receives a classified intelligence report that informs her that an APT group (APT28) might be trying to attack organizations similar to E-corp. To act on this intelligence, she must use the MITRE ATT&CK Navigator to identify the TTPs used by the APT group, to ensure it has not already intruded into the network, and to stop it if it has

Please visit this link(opens in new tab) to check out the MITRE ATT&CK Navigator layer for the APT group and answer the questions below.

## Solution

- I was given a link to the MITRE ATT&CK Navigator layer for this specific APT group
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/503a0a3a-17bb-4a3d-9b8f-318b99a420fa" />
</p>

- I did a little bit of research on this APT group and found that they are attributed to Russia's General Staff Main Intelligence Directorate (GRU) 85th Main Special Service Center (GTsSS) military unit 26165. They conduct espionage, credential harvesting, and disruptive cyber operations aligned with Russian geopolitical interests

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/636d24c2-2808-47c6-ad54-ba87dfb92bba" />
</p>

### What is a technique used by the APT to both perform recon and gain initial access?
- Highlighted in red is that technique used in both the tactics

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/ad6d6d39-c44b-4c94-a657-676a6cda85ce" />
</p>

- Answer: `Spearphishing link`

### Sunny identified that the APT might have moved forward from the recon phase. Which accounts might the APT compromise while developing resources?

- After the group uses the `Spearphishing Link` technique, victims who click the link may have their credentials stolen, which could lead to their email account being compromised which then can be used for other purposes such as sending phishing emails

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/c61d8fab-0a16-494e-aa3c-c69251d9cdd3" />
</p>

- Answer: `Email Accounts`
