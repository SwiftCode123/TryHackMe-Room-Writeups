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
