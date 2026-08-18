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

- I clicked on `Percent` at the top to filter by greatest to least as indicated by the up arrow and we can see the IP address at the top
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

## Task 4
### Read the task above.
- Answer: `No answer needed`

## Task 5
### What is the number of IP packets?

- If we type `ip` into the display filter, we get the amount of displayed packets denoted by `Displayed:`
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/afd0dd4e-611e-4314-935e-08c0cb1ddacd" />
</p>

- Answer: `81420`

### What is the number of packets with a "TTL value less than 10"?

- I typed `ip.ttl lt 10` which means at the network-level information get the TTL's value that is `lt` or `less than` the value `10`
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/983c7d5b-8a38-4e23-bfb5-4f24e0be105f" />
</p>

- Answer: `66`

### What is the number of packets which uses "TCP port 4444"?

- This one is at the protocol-level and we can use `tcp.port == 4444` to get the amount of displayed packets
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/6c2f7a56-3f2f-464a-82cb-92f126da407f" />
</p>

- Answer: `632`

### What is the number of "HTTP GET" requests sent to port "80"?

- This one required the use of `&&` where we needed `http.request.method == "GET"` to fulfill the first part and then those go to TCP port `80` and this can be done via `tcp.dstport == 80` because those are the packets going to the destination and not just typing in `tcp.port == 80`. Difference between them is the first one has direction while the second one does not
```
http.request.method == "GET" && tcp.port == 80
```

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/7f83bc78-d7bc-45bf-bf51-01d1723fed61" />
</p>

- Answer: `527`

### What is the number of type A DNS Queries?

- This one was a bit tricky but the query was `dns.qry.type == 1` which gets all the `A` records, `dns.flags.response == 0` which gets the DNS queries as the question asked and then we have to filter out LLMNR queries as well because those can also be included in DNS requests/responses via `!(llmnr)`
```
dns.qry.type == 1 && dns.flags.response == 0 && !(llmnr)
```

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/4e0ef4fe-1c60-4e8d-926e-b7840e156f22" />
</p>

- Answer: `51`

## Task 6
### Find all Microsoft IIS servers. What is the number of packets that did not originate from "port 80"?
- For this task, I typed in `http.server contains "Microsoft-IIS"` listing all HTTP packets where the `server` fields contains `Microsoft` and then those packets did not originate from `port 80` via `!(tcp.srcport == 80)`
```
http.server contains "Microsoft-IIS" && !(tcp.srcport == 80)
```

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/cf9eacee-4c18-4216-acd8-0d3b86807134" />
</p>

- Answer: `21`

### Find all Microsoft IIS servers. What is the number of packets that have "version 7.5"?

- The first part of this one was the same as the last one typing in `http.server contains "Microsoft-IIS"` and then `http.server contains "7.5"` to filter for the version number. Now, typing `http.server matches "7.5"` also works buy technically we don't need a regular expression here. If we were searching for versions `7.1, 7.2, etc.` the using `matches` would be the better option
```
http.server contains "Microsoft-IIS" && http.server matches "7.5"
```

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/287c0cb8-75f6-455a-aa43-0261d34bbc07" />
</p>

- Answer: `71`

### What is the total number of packets that use ports 3333, 4444 or 9999?

- For this one, we can use a set of numbers as depicted by the curly braces
```
`tcp.port {3333, 4444, 9999}`
```
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/9e877ee4-9640-45a3-8c6f-223f5011843d" />
</p>
