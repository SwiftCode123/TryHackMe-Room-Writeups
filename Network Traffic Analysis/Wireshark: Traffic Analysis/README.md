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
