<p align="center">
  <img src="https://assets.tryhackme.com/img/logo/tryhackme_logo_full.svg" width="150" alt="TryHackMe Logo">
</p>

# Towel on the Sunbed

| Room Name | Towel on the Sunbed |
|-----------|---------------------|
| Platform | TryHackMe |
| Event | Hacker Holidays 2026 |
| Points | 90 |
| Category | 🌐 Web |
| Difficulty | Medium |
| Tags | Business Logic, API Abuse, Burp Suite, Web Exploitation |
| Status | ✅ Completed |

## Room Information

```text
Type: CTF Challenge
Difficulty: Medium
Category: Web Exploitation / Business Logic

Description:
Ponzi set his towel down for one 24-hour reward claim. He came back to find the sunbed had been "claimed" three times over while he wasn't looking.
```

## Challenge Overview

**Hacker Holidays: Day 8 — Towel on the Sunbed**

The Byte Lotus Hotel's wellness portal hosts a crypto rewards application where users can claim a daily reward. The application enforces a 24-hour cooldown before another claim can be made, preventing guests from accumulating rewards too quickly.

The objective is to analyze how the application enforces this restriction, identify weaknesses in the business logic, abuse the application's API, and ultimately obtain enough rewards to unlock the Whale Vault and retrieve the flag.

## Objectives

- Create a guest account and explore Ponzi's daily reward mechanism.
- Work out exactly what's standing between you and Whale Vault status. 
- Find your way past it and retrieve the flag from the vault.

## Solution

- This challenge had to do with exploiting a race condition specifically a Time-of-Check to Time-of-Use. The challenge specifically states that `The app disagrees, politely, once every 24 hours. Somewhere between his request and the server's clock, there's a gap wide enough to walk a whale through`. Furthermore, `@0xMia` notes that the user thinks the clock is the only thing checking him, pointing to a flaw in how parallel requests are processed
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/b34c1bd8-5947-4e1b-923e-08fdb1bb10eb" />
</p>

- I went to the website given here and created a fake account. My username was `thmuser` and password was `123456`
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/86b04129-883c-4f09-bb3e-b0a1e71e963b" />
</p>

- Now that I am here we can see that if I were to click `Claim Reward` I would have to wait another 24 hours but then how do I unlock the vault? We can use Burp Suite's Repeater component to send multiple identical claim requests at the exact same millisecond and the server will perform the `Check` for all of them before any of them finish executing the `Use` action. This bypasses the 24-hour restriction multiple times in a single instant, rapidly inflating the points to unlock the Whale vault

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/e77ad39e-6495-4577-9137-e19f15cb386e" />
</p>

- For this to work, I would need capture the intercepted `POST` request, duplicate these requests and then send them in parallel to the website. I first turned on `Interceptor`
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/c145fba7-898b-4483-8bba-23e619ed16d7" />
</p>

- Then, I first clicked on `Burp` in `FoxyProxy` and then clicked `Claim Reward` to intercept the `POST` request in Burp Suite  
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/43daddb9-8c16-4afd-9a7e-fb84c6f07d32" />
</p>

- We can see in Burp Suite that the `POST` request was captured which is when I clicked the `Claim Reward` button
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/953e9da1-cd73-464f-8e24-866d35712a95" />
</p>

- I now duplicated these requests by right-clicking and selecting `Send to Repeater` but also I repeatedly pressed `Ctrl + R` which was much easier 15-20 times
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/b2d8ac2f-2745-4269-a20c-a874e872ae65" />
</p>

- We can see here that there are 15 of these requests at the top
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/bbc267a0-1391-4af5-8280-675da5a62f06" />
</p>

- I added all 15 requests into a group called `race_condition_attack`
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/a887369c-3a5e-462c-b53f-44804e03f83b" />
</p>

- Finally, I selected from the dropdown `Send group (parallel)` which sent all of these requests simultaneously
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/7298c95a-1ba5-4e9f-ae92-a37c766f0d06" />
</p>

- We can see here in the `Response` section that I was successfully able to unlock the vault
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/bfffb4ed-4caa-4935-a4a3-ffc266ba1ea4" />
</p>

- I went back to the website, turned off `FoxyProxy` and then refreshed the website and clicked `Open Vault` to retrieve the flag
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/168afac2-2322-4f9a-9777-739f2b2fbe12" />
</p>

## Skills Learned

- Identified and exploited a Time-of-Check to Time-of-Use (TOCTOU) race condition
- Analyzed a business logic vulnerability in a web application
- Intercepted HTTP POST requests using Burp Suite
- Executed parallel requests to bypass a server-side cooldown restriction
- Exploited an API endpoint through concurrent request execution
- Verified exploitation by analyzing server responses and application behavior
- Applied race condition testing techniques during web application exploitation

## Tools Used
- Burp Suite
- FoxyProxy
- Firefox
- TryHackMe Attackbox

## Flags

🚩 Flag: `THM{***************************}`
