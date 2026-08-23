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
Difficulty: Easy
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

## Task 2
### What is the number of ARP requests crafted by the attacker?

- For this task, I first needed to filter out for ARP requests via `arp.opcode == 1`. When I did that, we notice that there are a lot of ARP requests made by one particular IP address which is `192.168.1.25`. Then, I took a look at who the source MAC address was which was noted as `00:0c:29:e2:18:b4`. We also notice its sent to the destination MAC address of `00:00:00:00:00:00`. This is important to note because in ARP poisoning the attacker pretends to be a fake device on a network and links their own MAC address to the actual IP addresses to trick other devices into send traffic to the attacker's machine. Furthermore, the attacker is clearly trying to find devices on the local network 
  
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/05c02861-de34-4ca0-9d18-158df44128f2" />
</p>

- Now that we know the attacker's MAC address and where the requests are sent, we can filter the requests from the attacker because there may be some legitimate traffic as well. We then get the correct number of displayed packets

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

- Answer `6`

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
