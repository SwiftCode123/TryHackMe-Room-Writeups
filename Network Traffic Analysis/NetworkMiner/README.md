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

## Task 6
### Which version can detect duplicate MAC addresses?

- Version 2.7 can identify MAC address conflicts while version 1.6 can not
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/11ceed46-0e1d-4051-8c37-1a4160f93d47" />
</p>

- Answer: `2.7`

### Which version can handle frames?

- Version 1.6 provides the number of frame and details about the frames

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/9d9cc531-7fb1-494c-8ddb-486f3c66421d" />
</p>

- Answer: `1.6`

### Which version can provide more details on packet details?

- Version 1.6 and below can handle packets in more detail
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/673dbb9c-b442-4569-b461-c5044e6e2898" />
</p>

- Answer: `1.6`

## Task 7
### Use the ~/Desktop/Exercise Files/case1.pcap file to answer the questions below. What is the full OS name of the host 131.151.37.122?

- Opening up the file, I clicked on the `+` icon next to `131.151.37.122` and then the `+` icon next to `OS: Windows` to see the hostname
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/9e2bc13d-549c-4418-ad05-27180f1de528" />
</p>

- Answer: `Windows - Windows NT 4`

### Investigate the hosts 131.151.37.122 and 131.151.32.91. How many bytes were sent by the client (*.32.91) through port 1065?

- If we click on the `+` next to `131.151.37.122` and then the `+` icon next to `Incoming sessions: 2` and then the `+` icon next to `Server: 131.151.37.122 (Windows) TCP 1065`, we can see how many bytes were sent by `131.151.32.91` through port `1065`
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/791ad575-d140-44c4-9b09-37902b7cbc51" />
</p>

- Answer: `192`

### Investigate the communication between 131.151.37.122 and 131.151.32.21. How many bytes were sent back by the server (*.37.122) through port 143?

- We can repeat similar steps appear like the last question and we see the number of bytes sent by the server through port `143`
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/563394fc-63c3-459e-9232-6eb372412ca4" />
</p>

- Answer: `20769`

### What is the sequence number of frame number 9?

- For this question, we switch to a different version of NetworkMiner namely `NetworkMiner 1.6.1` to see the sequence number of frames
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/d08cec10-ea55-4a16-84c0-7dbd728fb8cf" />
</p>

- Answer: `2AD77400`

### What is the number of the detected "content types"?

- I went into the `Parameters` tab and typed in the filter `Content-Type` and set the dropdown next to `ExactPhrase` to `Parameter name` and we see there is only `2` types of content which are `text/plain` and `multipart/mixed`

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/e478d903-4412-4b98-b7c7-92261675c5a4" />
</p>

- Answer: `2`

### Use the ~/Desktop/Exercise Files/case2.pcap file to answer the questions below. What is the USB product's brand name?

- For this one, we can go to the `Files` tab and filter for `USB`. Clicking on the first one and opening up the file, we can see the USB product's brand name in the HTML code
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/e72a0b67-7ab1-427a-b0a9-e555b0187ef1" />
</p>

- Answer: `ASIX`

### What is the name of the phone model?

- Under the `Images` tab, I was scrolling for a while until I found this image named `Store_Lumia_Nav` which is a phone
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/17d4fe88-3192-4e89-9ded-85ef8621dd1b" />
</p>

- We can go back into the `Files` tab and see filter for `Lumia` and see two options with the first one being the phone name
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/74bc18de-f0b8-4af5-b0cc-931c22a4f54c" />
</p>

- Answer: `Lumia 535`

### What is the source IP of the fish image?

- For this one, we can search for the image name of the fish under the `Files` tab and the name was `Crazy-Fishing[1].jpg` along with the IP address listed next to `Source host`
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/db1bdd68-2acc-426b-8840-67db7acfd018" />
</p>

- Answer: `50.22.95.9`

### What is the password of the `homer.pwned.se@gmx.com`?
- Under the `Parameters` tab, we can filter for `homer.pwned.se@gmx.com` and looking here, we see multiple lines with `homer.pwned.se@gmx.com` indicating that this user is connecting to a GMX email server using a standard email protocol (TCP port `110`, which is used for POP3 email). It shows data traveling between an external mail server IP address (`212.227.17.171`) and a local computer on the network (`192.168.0.51`)

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/d36dce32-7dc7-4f43-a048-688e44d9eb8e" />
</p>

- Clicking on the `Credentials` tab, we can see the user's exact email, protocol (`Pop3`), the server the user is connected to and the password

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/65c0b6d4-bd36-4d9e-89b0-1f87f6c0f23c" />
</p>

- Answer: `spring2015`

### What is the DNS query of frame 62001?

- This one was a bit simpler as we can go to the `DNS` tab and filter for the frame number `62001` and see the DNS query made
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/285197ce-4bdd-4226-a391-67fc6fb7c2ad" />
</p>

- Answer: `pop.gmx.com`

## Skills Learned

* Analyzed PCAP files in NetworkMiner to investigate captured network traffic
* Identified frame counts, packet activity, MAC addresses, and host information
* Investigated hosts to uncover operating systems, web server banners, and network sessions
* Extracted usernames, NTLMv2 hashes, email addresses, and other credentials from captured traffic
* Used filters for frame numbers, filenames, parameters, and protocols to quickly locate evidence
* Traced files and images back to their source IP addresses
* Identified Linux distributions, Windows versions, USB brands, and mobile phone models from network artifacts
* Investigated emails, DNS queries, POP3 traffic, and TLS anomalies
* Compared NetworkMiner versions to understand differences in packet and MAC address analysis
* Examined network sessions and ports to determine how much data was transferred
* Used multiple artifacts together to piece together information during a network forensic investigation

## Conclusion

This room provided hands-on experience using NetworkMiner to analyze PCAP files and extract useful forensic evidence. It demonstrated how network captures can reveal information about hosts, users, credentials, files, emails, DNS queries, and network communications. Overall, the room improved my ability to navigate captured traffic, use filters effectively, and connect different network artifacts to investigate activity
