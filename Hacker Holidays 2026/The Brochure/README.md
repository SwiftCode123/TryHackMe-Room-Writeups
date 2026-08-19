<p align="center">
  <img src="https://assets.tryhackme.com/img/logo/tryhackme_logo_full.svg" width="150" alt="TryHackMe Logo">
</p>

# The Brochure

| Room Name | The Brochure |
|----------|---------------|
| Platform | TryHackMe |
| Event | Hacker Holidays 2026 |
| Points | 30 |
| Category | 🕵️ OSINT |
| Difficulty | Easy |
| Tags | OSINT, Image Analysis, Social Media, Digital Footprinting |
| Status | ✅ Completed |

## Room Information

```bash
Type: CTF Challenge
Difficulty: Easy
Category: OSINT

Description:
The brochure's hero photo has an AI fingerprint. Follow the account that posted it, and the trail doesn't end at the hotel; it ends at someone the hotel never mentioned.
```

## Challenge Overview

**Hacker Holidays: Day 0 — Recon**

Before arriving at the Byte Lotus Hotel, you're tasked with performing reconnaissance using publicly available information. A seemingly innocent brochure image hides valuable clues that lead to an overlooked online presence.

By combining image analysis with OSINT techniques, you'll uncover the hotel's hidden social media account and recover the flag left behind.

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/e2eaa422-b1fc-4db9-899e-476bca0b7240" />
</p>

## Objectives

 - Analyze the provided image for embedded clues.
 - Apply fundamental OSINT techniques to trace the findings.
 - Locate the hidden social media account.
 - Submit the flag.

## Solution

- This one was a bit of a fun OSINT challenge. Firstly, we are given the AI-generated image and I noticed it said `Find us on Instagram or not.`
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/140c1601-e32f-40f3-9584-7c51c9b59a71" />
</p>

- Then, I searched `BYTE LOTUS RESORTS instagram` on the Google and clicked on the first link
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/a00d08c8-7ba4-42b9-a815-6fed15ccde28" />
</p>

- I clicked around on the images but did not find much... but then I clicked on the `1 following` to see who it was
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/c0a527d7-01ad-45d6-9987-a77f426892e4" />
</p>

- It was `VERA` with what seemed like Base64 encoding?
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/cca8c02b-f17a-4d02-9e37-3e215f4b4992" />
</p>

- Using CyberChef, I cracked that Base64 code and retrieved the flag
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/e38e353e-0ca2-473e-8deb-cbc4795d5bfd" />
</p>

## Skills Learned
- Basic OSINT techniques
- Analyzing images for visible clues
- Investigating social media profiles
- Decoding Base64-encoded text with CyberChef
- Following digital footprints using publicly available information

## Tools Used
- Web Browser
- Google Search
- Instagram
- CyberChef

## Flag

🚩 Flag: `THM{*************************}`
