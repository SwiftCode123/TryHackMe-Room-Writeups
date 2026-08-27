<p align="center">
  <img src="https://assets.tryhackme.com/img/logo/tryhackme_logo_full.svg" width="150" alt="TryHackMe Logo">
</p>

# NetworkMiner
|  Room Name | NetworkMiner |
|----------|-------|
| Author | Dhruv Tripathi |
| Link | [NetworkMiner](https://tryhackme.com/room/networkminer) |

# Room Information
```bash Type: Walkthrough
Difficulty: Easy
Tags: - 
Meta Tags: Walkthrough, Walk-through, Write-up, Writeup
Subscription type: Premium
Description:
Learn how to use NetworkMiner to analyze recorded traffic files and practice network forensics activities.
```
## Task 1

### Read the task above.

- Answer: `No answer needed`

## Task 2

### Read the task above.

- Answer: `No answer needed`

## Task 3
### Read the task above.

- Answer: `No answer needed`

## Task 4
### Use the "~/Desktop/Exercise Files/mx-3.pcap" file to answer the questions below. What is the total number of frames?

- For this one, we can actually view the metadata of the file to view the total number of frames
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/5c5ab713-4e1f-4a34-952e-40db80f86802" />
</p>

- We can see the number of frames as listed below
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/eaffab01-eaf6-4cc7-8d25-19521df4125d" />
</p>

- Answer: `460`

### How many IP addresses use the same MAC address with host 145.253.2.203?

- If we click on the host `145.253.2.203`, we can see how many IP addresses there are with the same MAC address
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/116fadc4-7deb-4a68-8b88-265cb253b90d" />
</p>

- Answer: `2`

### How many packets were sent from host 65.208.228.223?

- If we click on host `65.208.228.223`, we can see how many packets were sent from this host
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/d7eecc2b-ac68-45c9-a6b0-8f2bb1a3db2a" />
</p>

- Answer: `72`

### What is the name of the webserver banner under host 65.208.228.223?

- We can expand the `Host Details` portion to see the name of the webserver banner
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/c917e551-a429-4e6e-9f37-10df6b768ff7" />
</p>

- Answer: `Apache`

### Use the "~/Desktop/Exercise Files/mx-4.pcap" file to answer the questions below. What is the extracted username for the 02694W-WIN10 host?

- The extracted username is seen below under the `Username` column
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/3195166a-ef92-4642-a09d-9558c0192dbc" />
</p>

- Answer: `#B\Administrator`

### What is the extracted password for the user logged into the 02694W-WIN10 host? Enter the full NTLM hash.

- The password was under the `Password` column. Note that it was very long as seen below
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/ebe88e16-d594-449e-9f5a-7b736eed284f" />
</p>

- Answer: `$NETNTLMv2$#B$136B077D942D9A63$FBFF3C253926907AAAAD670A9037F2A5$01010000000000000094D71AE38CD60170A8D571127AE49E00000000020004003300420001001E003000310035003600360053002D00570049004E00310036002D004900520004001E0074006800720065006500620065006500730063006F002E0063006F006D0003003E003000310035003600360073002D00770069006E00310036002D00690072002E0074006800720065006500620065006500730063006F002E0063006F006D0005001E0074006800720065006500620065006500730063006F002E0063006F006D00070008000094D71AE38CD601060004000200000008003000300000000000000000000000003000009050B30CECBEBD73F501D6A2B88286851A6E84DDFAE1211D512A6A5A72594D340A001000000000000000000000000000000000000900220063006900660073002F003100370032002E00310036002E00360036002E0033003600000000000000000000000000`

