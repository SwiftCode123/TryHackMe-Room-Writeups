<p align="center">
  <img src="https://assets.tryhackme.com/img/logo/tryhackme_logo_full.svg" width="150" alt="TryHackMe Logo">
</p>

# Wireshark: Packet Operations
|  Room Name | Wireshark: Packet Operations |
|----------|-------|
| Author | Dhruv Tripathi |
| Link | [Wireshark: Packet Operations](https://tryhackme.com/room/wiresharkpacketoperations) |

# Room Information
```bash Type: Walkthrough
Difficulty: Easy
Tags: - 
Meta Tags: Walkthrough, Walk-through, Write-up, Writeup
Subscription type: Premium
Description:
Learn the fundamentals of packet analysis with Wireshark and how to find the needle in the haystack!
```
## Task 1

### Read the task above.

- Answer: `No answer needed`

## Task 2
### Investigate the resolved addresses. What is the IP address of the hostname starts with "bbc"?

- I first opened up the `Exercise.pcapng` file in Wireshark
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/c66727a7-f6b4-415a-9c24-6469a474905e" />
</p>

- I clicked on `Statistics` and there was a dropdown. From that dropdown, I clicked on `Resolved Addresses` and to filter for hostnames starting with `bbc`, I typed `bbc` into the search area and we got the IP address. This way Wireshark shows the IP addresses found in the packet capture and their associated hostnames/DNS names

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/d262bcd5-cd58-4873-9cc3-9c8998547be9" />
</p>

- Answer: `199.232.24.81`

### What is the number of IPv4 conversations?

- For this task, I clicked again on `Statistics` and then from the dropdown, `Conversations` and we can see at the top near `IPv4` where it says the number of conversations. This shows traffic between two endpoints. Wireshark can show conversations for Ethernet, IPv4, IPv6, TCP, and UDP

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/f3845d1d-a514-4f47-ab4c-46fbb613a7bf" />
</p>

- Answer: `435`

### How many bytes (k) were transferred from the "Micro-St" MAC address?

- I first clicked on `Statistics` and then selected `Endpoints` which shows the devices/addresses that appear in the packet capture that are communicating. I then clicked the checkbox for `Name Resolutions` which enables Wireshark to resolve MAC addresses to manufacturer names using the first three bytes of the MAC address. It only identifies manufacturers that are known in Wireshark’s manufacturer database

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/c6efcdf0-6dd3-4879-a47f-cbdd57db063a" />
</p>

- We can then search for `Micro-St` and see the amounts of bytes transferred
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/2fee10d4-718e-495e-9c6e-b80c2ceb349d" />
</p>

- Answer: `7474`

### What is the number of IP addresses linked with "Kansas City"?

- I went to `Statistics` and then `Endpoints` and if we scroll down, we can see how many IP addresses are linked with `Kansas City`. Note that Wireshark can show us where an IP address is located geographically such as the country, city/region and ISP/Organization

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/a5ec32a4-8fe0-48bc-b38c-7825bcd2349f" />
</p>

- Answer: `4`

### Which IP address is linked with "Blicnet" AS Organisation?

- I clicked on `Statistics` and then `Endpoints` and I found the AS organization `Blicnet` listed as `Blicnet d.o.o.` and the associated IP address with it
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/a586759f-1677-41be-bc5a-ee94ccb9c985" />
</p>

- Answer: `188.246.82.7`

## Task 3
### What is the most used IPv4 destination address?

- To view the most used `IPv4 destination address`, I went to `Statistics > IPv4 Statistics > Destinations and Ports`

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/ec8379df-2466-44b7-8975-8a7058752a4e" />
</p>

- I clicked on `Percent` at the top to filter by greatest to least as indicated by the up arrow
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/9ed4c69c-b370-49e7-b875-e01327599c56" />
</p>

- Answer: `10.100.1.33`

### What is the max service request-response time of the DNS packets?

- I went to `Statistics > DNS` and we can see the max service request-response time as indicated by the `Max val` on `Service Stats`
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/1ae919f3-2079-4419-ab3b-698db3c92857" />
</p>

- Answer: `0.467897`

### What is the number of HTTP Requests accomplished by "rad[.]msn[.]com?

- I found `rad.msn.com` but we needed the exact count and no it was not `18`
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/deaf00ba-a08d-4b87-8fde-c3f7ecf051ef" />
</p>

- We can scroll to the right and see the actual number
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/36734726-8cb4-4d37-8366-e3f2de87a1e9" />
</p>

- Answer: `39`
