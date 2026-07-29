<p align="center">
<img src="https://assets.tryhackme.com/img/logo/tryhackme_logo_full.svg" width="150" alt="TryHackMe Logo">
</p>

# MITRE
|  Room Name | MITRE |
|----------|-------|
| Author | Dhruv Tripathi |
| Link | [MITRE](https://tryhackme.com/room/mitre) |

# Room Information
```bash Type: Walkthrough
Difficulty: Medium
Tags: - 
Meta Tags: Walkthrough, Walk-through, Write-up, Writeup
Subscription type: Premium
Description:
Explore the various resources that MITRE has made available to the cyber security community.
```
## Task 1

### I understand the learning objectives and am ready to learn about MITRE!

- Answer: `No answer needed`

## Task 2

### What Tactic does the Phishing technique belong to in the ATT&CK Matrix?

- If we take a look at the MITRE ATT&CK Matrix, we can see what column `Phishing` belongs to
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/aa0479e9-e6a6-42f9-8ea1-e8ea53459494" />
</p>

- Answer: `Initial Access`

### Which ID is associated with the Create Account technique?

- I clicked on `Create Account` under the `Persistence` column
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/ccb17239-91bd-4ce6-919b-4a3c11690a3e" />
</p>

- We can then see the ID here
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/0bf5a419-7794-44f9-bfe6-16b4a90785f7" />
</p>

- Answer: `T1136`

## Task 3

### In which country is Mustang Panda based?

- TryHackMe gave me the link to go to `Mustang Panda`'s matrix page and we can see where they are based in
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/272ba040-a456-40fa-9aec-a65cb7fe76c5" />
</p>

- Answer: `China`

### Which ATT&CK technique ID maps to Mustang Panda’s Reconnaissance tactics?

- `Mustang Panda` specifically uses Spearphishing as noted below whether it is `Spearphishing Link` or `Spearphishing Attachment`

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/65be71cc-00db-40d1-81e5-e4317672aed9" />
</p>

- I was given a link by TryHackMe to use the ATT&CK Navigator for `Mustang Panda` and I hovered over `Spearphishing Link` and saw what the associated ATT&CK technique ID was
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/af7f753e-5cb4-45e8-9247-3612cb552475" />
</p>

- Answer: `T1598`

### Which software is Mustang Panda known to use for Access Token Manipulation?

- I did `Ctrl + F` for `Access Token Manipulation` and on the left side, it lists the software they use
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/28fb2ae4-e28f-405a-8ab0-fdc03c9dd453" />
</p>

- Answer: `Cobalt Strike`

## Task 4

### Which APT group has targeted the aviation sector and has been active since at least 2013?

- I used `Ctrl + F` to find `aviation` and the first one showed up which was also active since 2013
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/f612b043-e04c-4d43-9d1e-becb8ba43cc9" />
</p>

- Answer: `APT33`

### Which ATT&CK sub-technique used by this group is a key area of concern for companies using Office 365?

- I clicked on `APT33` from the last question and went to their page specifically. I again used `Ctrl + F` to find `office` and we can see the name of the sub-technique that targets companies using Office 365
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/0a260327-9e5a-48d5-b83b-b9b670d757bd" />
</p>

- Answer: `Cloud Accounts`

### According to ATT&CK, what tool is linked to the APT group and the sub-technique you identified?

- Here, the description lists the software they use
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/604eb1c1-8a70-456e-8ba0-8cafafd2f432" />
</p>

- Answer: `Ruler`

### Which mitigation strategy advises removing inactive or unused accounts to reduce exposure to this sub-technique?

- I clicked on the sub-technique listed
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/7c66ea38-9fcd-450f-9877-46812aa39a39" />
</p>

- Under the `Mitigations` section, we can see the mitigation name for removing inactive or unused accounts for this particular sub-technique
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/8343429d-1acf-4ad5-b474-a82ec3e285c5" />
</p>

- Answer: `User Account Management`

### What Detection Strategy ID would you implement to detect abused or compromised cloud accounts?

- Under the `Detection Strategy` section, we can see the ID listed
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/1fc59253-570a-4a78-ae3f-cb6cbb236a04" />
</p>

- Answer: `DET0546`

## Task 5

### Which ATT&CK Tactic is associated with CAR-2019-07-001 (opens in new tab)?

- TryHackMe gave the link to this page for this analytic and below listed is the ATT&CK tactic
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/aced4293-d41b-49bb-87ce-a1a6927be9a9" />
</p>

- Answer: `Defense Evasion`

### What is the Analytic Type for Access Permission Modification?

- The `Analytic Type` is listed in the box
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/b5d22377-f74e-443d-b646-a7e0fbfa7163" />
</p>

- Answer: `Situational Awareness`

## Task 6

### Which sub-technique of User Behavior Analysis (opens in new tab) would you use to analyze the geolocation data of user logon attempts?

- TryHackMe gave me a link to this page and we can see the defensive technique for analyzing the geolocation of user logon attempts
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/c79a7fa7-b673-49e2-8f5b-6a2e3b5b820b" />
</p>

- Answer: `User Geolocation Logon Pattern Analysis`

### Which digital artifact does this sub-technique rely on analyzing?

- I clicked on the ID next to `User Geolocation Logon Pattern Analysis`
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/61616a22-b98b-4558-9abb-678697998c72" />
</p>

- We can see what artifact it points to
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/90428705-0d4c-43c6-a986-769b90075eb4" />
</p>

- Answer: `Network Traffic`

## Task 7

### What technique ID is associated with Scrape Blockchain Data (opens in new tab) in the AADAPT framework?

- TryHackMe gave me the link for this page and we can see the ID listed below
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/5302d9ec-7e0a-4072-931e-eb370f3b6038" />
</p>

- Answer: `ADT3025`

### Which tactic does LLM Prompt Obfuscation (opens in new tab) belong to in the ATLAS framework?

- TryHackMe gave me the link to this page and we can see the tactic below
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/e3a6ec05-9367-4258-9de2-1ec8b94d1eee" />
</p>

- Answer: `Defense Evasion`
