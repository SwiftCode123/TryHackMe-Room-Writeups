<p align="center">
<img src="https://assets.tryhackme.com/img/logo/tryhackme_logo_full.svg" width="150" alt="TryHackMe Logo">
</p>

# Pyramid Of Pain
|  Room Name | Pyramid Of Pain |
|----------|-------|
| Author | Dhruv Tripathi |
| Link | [Pyramid Of Pain](https://tryhackme.com/room/pyramidofpainax) |

# Room Information
```bash Type: Walkthrough
Difficulty: Easy
Tags: - 
Meta Tags: Walkthrough, Walk-through, Write-up, Writeup
Subscription type: Premium
Description:
Learn what the Pyramid of Pain is and how it ranks indicators by how painful they are for an adversary to change.
```
## Task 1

### Read the above.

- Answer: `No answer needed`

## Task 2

### Analyse the report associated with the hash "b8ef959a9176aef07fdca8705254a163b50b49a17217a4ff0107487f59d4a35d" here. What is the filename of the sample?

- If we open up the given link, we can see the name
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/5aae951f-25bc-44be-ab86-d9ecbcfef0e2" />
</p>

- Answer: `Sales_Receipt 5606.xls`

## Task 3

### Read the following report to answer this question. What is the first IP address the malicious process (PID 1632) attempts to communicate with? 
- One of the first IP addresses it attempts to communicate to is listed below in `Network Activity`
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/5a4f9ffe-a2e1-411c-804c-01de38b8ea86" />
</p>

- Answer: `50.87.136.52`

### Read the following report to answer this question. What is the first domain name the malicious process ((PID 1632) attempts to communicate with?
- The domain name associated with that IP address is listed below under `DNS Requests`
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/5349cc4c-c1e8-4d51-9c69-e23acc0b814c" />
</p>

- Answer: `craftingalegacy.com`

## Task 4

### Go to this report on app.any.run (opens in new tab) and provide the first suspicious domain request you are seeing, you will be using this report to answer the remaining questions of this task.

- If we open the link and click on `DNS Requests`, we can see the first suspicious domain request
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/789ec981-db6d-4f2d-93c4-8fa90045c56a" />
</p>

- Answer: `craftingalegacy.com`

### What term refers to an address used to access websites?

- Answer: `Domain Name`

### What type of attack uses Unicode characters in the domain name to imitate the a known domain?

- Punycode is a way of converting words that cannot be written in ASCII, into a Unicode ASCII encoding. For example, writing `adıdas.de` instead of `adidas.com`

- Answer: `Punycode attack`

### Provide the redirected website for the shortened URL using a preview: `https://tinyurl.com/bw7t8p4u`

- We can type `+` at the end of the URL to see the actual website the shortened URL link is redirecting to. However, this did not work for me but you can go into VirusTotal to see the redirected URL

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/c7e750b2-c0af-4542-9e52-7be50bdf3093" />
</p>

- Answer: `https://tryhackme.com/`

## Task 5

### A security vendor has analysed the malicious sample for us. Review the report here to answer the following questions.

- Answer: `No answer needed`

### A process named regidle.exe makes a POST request to an IP address based in the United States (US) on port 8080. What is the IP address?

- I scrolled all the way to page 50 on the report to see the `Network Activity` this executable made and we can see the specific IP address it made it to
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/f7b87a3b-338a-4afb-b683-ddf667a318a5" />
</p>

- Answer: `96.126.101.6`

### The actor drops a malicious executable (EXE). What is the name of this executable?

- I opened up the report again and we can see what executable the actor dropped under the `Behavior Activities` section on page 4
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/0ef320ec-be61-46f2-befa-50c3614df3c7" />
</p>

- Also, I confirmed my thoughts when I scrolled to page 39 seeing under `File Activity` and `Dropped files` section
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/c20a8e7c-1e90-4eb5-8927-cabd450ed012" />
</p>

- Answer: `G_jugk.exe`

### Look at this report by VirusTotal. How many vendors determine this host to be malicious?

- We can see how many vendors determine this is malicious by opening the report
<p align="center">
<img width="1512" height="825" alt="image" src="https://github.com/user-attachments/assets/3dcfcf24-fea2-4f51-a9c8-5b6059bc92bd" />
</p>

- Answer: `9`

## Task 6

### Which network indicator helped us identify the malware type (Emotet)?

- User-strings identify the application or browser making an HTTP request such as `User-Agent: Mozilla/5.0`

- Answer: `User-Agent`

### How many POST requests are in the screenshot from the PCAP file?

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/ebb8bc88-52d3-4ce7-8907-6d58680620c4" />
</p>

- Answer: `6`

## Task 7
## Task 8
## Task 9
