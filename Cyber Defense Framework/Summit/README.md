<p align="center">
<img src="https://assets.tryhackme.com/img/logo/tryhackme_logo_full.svg" width="150" alt="TryHackMe Logo">
</p>

# Summit
|  Room Name | Summit |
|----------|-------|
| Author | Dhruv Tripathi |
| Link | [Summit](https://tryhackme.com/room/summit) |

# Room Information
```bash Type: Walkthrough
Difficulty: Easy
Tags: - 
Meta Tags: CTF, CTF Challenge, Capture The Flag
Subscription type: Premium
Description:
Can you chase a simulated adversary up the Pyramid of Pain until they finally back down?
```

## Reference

- This is a really good image to describe the `Pyramid Of Pain`
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/a23b000c-c9da-44f1-8d09-f19d879b1e21" />
</p>

## Solution

- First thing I did was go to the website that TryHackMe had given me and read the email

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/102c6ab4-cf57-428f-bfcd-156703ad20e1" />
</p>

- I noticed it said to use the `Malware Sandbox` to scan the file, review the report and find a way to add a detection rule to block it and so that is what I did

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/082be722-3206-4b52-8933-aabbe1979116" />
</p>

- As you can see I selected `sample1.exe` and clicked the button `Submit for Analysis`. Now, remembering from the `Pyramid Of Pain` room and general knowledge, we can block files from executing via their hashes. The hashes are at the lowest tier in the pyramid and so an attacker can just change something in the file to create a completely different hash and so its not really frustrating for an attacker to evade this defense. We can not just rely on hashes

- I copied the hash and pasted it in the `Hash Blocklist` and successfully prevented the file from running

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/75a55922-8f33-4472-8c0e-661f97b89348" />
</p>

- Here, I got the flag
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/c13c14eb-9ac0-4e37-af7e-ecc65913107e" />
</p>

- However, we notice in the email that the attacker now recompiled the malware and generated a new file hash. If we keep blocking the new hash, the attacker can just simply create another one and so on. We have to find another way to block the file from running. I went back to `Malware Sandbox` and uploaded the `sample2.exe` and it generated the report

- I scrolled down to the bottom and here we can see the `Network Activity` section. I noticed that it makes a `GET` request to that particular IP address with that port. Therefore, I would assume we can just block that network connection from happening in the first place. It is most likely making a connection to a C2 server somewhere

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/48af02ab-63e4-4aa3-a2d5-a5131ac65fcd" />
</p>

- I created the firewall rule. Here, `Egress` means exiting so a outbound connection and `Source IP` can be set to `Any` because any computer could be infected and we do not want any connections from any port and since its connecting to that particular IP address we saw in the `Network Activity`, I put the `Destination IP` as that IP address and `Action` was to `Deny`. As we can see when I saved the rule, it was successful in blocking the connection to the C2 server

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/0c582b4f-019c-4cbd-a81f-bfa30a5032df" />
</p>

- Here, I got the flag
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/5d8efeb3-befe-45ca-ba44-c8a35f434124" />
</p>

- However, remembering from the `Pyramid of Pain`, an attacker can just get a new public IP address as stated in the email above. It is not hard for the attacker to evade this defense but it was a bit harder for the attacker considering `IP addresses` are above `Hash Values` in the pyramid

- I went back to Malware Sandbox and uploaded the `sample3.exe` and it generated the report. I scrolled all the way down and we can see some domain names. I noticed that `backdoor.exe` is making a outbound connection to `emudyn.bresonicz.info` over port `80`. Furthermore, `sample3.exe` is also communicating with the malicious domain over port `80` and `1337`.

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/51be2f89-f7d3-4bc8-a35b-a3da04d1a759" />
</p>

- There are also two HTTP requests made by `sample3.exe` to domain. `Xplorita Cloud Services`, seems be hosting the malicious infrastructure for this attack. So, what I am assuming happened is that `sample3.exe` acted as a dropper, ran first and then connected to `http://emudyn.bresonicz.info/backdoor.exe` to download `backdoor.exe` over port 80

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/d34692a6-e26a-41a2-afa3-1144d60b43fe" />
</p>

- Now that I know that this domain is malicious, we can just simply block the domain. This will be higher than `IP addresses` on the `Pyramid Of Pain` making it a bit harder for the attacker to evade this defense as they will have to choose, purchase, and register the domain again. Notice that `emudyn.bresonicz.info` resolves to `62.123.140.9` so we can just block this from happening via DNS filtering

- I used DNS filtering and simply blocked the domain name being resolved to that IP address and it was successful

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/aba86f47-1b62-4ea1-b0af-00ddfd81e043" />
</p>

- Here, I got the flag
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/21904155-6b9e-4f7e-a3d7-a8ec1415545d" />
</p>

- However, like the past three times, the attacker will not give up and we must keep trying. `Host Artifacts` is above `Domain Names` on the pyramid and these are basically traces/clues that malware/attackers leave behind such as registry values, suspicious process execution, IOCs, etc. I went back to `Malware Sandbox` and uploaded the `sample4.exe` and it generated the report

- I scrolled down and we can see the `Registry Activity` here. The attacker changed the registry value to a value of `1` to disable real time monitoring. This means it set to "True" or "Enabled" that real time monitoring is off

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/a09629ba-1a3d-4268-9bcd-646fb810c6d1" />
</p>

- Now, we need a way to detect this attack and so I clicked on the `Sigma Rule Builder` option
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/9f737a4c-7968-4e7b-a39e-0f7504d3caff" />
</p>

- I clicked on `Create Sigma Rule`
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/0c606265-6f7d-4f5c-be67-8dcec64275a9" />

- Out of these four, I clicked on `Sysmon Event Logs` as it can provide information about CLI activity, process creations, network connections, file creation, registry value changes, etc. which is what we want

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/222c7f8e-be62-4afd-8889-2e0e5882c146" />
</p>

- I clicked on `Registry modifications` since that is what we want
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/d1d75e91-cd32-4894-bfe0-07b299711ab3" />
</p>

- I filled in all the values correctly
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/37fea417-571f-44e4-b1d4-e9c581b1d5b0" />
</p>

- We were successfully able to detect this attack making it even harder for the attacker and retrieve the flag
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/961e7df4-796f-4b8a-a121-dbf181b8f4cf" />
</p>

- However, attackers can still leave `Network Artifacts` so we need a way to detect those as well. These can be user-agent string, C2 information, or URI patterns followed by the HTTP POST requests, etc. I generated the report for `sample5.exe` and here, I did notice a lot of HTTP POST requests which makes me think we need to create a Sigma detection rule for this as well

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/e8dbb697-db43-44d5-8393-46ab8c64ec51" />
</p>

- In the image above, I noticed that `sample5.exe` connects to `51.102.10.19:443` to download `beacon.bat` and then `beacon.bat` keeps making POST requests to `51.102.10.19:443`. Now, in the email the attacker gave me a `outgoing_connections.log` file to take a look at and so I did

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/b435f8e5-9c27-4001-ad9b-74bd0d7cca4e" />
</p>

- I noticed the repeated connections from `10.10.15.12` to `51.102.10.19:443` that I had identified in the report and noted down the size as well (`97 bytes`). Also something to note is the frequency of the requests. We can see that each request made from these two IP addresses is in 30 minute intervals

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/ce37c01b-2b35-42ea-89a2-ed1b180814dd" />
</p>

- I clicked on `Sysmon Log Events` because I wanted to detect any network connections made

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/0d384e58-5ce4-4f56-83aa-dace64b6f7e9" />
</p>

- Then on `Network Connections`

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/3e2fc170-c55a-4ab4-b068-19d2067a2707" />
</p>

- I filled in the all values. Note that 30 minutes is equal to 1800 seconds. Both `Remote IP` and `Remote Port` are set to `Any` as the attacker could change those easily. We identified `Size` to be `97` bytes from the `outgoing_connections.log` file and the `ATT&CK ID` is `Command and Control` as this is most likely malware trying to keep a connection to a C2 server

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/c0251c06-575d-4738-9020-c273b8efd468" />
</p>

- We can see the fully generated Sigma rule
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/711787a1-4949-4852-beeb-b41e4337e006" />
</p>

- I was successful in stumping the attacker once again

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/86a6b970-a479-4440-8f03-84cb24ba649d" />
</p>

- However, there is one more layer on the `Pyramid Of Pain` and those are detecting TTPs which are the tactics, techniques and procedures of the attacker. Essentially, the attacker's whole objectives/goals/way of attacking and if we were to detect a TTP, it would be very difficult for the attacker to evade this defense

- This time I was given some commands in the `commands.log` file
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/5bf18d65-0879-406f-a240-9a1157b85adc" />
</p>

- From what I can deduce, the attacker is gathering who has admin privileges in the system (`net localgroup administrator`), `ver` checks OS version, `systeminfo` is a full report of the system architecture, hardware, and OS configuration, `ipconfig /all` is all the network adapter details (IP addresses, DNS servers, etc.), `netstat -ano` is all the active network connections and open ports and lastly, `net start` displays all the Windows services. The attacker appends all of this information to the `exfiltr8.log` file

- Therefore, what we can do is make a Sigma rule that detects this commands specifically the `exfiltr8.log` one because that is the common one

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/9a56fa53-2a04-4496-916c-70371783f08a" />
</p>

- The attacker is running these from the command prompt. The commands we saw in the previous log (like `dir`, `netstat`, and `ipconfig`) are native built-in commands or utilities that run inside the Windows command line environment and the MITRE ATT&CK is `Discovery (TA0007)`

- At last, we were successfully able to detect this TTP and the attacker finally gave up

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/54ac31d9-098f-4a59-8cea-f97f0adcf4d2" />
</p>

## 🚩 Flags

### Question 1
**Flag:** `THM{********************************}`

### Question 2
**Flag:** `THM{********************************}`

### Question 3
**Flag:** `THM{********************************}`

### Question 4
**Flag:** `THM{********************************}`

### Question 5
**Flag:** `THM{********************************}`

### Question 6
**Flag:** `THM{********************************}`
