<p align="center">
  <img src="https://assets.tryhackme.com/img/logo/tryhackme_logo_full.svg" width="150" alt="TryHackMe Logo">
</p>

# Wireshark: Traffic Analysis
|  Room Name | Wireshark: Traffic Analysis |
|----------|-------|
| Author | Dhruv Tripathi |
| Link | [Wireshark: Traffic Analysis](https://tryhackme.com/room/wiresharktrafficanalysis) |

# Room Information
```bash Type: Walkthrough
Difficulty: Medium
Tags: - 
Meta Tags: Walkthrough, Walk-through, Write-up, Writeup
Subscription type: Premium
Description:
Learn the basics of traffic analysis with Wireshark and how to find anomalies on your network!
```
## Task 1

### Read the task above.

- Answer: `No answer needed`

## Task 2

### What is the total number of the "TCP Connect" scans?

- To search for this, I used the below filter. The part `tcp.flags.syn == 1` is where the SYN flag is on, `tcp.flags.ack == 0` is the ACK flag being off and `tcp.window_size > 1024` is the receiver can receive more than `1024` bytes at a time. Essentially, we are looking initial TCP connection attempts with a relatively large receive window

- Another thing is that TCP connect scans usually have a windows size larger than `1024` bytes as the request expects some data due to the nature of the protocol. We only want the attacker’s actions here, so we focus on the initial SYN packet and ignore the ACK/SYN-ACK responses from the target identifying the scanning attempt
```
tcp.flags.syn==1 && tcp.flags.ack == 0 && tcp.window_size > 1024
```
<p align="center">
<img width="1512" height="794" alt="image" src="https://github.com/user-attachments/assets/667588bb-7c5a-49d9-b78d-1a98edcf45eb" />
</p>

- Answer: `1000`

### Which scan type is used to scan the TCP port 80?

- I typed `tcp.port == 80` and here we can see the full three-way handshake which makes me think this is a TCP connect scan type. We have the `SYN`, `SYN-ACK`, `ACK`, and then the `RST` for the first four packets

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/4b3e8a30-2336-4495-a10b-d553140b64d0" />
</p>

- Answer: `TCP Connect`

### How many "UDP close port" messages are there?

- Usually, for UDP closed ports, there are ICMP error messages in response. Knowing this, I used the below query to get my answer. This filter tells us that the destination is unreachable specifically because the port is unreachable otherwise if we did not include `icmp.code==3`, we would get all types of destination unreachable reasons like host unreachable, network unreachable, etc.
```
icmp.type == 3 && icmp.code == 3
```
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/5f8faff9-0957-4856-b936-cbf22ce3dfcd" />
</p>

- Answer: `1083`

### Which UDP port in the 55-70 port range is open?

- This one was a bit tricky but the trick to this is to filter out ports via the below query. This way the destination port is only between those two numbers but there was port `67`, `68`, and `69`
```
udp.dstport >= 55 and udp.dstport <= 70
```
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/b6fca07e-a846-4c9c-b32c-7fedc7317099" />
</p>

- To figure out which port was the correct one, we can see that port `67` and port `69` return with an ICMP error message indicating they are closed while port `68` is the only one that is open
  
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/d94101ee-6cd0-45a5-a7b6-9b412fd6847d" />
</p>

- Answer: `68`

## Task 3
### What is the number of ARP requests crafted by the attacker?

- For this task, I first needed to filter out for ARP requests via `arp.opcode == 1`. When I did that, we notice that there are a lot of ARP requests made by one particular IP address which is `192.168.1.25`. Then, I took a look at who the source MAC address was which was noted as `00:0c:29:e2:18:b4`. We also notice its sent to the destination MAC address of `00:00:00:00:00:00`. This is important to note because in ARP poisoning the attacker pretends to be a fake device on a network and links their own MAC address to the actual IP addresses to trick other devices into send traffic to the attacker's machine. Furthermore, the attacker is clearly trying to find devices on the local network 
  
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/05c02861-de34-4ca0-9d18-158df44128f2" />
</p>

- Now that we know the attacker's MAC address and where the requests are sent, we can filter the requests from the attacker because there may be some legitimate traffic as well. We then get the correct number of displayed packets
```
arp.opcode == 1 && arp.src.hw_mac == 00:0c:29:e2:18:b4 && arp.dst.hw_mac == 00:00:00:00:00:00
```
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/fb37b03f-d682-4d9b-918b-3719455cf7b1" />
</p>

- Answer: `284`

### What is the number of HTTP packets received by the attacker?

- To filter out HTTP packets, we can simply type `http` and we get `92` displayed packets. However, further analyzing, we can see that `192.168.1.12` is sending data to `44.228.249.3` and it keeps going back and forth but one thing that we can notice is the destination MAC address is the attacker's MAC address as we saw from the last task. Therefore, the victim sends information to the attacker and the attacker forwards it to the real router and then the router sends information back to the attacker thinking its the victim computer and essentially we have a man-in-the-middle attack

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/b0bfba1d-f459-49c5-a479-beaaf3ec0348" />
</p>

- Now that we know this, we can filter out the amount of HTTP packets received by the attacker by including the attacker's MAC address in the display filter. We can apply this as a filter first

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/bbaa26e5-175c-479c-b8db-78b686bbab3a" />
</p>

- After applying the filter, we see the correct amount of packets displayed
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/c516fa6c-d9ba-45f3-aeac-11cdbbd06ae7" />
</p>

- Answer: `90`

### What is the number of sniffed username&password entries?

- First thing I did was figure out which site the user was visiting and we can do this by following the `TCP stream` by right-clicking on any packet. It shows the user was visiting `testphp.vulnweb.com`

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/1a0d4754-1630-4fe1-8bee-f977a18961dd" />
</p>

- Now that we know this, we can filter for that site as well as filtering out the `POST` requests because we want to see what the user was inputting. We get `10` displayed packets

```
http.host == testphp.vulnweb.com && http.request.method == "POST"
```

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/5cf55444-8fd1-4bfb-970a-32acbf940fea" />
</p>

- Going through packets `1599`, `1668` and `1791`, we can see the usernames and passwords for each
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/cf90fa01-34b8-4c4c-a7a9-304e643c542b" />
</p>

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/1b669f8b-e8bf-42b1-affb-203f31c9da65" />
</p>

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/600b2dc9-a659-4824-818d-434508d2a4cc" />
</p>

- Answer: `6`

### What is the password of the "Client986"?

- As we know from the last task, the password of `Client986` is in packet `1668`
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/b8a05bcc-9b14-4112-b196-7f1dbcc0477f" />
</p>

- Answer: `clientnothere!`

### What is the comment provided by the "Client354"?

- We can see the comment made in packet `2320` by `Client354`
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/c5fca89e-cc5e-4a88-be39-6457f8d4f06e" />
</p>

- Answer: `Nice work!`

## Task 4
### What is the MAC address of the host "Galaxy A30"?

- We can use the filter `dhcp.option.hostname contains "A30"` and I only saw one packet which contained the MAC address of the hostname. Furthermore, we can see the actual hostname as well
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/a1f2e56c-36e5-4670-be0b-070e02b09483" />
</p>

- Answer: `9a:81:41:cb:96:6c`

### How many NetBIOS registration requests does the "LIVALJM" workstation have?

- We can use `nbns.name contains "LIVALJM" && nbns.flags.opcode == 5`. The first part `nbns.name contains "LIVALJM"` filters for the `LIVALJ` workstation and `nbns.flags.opcode == 5` gives us only name registration packets

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/f1c5f223-9056-4a37-afdc-7d19ee60f517" />
</p>

- Answer: `16`

### Which host requested the IP address "172.16.13.85"?

- If we filter for DHCP requests only via the filter `dhcp.option.requested_ip_address == 172.16.13.85`, we see only one packet `72529` which requested the IP address `172.16.13.85` and the hostname listed underneath `Option: (12) Host Name`

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/d2ae41d4-bd16-4c4a-8a58-8b5a2b19134d" />
</p>

- Answer: `Galaxy-A12`

### What is the IP address of the user "u5"? (Enter the address in defanged format.)

- I used the filter `kerberos.CNameString contains "u5"` and we can confirm that the `CNameString` for `10.1.12.2` is `u5` and know we know the IP address as well

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/08ff6038-0e99-4409-a22b-86044eda106c" />
</p>

- Answer: `10[.]1[.]12[.]2`

### What is the hostname of the available host in the Kerberos packets?

- To find the hostnames, we can apply the `CNameString` as a column
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/8ebbbbd2-b9df-4eca-8346-92ce640715c4" />
</p>

- Here, we find the hostname aka the computer that was generating Kerberos traffic
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/e287842d-cb9f-481f-ae98-633ec3c1d316" />
</p>

- Answer: `xp1$`

## Task 5

### Use the "Desktop/exercise-pcaps/dns-icmp/icmp-tunnel.pcap" file. Investigate the anomalous packets. Which protocol is used in ICMP tunnelling?

- For this one, I first filtered out ICMP traffic with a data length greater than 64 bytes, since ICMP packets are typically small. An unusually large ICMP payload could indicate that additional data is being smuggled or tunneled through ICMP traffic
```
data.len > 64 and icmp
```
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/0c041573-cb39-4afb-8168-6dad5049d3ac" />
</p>

- Now here I just started scrolling down the packets and watched if there was anything suspicious in the packet details or packet bytes panel and then I saw that SSH data/traffic is being smuggled over ICMP

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/0a1ac234-7224-412f-9d02-b20da0889458" />
</p>

- We can even see more SSH related traffic in the next packet
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/0e928aa0-acc2-467e-b81b-f2c85102f6e2" />
</p>

- Answer: `SSH`

### Use the "Desktop/exercise-pcaps/dns-icmp/dns.pcap" file.Investigate the anomalous packets. What is the suspicious main domain address that receives anomalous DNS queries? (Enter the address in defanged format.)

- I tried `dns contains "dnscat"` but I didn't really see anything related to an actual domain name
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/d7933be4-2596-4d51-8101-409c7b468122" />
</p>

- However, knowing that there can be long DNS addresses I tried to filter for the length of the name and we can see there is a suspicious domain here which is most likely our answer
```
dns.qry.name.len > 50 && !mdns
```
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/c040a884-61b8-4cd8-af70-c22a3cc64378" />
</p>

- Answer: `dataexfil[.]com`

## Task 6
### How many incorrect login attempts are there?

- We can use the filter below to list all the failed login attempts
```
ftp.response.code == 530
```

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/47c8721b-9da6-4ab6-a978-e213bde51c2f" />
</p>

- Answer: `737`

### What is the size of the file accessed by the "ftp" account?

- We can see if any user is logged in and yes there was one
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/619f72c3-2e16-4ccd-be08-2790bed471c3" />
</p>

- Following the `TCP Stream`, we can confirm this was the `ftp` account and the file size as indicated underneath `SIZE resume.doc`
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/ffa74e3b-eeec-49a1-bd16-d77126975255" />
</p>

- Answer: `39424`

### The adversary uploaded a document to the FTP server. What is the filename?

- Now technically, we can see here that the attacker uploads a `README` file via the `STOR README`. However, I believe THM wanted `resume.doc` to be the answer because technically we can see that the attacker retrieves the file via `RETR resume.doc` but its possible maybe the attacker did something after with this file

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/11684950-fa8d-423e-80fd-1a499777515b" />
</p>

- Answer: `resume.doc`

### The adversary tried to assign special flags to change the executing permissions of the uploaded file. What is the command used by the adversary?

- We can see the commands used below but the attacker was denied permissions
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/00b5697c-8418-4146-9d20-60c2c2f1b668" />
</p>

- Answer: `CHMOD 777`

## Task 7
### Investigate the user agents. What is the number of anomalous  "user-agent" types?

- For this one, I just scrolled down the `.pcap` file and noted the weird user-agents as asked. There was the JNDI (Log4j exploit), Windows 6.4 which was never released as an OS, Wfuzz, sqlmap, Mozlila, and Mozilla with nmap scripting 

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/6ee46e71-01fa-456c-9aa1-f04a1e4dbed7" />
</p>


<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/3d6953d5-7c73-4136-9b3e-817408832b15" />
</p>

- Answer: `6`

### What is the packet number with a subtle spelling difference in the user agent field?

- There is only one packet with the subtle misspelling
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/8e8d0704-68d3-4089-9e9d-f82c96cf44f2" />
</p>

- Answer: `52`

### Locate the "Log4j" attack starting phase. What is the packet number?

- Filtering for the Log4j attack starting phase, we can see what packet number it is
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/74550297-2b44-488b-bd39-5d594d5fa9bc" />
</p>

- Answer: `444`

### Locate the "Log4j" attack starting phase and decode the base64 command. What is the IP address contacted by the adversary? (Enter the address in defanged format and exclude "{}".)

- We can copy and paste the Base64 encoded command from here into Cyberchef
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/747575fc-9d12-478b-bad9-96e6cbf76f70" />
</p>

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/01427f51-167a-4513-9b7b-c5751cfcada4" />
</p>

- Answer: `62[.]210[.]130[.]250`

## Task 8
### What is the frame number of the "Client Hello" message sent to "accounts.google.com"?

- For this task, we can use the filter `(http.request or tls.handshake.type == 1) and !(ssdp)` which shows HTTP traffic and the start of a TLS/HTTPS connection, but excludes SSDP traffic. However, note that I didn't get the server names but we can apply that as a column

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/88fa813d-5154-4fc4-9fec-b7595d0d29b5" />
</p>

- With applying that as a column, we can see the frame number properly
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/47117c2e-878d-4d10-9168-3458b795410c" />
</p>

- Answer: `16`

### Decrypt the traffic with the "KeysLogFile.txt" file. What is the number of HTTP2 packets?

- First, we need to actual give Wireshark the `KeysLogFile.txt` file and we can do this via `left-click` on any packet, then `Protocol Preferences` and then `(Pre)-Master-Secret log filename...`

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/0e818d97-01b7-468d-b5db-247f07210d06" />
</p>

- We can find `TLS` and then upload the `KeysLogFile.txt` file
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/841c54b9-a1c1-4b2f-80b8-a7d29a31b9ce" />
</p>

- I typed `http2` and we get the correct number of displayed packets
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/599bed4e-af25-4d45-a124-6b2922c592e4" />
</p>

- Answer: `115`

### Go to Frame 322. What is the authority header of the HTTP2 packet? (Enter the address in defanged format.)

- We can see the authority header in frame `322`
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/d2164763-abc5-4a58-875d-68493d70a685" />
</p>

- Answer: `safebrowsing[.]googleapis[.]com`

### Investigate the decrypted packets and find the flag! What is the flag?

- For this one, I first typed `http2` to see the packets and we can see that there is a outbound request to `flag.txt`
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/f79e9b35-3a79-4c85-8d7f-205af9a02e25" />
</p>

- Since the traffic is already decrypted, I can use Wireshark’s `Export Objects` feature to extract the file directly instead of manually following the packets. I went to `File → Export Objects → HTTP` and looked for the downloaded file. I then clicked `Save`

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/8722ae3b-e298-4142-b555-71e1963847c1" />
</p>

- Opening up the file, we can see the flag
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/73f974ae-98c7-4a6c-83eb-e2b6ad27ed2c" />
</p>

- Answer: `FLAG{THM-PACKETMASTER}`

## Task 9
### Use the "Desktop/exercise-pcaps/bonus/Bonus-exercise.pcap" file. What is the packet number of the credentials using "HTTP Basic Auth"?

- I went to `Tools > Credentials` and we can see the packet number here for `HTTP Basic Auth`
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/03bd5171-939d-43aa-89f7-81bab003a88e" />
</p>

- Answer: `237`

### What is the packet number where "empty password" was submitted?

- Clicking on each packet number one by one, we can see once we get to packet number `170`, there is no password after `PASS`. For the other packets, there was a password provided after `PASS` such as `Request: PASS nancy` for packet number `126`
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/966a6b0d-800a-4eb5-a074-6174848e74d3" />
</p>

- Answer: `170`

## Task 10

### Use the "Desktop/exercise-pcaps/bonus/Bonus-exercise.pcap" file. Select packet number 99. Create a rule for "IPFirewall (ipfw)". What is the rule for "denying source IPv4 address"?

- I went to packet `90` and then went to `Tools > Firewall ACL Rules` and then I selected the appropriate firewall system (`IPFirewall (ipfw)`) we can see the rule name for denying IPv4 source address

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/f7ea1ef4-405d-4417-ba1f-04cfbb51d06b" />
</p>

- Answer: `add deny ip from 10.121.70.151 to any in`

### Select packet number 231. Create "IPFirewall" rules. What is the rule for "allowing destination MAC address"?

- I did the same exact thing here except unchecking the box `Deny` since we want to allow traffic to pass

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/bdc81a4d-8c76-43c1-8ec9-79b1bf9faef2" />
</p>

- Answer: `add allow MAC 00:d0:59:aa:af:80 any in`

## Skills Learned

* Developed practical experience using Wireshark display filters to isolate specific network traffic
* Improved my ability to identify TCP Connect scans by analyzing SYN, ACK, and window size values
* Gained a better understanding of TCP and UDP scanning techniques and how they appear in packet captures
* Practiced identifying closed UDP ports through ICMP destination unreachable messages
* Built experience detecting ARP poisoning and potential man-in-the-middle activity
* Strengthened my ability to trace attacker traffic using MAC addresses and packet characteristics
* Practiced analyzing HTTP requests to uncover usernames, passwords, and submitted data
* Used TCP stream analysis to investigate network conversations and extract useful information
* Improved my ability to investigate DHCP, NetBIOS, and Kerberos traffic to identify hosts and users
* Gained experience detecting ICMP tunneling and recognizing SSH traffic hidden within ICMP packets
* Practiced identifying suspicious DNS queries that could indicate tunneling or data exfiltration
* Developed skills for investigating FTP activity, including failed logins, file transfers, and attacker commands
* Improved my ability to recognize suspicious user agents associated with security tools and exploitation attempts
* Practiced investigating Log4j exploitation activity and decoding Base64 encoded commands
* Gained experience identifying TLS Client Hello messages and determining the requested host
* Practiced decrypting TLS traffic in Wireshark using a key log file
* Used decrypted HTTP2 traffic to investigate requests and extract files from packet captures
* Practiced identifying HTTP Basic Authentication credentials and empty password submissions
* Gained familiarity with creating IPFirewall rules based on captured network traffic

## Conclusion

This room provided hands-on practice with Wireshark and showed how packet captures can be used to investigate network activity, identify attacks, and uncover suspicious behavior. The exercises covered a wide range of techniques including network scanning, ARP poisoning, credential sniffing, protocol analysis, tunneling, DNS exfiltration, FTP abuse, Log4j exploitation, and encrypted traffic analysis. I also became more comfortable with advanced Wireshark features such as TCP stream following, custom display filters, packet field columns, TLS decryption, file extraction, and firewall rule generation. Overall, the room strengthened my ability to investigate network traffic and use packet-level evidence to understand what happened during a security incident
