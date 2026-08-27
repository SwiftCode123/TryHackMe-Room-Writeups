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

## Task 5

### Use "~/Desktop/Exercise files/mx-7 pcap" file to answer the questions below. What is the name of the Linux distro mentioned in the file associated with frame 63602? (Note: If there are no results associated with this frame, check out frame 63075)

- Instead of manually scrolling and finding packet `63602`, we can click on the dropdown next to `ExactPhrase` and select `Frame nr.` but when I typed this packet name I didn't find anything. As the question stated, we didn't find anything associated with packet `63602`, we can search for packet `63075` which is what I did and double clicking the file, we see the Linux distro

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/0459a73e-00c1-4e51-9fbb-f129c41166d3" />
</p>

- Answer: `CentOS`

### What name and surname are mentioned in the file associated with frame 76469? (Note: If there are no results associated with this frame, check out frame 75942)

- Same issue happened with this one where I didn't find anything associated with packet `76469` but worked with packet number `75942` instead and we can see the name and surname when double clicking on the file

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/67f8f637-0b3e-4589-a0f7-d2394b334481" />
</p>

- Answer: `Ned Flanders`

### What is the source address of the image "ads.bmp.2E5F0FD9[1].bmp"?

- Here, we can search for the filename and see the source address. Note that we had to select `Filename` in the dropdown next to `ExactPhrase`

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/b87526ac-cb37-43af-ac49-436400b864d6" />
</p>

- Answer: `80.239.178.187`

### What is the frame number of the possible TLS anomaly?

- When we click on the `Anomalies` tab, there are two frame numbers and both are correct
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/b3ba3cf2-d67e-4c3e-a312-1402d3e82aea" />
</p>

- Answer: `36255`
- Answer: `73073`

### Use "~/Desktop/Exercise files/mx-9" file to answer the questions below Which platform sent an email with the subject starting with "You have more..."?

- Under the `Messages` tab, we can see where the email is from
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/ec1c9176-64c6-4765-bbe6-7ade7c54ebf7" />
</p>

- Answer: `Facebook`

### What is the email address of Branson Matheson?

- I typed in his first name into the filter and if we click on the first row, we can see his email next to the `From` row
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/3a1a8e78-6882-4644-af8b-d48fb41b7d0f" />
</p>

- Answer: `branson@sandsite.org`
