
WORK IN PROGRESS

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

### E-corp has found that the APT might have gained initial access using social engineering to make the user execute code for the threat actor. Sunny wants to identify if the APT was also successful in execution. What two techniques of user execution should Sunny look out for? (Answer format: <technique 1> and <technique 2>)

- The attacker might have tricked the user into executing the code whether its through opening an attachment or clicking a link. Below, I found the two sub-techniques under the `User Execution` technique under the `Execution` tactic. `Malicious File` gets the user to click on a Microsoft Office attachment with a malicious macros script and `Malicious Link` gets the user to click on malicious hyperlinks

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/d3606d48-afc9-4a11-9355-c5290e2e544a" />
</p>

- Answer: `Malicious file and malicious link`

### If the above technique was successful, which scripting interpreters should Sunny search for to identify successful execution? (Answer format: <technique 1> and <technique 2>)

- The scripting interpreters can be found here under the same tactic. `Powershell` and `Windows Command Shell` are common tools to execute these scripts
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/977f69d6-34b2-445a-9e01-86319330f7af" />
</p>

- Answer: `Powershell and Windows Command shell`

### While looking at the scripting interpreters identified in Q4, Sunny found some obfuscated scripts that changed the registry. Assuming these changes are for maintaining persistence, which registry keys should Sunny observe to track these changes?

- Under the `Persistence` tactic, we can see the answer. `Registry Run Keys` allows the malware to gain persistence because it can place itself in that directory and always runs whenever the computer restarts or boots up

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/66534fc7-94fd-447a-ab98-9685e3baf967" />
</p>

- Answer: `Registry run keys`

### Sunny identified that the APT executes system binaries to evade defences. Which system binary's execution should Sunny scrutinize for proxy execution?

- It is a system binary located under the `Defense Evasion` tactic. While `rundll32.exe` is normally used by Windows to load DLL files, an attacker can hijack the real, trusted binary to load and execute malicious DLLs instead

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/04f60607-e0c7-40d6-8449-7899789c630f" />
</p>

- Answer: `Rundll32`

### Sunny identified tcpdump on one of the compromised hosts. Assuming this was placed there by the threat actor, which technique might the APT be using here for discovery?

- The attacker can use `tcpdump` for capturing, displaying, and filtering live network traffic passing through a device's network interface such as plaintext username, passwords from insecure protocols, IP addresses, OS types, etc.

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/434929c5-fab6-49f1-bfdc-506872e30c29" />
</p>

- Answer: `Network sniffing`

### It looks like the APT achieved lateral movement by exploiting remote services. Which remote services should Sunny observe to identify APT activity traces?

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/185bdf8b-5edd-4e0e-afcd-081e17de83fe" />
</p>

- Answer: `SMB/Windows Admin shares`
