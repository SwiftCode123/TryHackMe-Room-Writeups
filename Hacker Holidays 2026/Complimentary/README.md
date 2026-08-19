<p align="center">
  <img src="https://assets.tryhackme.com/img/logo/tryhackme_logo_full.svg" width="150" alt="TryHackMe Logo">
</p>

# Complimentary

| Room Name | Complimentary |
|-----------|---------------|
| Platform | TryHackMe |
| Event | Hacker Holidays 2026 |
| Points | 60 |
| Category | ☁️ Cloud |
| Difficulty | Easy |
| Tags | AWS, Cloud, Cognito, IAM Misconfiguration, DynamoDB |
| Status | ✅ Completed |

---

## Room Information

```bash
Type: CTF Challenge  
Difficulty: Easy  
Category: Cloud

Description:
Install the free app and it hands your phone a set of cloud keys, the same set it hands everyone. They're read-only, but read-only of every guest's contacts, location, and passwords, not just Lambo's. She gave consent. Technically.
```
---

## Challenge Overview

**Hacker Holidays: Day 3 — Complimentary**

The Byte Lotus Wellness app relies on AWS cloud services to provide seamless access without requiring users to authenticate. However, an underlying IAM misconfiguration allows the application to expose more information than intended.

By investigating the application's AWS configuration and understanding how temporary credentials are assigned, you'll discover that the permissions extend beyond the current user, ultimately leading to the challenge flag.

## Objectives

- Track down the AWS mechanism issuing you credentials behind the scenes.
- Use those credentials to dump more than your own record from the app's DynamoDB table.
- Retrieve the flag from another guest's data.

## Solution
- First thing I did was look at the challenge description and it says `It just… knows things about you the moment you open it.` which makes me think that how does the website know who I am if I have never logged in
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/516e595d-9d08-400d-959e-ce055be1d9c1" />
</p>

- This comment even stated that `something has to be quietly handing it access behind the scenes...`
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/102c4fc6-8009-4db6-87ea-6a2eaaae439a" />
</p>

- Also, we know this has to do with AWS, Cognito and IAM misconfigurations
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/47c7f67a-7f1e-482b-b0c8-2d9ac00dd52c" />
</p>

- Now, I went to the website they gave me and my first instinct was to open up the developer's tool and navigate to `Sources` because it is completely possibly that sometimes developers might leave important important in the code and I was right!

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/b9e359e5-8ece-4748-aae1-c234b482f420" />
</p>

- We can see the `AWS_REGION`, which is the specific AWS region being used, the `IDENTITY_POOL_ID`, which tells us what specific Cognito identity pool should be used and that specific identity pool is used to obtain temporary AWS credentials based on an IAM role, and the `TABLE_NAME`, which holds all of the information we want

- Now, instead of using the website which I didn't really find anything to interact with, I instead used my terminal. There was an AttackBox inside TryHackMe but I personally found it easier this way. After researching a bit about how to use AWS CLI, I stumbled upon this command

```
aws cognito-identity get-id \
    --identity-pool-id <IDENTITY_POOL_ID> \
    --region <AWS_REGION>
```
- This command uses the AWS CLI and talks to the AWS Cognito. The `get-id` portion asks for a new anonymous guest identity. `--identity-pool-id` tells which guest pool to use and `--region` tells which AWS region to use. Now, I already had the `--identity-pool-id` and the `--region` as you saw in the website so I input those into my terminal and we get an identity ID back

```
aws cognito-identity get-id \
  --identity-pool-id "us-east-1:836c0949-292d-485b-b532-52d5ca7bb688" \
  --region us-east-1
```
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/1c71e607-e194-4bce-9901-debf0fa5bb65" />
</p>

- We have our ID but we do not have any temporary AWS credentials per se. Therefore, I found another command
```
aws cognito-identity get-credentials-for-identity \
    --identity-id <IDENTITY_ID> \
    --region <AWS_REGION>
```
- What this command is doing is getting those temporary AWS credentials/access keys. I have my `--identity-id` and `--region` and I can use those here

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/55a653eb-5196-422e-b00d-452104bb2612" />
</p>

- I successfully got my AWS credentials. Now, one more thing I have to do use `export` and what this does is every future AWS command will act as the guest account instead of my normal account and it will use these specific AWS credentials unless I close my terminal
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/9fb520f0-2256-490b-9e61-dd055bab8f2b" />
</p>

- Finally, I can use the guest credentials to read everything from the table and in return retrieve the flag
```
aws dynamodb scan \
    --table-name complimentary-GuestWellnessProfiles \
    --region <AWS_REGION>
```
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/8851a80f-f04e-49a9-80da-efe90836552b" />
</p>

- This allowed me to talk to the DynamoDB database and the `scan` was the most important part as it meant, as stated before, I could read everything

- Normally, guests should only be able to read their own profile and because of some IAM misconfiguration, AWS allowed the guest account to run `dynamodb:Scan` which should never be allowed to anonymous users and instead should use `dynamodb:GetItem` which retrieves one specific record not the entire table

## Skills Learned
- Identifying exposed AWS configuration values in client-side JavaScript
- Working with Amazon Cognito Identity Pools
- Obtaining and using temporary AWS credentials via Cognito
- Authenticating AWS CLI sessions with exported environment variables
- Querying DynamoDB using temporary IAM credentials
- Identifying IAM misconfigurations that permit unintended `dynamodb:Scan` access

## Tools Used
- Browser Developer Tools (JavaScript inspection)
- AWS CLI
- Amazon Cognito Identity
- Amazon DynamoDB
- Terminal (Bash)

## Flag

🚩 **Flag:** `THM{****************}`
