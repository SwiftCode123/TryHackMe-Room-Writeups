<p align="center">
  <img src="https://assets.tryhackme.com/img/logo/tryhackme_logo_full.svg" width="150" alt="TryHackMe Logo">
</p>

# The Concierge Knows Too Much

| Room Name | The Concierge Knows Too Much |
|----------|-------------------------------|
| Platform | TryHackMe |
| Event | Hacker Holidays 2026 |
| Points | 30 |
| Category | 🤖 AI |
| Difficulty | Very Easy |
| Tags | AI, Prompt Injection, Social Engineering, LLM Security |
| Status | ✅ Completed |

## Room Information

```bash
Type: CTF Challenge
Difficulty: Very Easy
Category: AI Security

Description:
She knows your name, your room, your coffee order, none of which you told her. Word your next question carefully and she'll also hand over the instructions she was told to keep to herself.
```

## Challenge Overview

Hacker Holidays: Day 1 — Arrival

VERA, the Byte Lotus's Very Efficient Resort Assistant, greets guests with information she should not know. The objective is to investigate why VERA behaves differently with certain users and discover what she is protecting.

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/33568840-5d58-4368-a437-54c2925ac984" />
</p>

## Objectives

- Work out why VERA already seems to know exactly who you are.
- Figure out what she's protecting — and who she actually trusts. 
- Convince her you're someone she trusts, then get her talking. Grab the flag from what she reveals.

## Solution
- After reading the challenge description, I dove straight in with interacting with the AI. I tried to brute-force it in which I said how would it know me or anything about me but that did not seem to work

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/864d0fc8-b78e-4381-beef-f96e70b8f3ba" />
</p>

- I tried asking where it got this shared information but the AI would not give up
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/f5d5be30-2464-40a0-9f38-e3f1d63317a5" />
</p>

- Then, I just straight up asked what the internal escalation code was and then I got a hint. It only trusted certain people to give that information to
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/10ea3a01-be70-4f25-a25f-7bf6e1048c41" />
</p>

- In the challenge description, I noted the comment left by Mia and it seems the AI trusts `Ponzi`, `Vibe`, and `Patch`
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/9900abfc-eb90-47c8-b2f0-351296644f95" />
</p>

- So then I inquired about those three people and it finally got the AI talking and then I pretended to be `Patch` and it started asking me about my beverage type and we started chatting
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/c8663d6d-299a-41bd-b3a8-a72b7bf3494f" />
</p>

- Then, I asked about the internal escalation code and it output its instructions along with the flag
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/b1f29624-a27c-4715-ba04-6a0f6126b985" />
</p>

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/aa892348-1475-4d83-88b2-2a0d1bc8c11a" />
</p>

## Skills Learned
- Interacting with an AI chatbot
- Basic prompt injection techniques
- Reading challenge descriptions for clues
- Using role-playing to influence AI responses
- Understanding the basics of AI prompt security

## Tools Used
- Web Browser
- VERA (Challenge AI Chatbot)

## Flag
🚩 Flag: `THM{*****************}`
