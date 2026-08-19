<p align="center">
  <img src="https://assets.tryhackme.com/img/logo/tryhackme_logo_full.svg" width="150" alt="TryHackMe Logo">
</p>

# Management Wants a Word

| **Room Name**  | Management Wants a Word                                               |
| -------------- | --------------------------------------------------------------------- |
| **Platform**   | TryHackMe                                                             |
| **Event**      | Hacker Holidays 2026                                                  |
| **Points**     | 120                                                                   |
| **Category**   | 🕵️ Forensics                                                         |
| **Difficulty** | Hard                                                                  |
| **Tags**       | Windows Forensics, Cryptography, Browser Forensics, Password Recovery |
| **Status**     | ✅ Completed                                                           |

## Room Information

```text
Type: CTF Challenge
Difficulty: Hard
Category: Forensics

Description:
It was always her. It was never a bug; it was the business model.
```

## Challenge Overview

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/deaa36dd-926c-4e0b-beb8-246c8176a8fa" />
</p>

**Hacker Holidays: Day 14 — Management Wants a Word**

This challenge involves investigating a Windows triage image from a guest's laptop to uncover a hidden password. The investigation focuses on browser artifacts and other forensic evidence scattered across the system. By piecing together these clues, the password can be recovered and used to obtain the final flag.

## Objectives

- Take a closer look at what she left behind
- Some things aren't as locked away as she thought
- Find out what she was hiding, and claim the flag

## Solution

- I read the challenge description and a couple of things stood out to me. The mention that `a browser will remember things for you` suggests that Chrome may have saved some credentials, while `not every hidden file needs a password cracker` hints that we won't need to brute force anything. Instead, we'll be following the artifacts already stored on the machine and using each piece of information to unlock the next

- The challenge also drops a couple of hints in the description. The line about a browser remembering things we never told anyone else is basically pointing us toward saved browser credentials. The `really good memory` part is a hint that the information may already be sitting somewhere on the machine rather than needing to be cracked. As for `1.26.29`, that's a `Chrome for Testing` version number, which might become relevant later when dealing with the browser artifacts

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/a893fd63-67ae-4f75-99f6-0a42a9badfc6" />
</p>

- I then proceeded to download the task files and used `scp` to get them onto my Kali Linux VM
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/e102ff0e-b76f-406c-8c5b-46d6add557d6" />
</p>

- I started by checking `C\Windows\System32\config`, which contains several important Windows registry hives. The three that immediately stood out were `SAM`, `SYSTEM`, and `SECURITY`, since they can contain information related to local accounts, authentication, and Windows secrets. At this point I didn't know exactly what I would find, but given that the challenge was hinting at a password, these seemed like a good place to start

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/aa5170ee-a0de-459b-a81b-1ca8c5b3c2fe" />
</p>

- I even checked `Documents` to see if there was anything and there was a file or folder called `backup` but when I tried to `cat` it, it was all gibberish which makes sense since it might be encrypted but for now I left it alone

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/5e1f685b-e9a5-49f9-81f4-d48817888e24" />
</p>

- Since we had the `SAM`, `SYSTEM`, and `SECURITY` hives, I wanted to see what credentials and secrets could be recovered from them. Instead of immediately trying to crack Vera's password hash, I used Impacket's `secretsdump`, which can extract information from offline Windows registry hives, including LSA secrets. The challenge had already hinted that we might not need to crack anything, so I was mainly interested in seeing whether Windows had already stored the password somewhere
```
impacket-secretsdump -sam SAM -system SYSTEM -security SECURITY LOCAL
```
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/4a078667-18c9-41cd-bf28-a445a9f2bbeb" />
</p>

- The command gave us a few different pieces of information, but the line that immediately caught my attention was `DefaultPassword`. It returned `minivera`, which means Windows had an autologon password stored in the `SECURITY` hive. This was exactly what the challenge's hint was getting at where instead of cracking Vera's NTLM hash, the password was already sitting in the forensic artifacts

- Now that we had Vera's Windows password, the next question was what can we use this password for? The challenge is clearly leading us toward saved browser credentials, and Windows uses something called `DPAPI` (Data Protection API) to protect secrets such as saved passwords. Vera's DPAPI master keys are stored inside her profile under `AppData\Roaming\Microsoft\Protect`, with a folder named after her Windows SID. I first navigated to Vera's DPAPI directory

```
cd ~/management-wants-a-word-forensics-hh-day-14/KAPE/C/Users/vera/AppData/Roaming/Microsoft/Protect
ls
```
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/cc298361-49c7-488b-9e5f-01a106575c1b" />
</p>

- This showed Vera's SID `S-1-5-21-2529683458-431225740-1723070931-1000` and then I entered that directory and checked what master key files were available

- There was a single master key `c90719ef-5b98-474e-b934-136d606a702a`

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/9a3a5ed5-f5b0-46ec-b4a8-6434dcd8d20e" />
</p>

- Since we already had Vera's password (`minivera`) and now had her SID, we had everything needed to try decrypting the master key with Impacket

```
impacket-dpapi masterkey \
-file "c90719ef-5b98-474e-b934-136d606a702a" \
-sid "S-1-5-21-2529683458-431225740-1723070931-1000" \
-password "minivera"
```

- The command successfully decrypted the master key and gave us `5e5715ec9b6df5a86e97902692a66d28e691f05d5bc1e04d0159cfe960e94c978c07e5004a0179d3a96df2468885a28175b0b02cc064445f116a752d2b3e9d40`. While this isn't Vera's password or the final answer. It's another key that we'll need for the next step, where we investigate Chrome's stored credentials

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/07c4bfab-a6b3-4042-9f27-2021fa2f219c" />
</p>

- Now that we had Vera's decrypted DPAPI master key, I wanted to see how Chrome was storing its saved passwords. Looking through Vera's Chrome profile, I found a file called Local State. This isn't a directory but rather it's a JSON configuration file used by Chrome

- Inside it, Chrome stores an `encrypted_key` under `os_crypt`. Since the key is protected with Windows DPAPI, we can use the master key we recovered in the previous step to decrypt it

- If we do `cat Local\ State | grep encrypted_key`, we can see the `encrypted key` is indeed stored under `os_crypt`

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/23cff2aa-668f-428a-9b2e-ad73baee2f27" />
</p>

- Among the files and directories, we can see `Local State` and `Default`. I then extracted Chrome's encrypted key from the JSON file

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/6606bcb2-dc5b-416e-8390-86062cea8f04" />
</p>

```
python3 -c 'import json,base64; d=json.load(open("Local State")); print(d["os_crypt"]["encrypted_key"])'
```

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/f9fe55c4-445b-46c5-bf9a-197bae1786b8" />
</p>

- The value was Base64 encoded and began with `RFBBUEk`, which decodes to DPAPI. This tells us that Chrome's key is itself protected using Windows DPAPI

- So I removed that DPAPI prefix, decoded the Base64 data, and saved the resulting encrypted blob

```
python3 -c 'import json,base64; raw=base64.b64decode(json.load(open("Local State"))["os_crypt"]["encrypted_key"])[5:]; open("/tmp/enckey.bin","wb").write(raw)'
```

- Finally, I used the DPAPI master key we recovered earlier to decrypt Chrome's key
```
impacket-dpapi unprotect \
-file /tmp/enckey.bin \
-key 0x5e5715ec9b6df5a86e97902692a66d28e691f05d5bc1e04d0159cfe960e94c978c07e5004a0179d3a96df2468885a28175b0b02cc064445f116a752d2b3e9d40
```

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/526fb736-eff6-4026-8dce-dd8ef9002d1e" />
</p>

- This successfully returned 32 bytes and now we have Chrome's AES-256 encryption key. We haven't recovered the actual saved password yet but we are closer

- Now that we had Chrome's AES key, I looked at the Default profile for the database that stores saved logins. Chrome keeps these in a SQLite database called Login Data

```
sqlite3 "Login Data" \ 'SELECT origin_url, username_value, hex(password_value) FROM logins;'
```
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/38dfa1ed-b5d9-473b-a77a-11de192aed19" />
</p>

- So we had a saved login for `bytelotus.thm`, with the username `VeraSecretVault`. The password, however, was still encrypted. The encrypted value starts with `763130`, which is the hexadecimal representation of v10. This tells us Chrome is using its v10 encrypted-password format. We already recovered the AES key earlier, so we can use Python's AESGCM implementation to decrypt this value

- I used the Chrome AES key and the encrypted password from SQLite
```python
python3 - <<'PY'
from cryptography.hazmat.primitives.ciphers.aead import AESGCM

key = bytes.fromhex(
    "206a39a0971327ea94"
    "87e4aea9844f5d36"
    "7016245698227693"
    "9a712646da0b02"
)

blob = bytes.fromhex(
    "763130C88A72A64F35F63E883EA0A7F64A6870E46B0BBB469A756EDA88B7E324C3E1C51015AA6FD8D65AC48961E1EA324CE1707807FEB3D7"
)

nonce = blob[3:15]
ciphertext = blob[15:]

password = AESGCM(key).decrypt(nonce, ciphertext, None)

print("Recovered password:", password.decode())
PY
```
- And finally, Chrome gave us the password which was `Wh4t1sV3raD0inG0nTh1sH0st`. This is a pretty satisfying point in the investigation. We started with the Windows registry, used Vera's login password to unlock DPAPI, used DPAPI to recover Chrome's encryption key, and finally used that key to decrypt a password Chrome had saved. Now I just needed to figure out what this recovered password actually unlocks

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/35f33b23-2664-4745-9808-6ce7575f8c50" />
</p>

- We now had a recovered password, but we still didn't know what it was for. Earlier, while browsing Vera's Documents folder, we noticed a file simply called `backup` with no file extension. I checked the file to see what we were dealing with

```bash
ls -lh backup
file backup
```
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/c88abef1-d1aa-4bb3-b06b-e0898e5100c9" />
</p>

- `file` couldn't identify a normal file type and the file was exactly 100 MiB and so that made it look more like a deliberately created container than a regular document

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/ff481cff-4074-4ac9-8ed7-86e48bab3f8a" />
</p>

- I also checked the beginning of the file and the bytes looked random rather than having a recognizable file header. At this point, an encrypted container seemed like a reasonable possibility

```bash
xxd -l 64 backup
```

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/dbca7252-dd7d-47f0-9f75-fc1b267dda54" />
</p>

- Since we had just recovered a password that could potentially unlock it, I decided to test it against the file. It printed `Command successful.` and so we now knew that `backup` was a VeraCrypt container and that the password we recovered from Chrome was the correct password for it

```
printf '%s' 'Wh4t1sV3raD0inG0nTh1sH0st' | \
cryptsetup open --type tcrypt --veracrypt --test-passphrase backup --verbose
```

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/a8ba95f6-d6ae-45e8-a570-5f7cd36de97d" />
</p>

- I then opened the container and mounted it read-only

```
sudo cryptsetup open \
  --type tcrypt \
  --veracrypt \
  backup \
  vera_backup

sudo mkdir -p /mnt/vera_backup

sudo mount -o ro /dev/mapper/vera_backup /mnt/vera_backup
```

- Finally, I checked what was inside via `ls -la /mnt/vera_backup`. This revealed a directory called `secret_financial_documents`, which looked very promising. Through `ls -la /mnt/vera_backup/secret_financial_documents` we can find there were two files `important_invoice_byte_lotus.pdf` and `transactions_q3.csv`. At this point, the password chain had finally led us to the actual hidden files. The remaining question was where the flag was hidden

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/95eecd41-e8f1-4f85-a0ad-8bdcf1f4a979" />
</p>

- I started with the invoice PDF because its name immediately stood out as the most likely place for the final clue. Running `pdfinfo` showed that it was a normal, single page PDF, but when I tried extracting its text with `pdftotext`, nothing was returned

```
pdfinfo /mnt/vera_backup/secret_financial_documents/important_invoice_byte_lotus.pdf
pdftotext /mnt/vera_backup/secret_financial_documents/important_invoice_byte_lotus.pdf -
```
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/d316b6f6-d1e6-4d52-b4a4-4f3558d84090" />
</p>

- Since there was no selectable text, I figured the invoice was probably rendered as an image rather than being made up of normal PDF text. I converted the PDF page into a PNG so I could inspect it visually which gave us `/tmp/invoice-1.png`

```
pdftoppm -png \
/mnt/vera_backup/secret_financial_documents/important_invoice_byte_lotus.pdf \
/tmp/invoice
```

<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/a76c74b5-373a-44cc-9f59-19a2a4c32135" />
</p>

- Opening the image and zooming into the invoice revealed the flag embedded directly in the image
```
xdg-open /tmp/invoice-1.png
```
<p align="center">
<img width="90%" height="90%" alt="image" src="https://github.com/user-attachments/assets/cee84df9-fa07-49d8-b608-b936970263d5" />
</p>

## Skills Learned

* Windows registry hive analysis and offline credential extraction
* Identifying and recovering plaintext autologon credentials from LSA secrets
* Understanding the Windows DPAPI key hierarchy and decrypting user master keys
* Analyzing Chrome forensic artifacts including Local State and Login Data
* Extracting and decrypting Chrome v10 credentials using AES-256-GCM
* Recognizing encrypted container characteristics through file structure clues
* Identifying and accessing VeraCrypt containers using recovered credentials
* Read-only mounting and examination of encrypted forensic evidence
* PDF and image-based artifact analysis when standard text extraction fails
* Navigating and interpreting a KAPE-collected Windows triage image
* Using Windows user SIDs to locate user-specific forensic artifacts
* Extracting and analyzing SQLite data from browser credential databases
* Understanding Base64 encoding and identifying DPAPI-wrapped Chrome encryption keys
* Working with encrypted blobs and extracting nonces and ciphertext for AES-GCM decryption
* Using command-line forensic utilities to identify and validate unknown file types
* Correlating artifacts across Windows, DPAPI, Chrome, and encrypted storage

## Tools Used

* Kali Linux
* Linux commands
  * ls
  * stat
  * file
  * xxd
  * mkdir
  * mount
  * printf
  * cd
  * scp
* KAPE
* Impacket
* secretsdump
* impacket-dpapi
* Python 3
* SQLite
* cryptography
* cryptsetup
* pdfinfo
* pdftotext
* pdftoppm
* ExifTool
* xdg-open

## Flag

🚩 **Flag:** `THM{*******************}`
