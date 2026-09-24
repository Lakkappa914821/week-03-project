# 🔓 PDF Password Cracking — Week 3

**Extracting a crackable hash from a password-protected PDF and recovering the password via dictionary attack**

![Skill](https://img.shields.io/badge/Skill-Password%20Cracking-404040?style=flat-square&labelColor=C00000)
![Skill](https://img.shields.io/badge/Skill-Hash%20Analysis-404040?style=flat-square&labelColor=C00000)
![Tool](https://img.shields.io/badge/John%20the%20Ripper-000000?style=flat-square&labelColor=000000)
![Tool](https://img.shields.io/badge/pdf2john-2E7D32?style=flat-square&labelColor=000000)
![Ethics](https://img.shields.io/badge/Educational%20Purposes%20Only-C00000?style=flat-square&labelColor=000000)

---

## 📌 Project Overview

This project covers **Week 3** of the Networkwalks Academy Cybersecurity & Ethical Hacking curriculum: recovering the password from an encrypted, password-protected PDF (`My-Locked-PDF1.pdf`) using **hash extraction** and a **dictionary attack**.

The exercise was completed two ways:

1. **Browser-based method** — using Networkwalks' own **Hash Calculator** and **Password Cracker** web tools to extract the PDF's crackable hash and run a dictionary attack against it directly in the browser.
2. **Tool-based method** — extracting the same `$pdf$...` hash with **pdf2john** (via an online hash-extractor as an alternative to running it locally in Kali) and loading it into **Johnny**, the GUI front-end for **John the Ripper**, for a real offline cracking session.

Both methods target the identical hash format used by John the Ripper / Hashcat, so the exercise demonstrates the same underlying concept — password-protected PDFs store a derivable hash of their password, and a weak password can be recovered by hashing every word in a wordlist and checking for a match — using both an educational simulation and the real-world tool.

> ⚠️ `My-Locked-PDF1.pdf` is a training file deliberately created and provided by the instructor with a known, weak dictionary password. This exercise never targets a real, unknown, or third-party encrypted file.

---

## 🎯 Objectives

- Understand how a password-protected PDF's password is represented as a crackable hash.
- Extract that hash using a **Hash Calculator** tool.
- Run a **dictionary attack** against the extracted hash to recover the plaintext password.
- Use the recovered password to unlock the PDF and retrieve the hidden flag.
- Repeat the hash-extraction step using **pdf2john** logic (via an online extractor) to produce the exact same hash format used by **John the Ripper**.
- Load that hash into **Johnny** (John the Ripper's GUI) to see the same attack performed with a professional cracking tool.

---

## 🛡️ Purpose

Password-protected documents are common in real environments — HR files, financial statements, and legal documents are frequently distributed as encrypted PDFs. Understanding how their protection actually works (a hash derived from the password and the document's encryption parameters) — and how quickly a weak password falls to a basic dictionary attack — is a core lesson in password security: **the strength of the password, not the encryption algorithm, is almost always the weakest link.**

This lab also introduces the standard offline-cracking workflow used throughout the industry:

```
Encrypted file → extract hash (pdf2john) → crack hash offline (John the Ripper / Hashcat) → recovered password
```

> ⚠️ **This lab is for educational purposes only.** The target file was provided by the instructor specifically for this exercise, with a known, intentionally weak password. Attempting to crack the password of any document you do not own or have explicit authorization to test is illegal in most jurisdictions.

---

## ⚙️ Tools Used

| 🧩 Tool                     | Purpose                                                              |
|------------------------------|------------------------------------------------------------------------|
| **Hash Calculator** (networkwalks.com) | Extracts a crackable `$pdf$...` hash directly from an uploaded PDF, entirely client-side |
| **Password Cracker** (networkwalks.com) | Runs a dictionary attack against a pasted hash, in-browser, mirroring how John the Ripper works |
| **pdf2john** (via Online HashCrack's PDF Hash Extractor) | Extracts the same John-the-Ripper-compatible hash format from a PDF, as an alternative to running `pdf2john.pl` locally |
| **John the Ripper / Johnny** | Industry-standard offline password-cracking tool; Johnny is its graphical front-end |

---

# 🪜 Lab Walkthrough

## Method 1 — Browser-Based Hash Extraction & Dictionary Attack

### Step 1. Extract the Hash with the Hash Calculator

The target file, `My-Locked-PDF1.pdf` (65.2 KB), was uploaded to the **Hash Calculator** tool. Since the tool detected the PDF was encrypted, it automatically extracted a crackable hash in `pdf2john` / hashcat-compatible format — entirely client-side, with nothing uploaded to a server.

![Hash Calculator tool landing page](screenshots/01-hash-calculator-tool.png)
*The Hash Calculator tool — generates MD5/SHA hashes from text or files, and extracts crackable hashes from password-protected PDFs.*

![Hash extracted from the locked PDF](screenshots/02-hash-calculator-pdf-upload.png)
*`My-Locked-PDF1.pdf` uploaded and parsed locally — the tool confirms the PDF is encrypted and outputs the extracted `$pdf$...` hash, ready to copy into a cracking tool.*

**Extracted hash:**
```
$pdf$4*4*128*-1060*1*16*55d1a5c14175da449753199e44971d32*32*777fd021a7f3c5ae598c0c6495c7f76e0000000000000000000000000000000*32*ceecdac74b19b5a62688d3b3524e1374c955cbb9cc3c45316494d9446ef81af1
```

### Step 2. Run a Dictionary Attack with the Password Cracker

The extracted hash was pasted into the **Password Cracker (Dictionary Attack Lab)** tool, which hashes every word in a wordlist and compares it against the target hash — the exact same logic John the Ripper uses internally.

![Password Cracker running through wordlist, 30/100 tried](screenshots/03-password-cracker-progress.png)
*The dictionary attack in progress — 30 of 100 candidate passwords tried, hashing each one and comparing it against the extracted PDF hash.*

![Password Cracker successfully matches the password](screenshots/04-password-cracker-success.png)
*Match found at attempt 91/100 — the tool confirms `PASSWORD CRACKED SUCCESSFULLY` with the recovered password.*

**Result:** the PDF's password was recovered as **`password1`** — a password that appears in virtually every common password wordlist, which is exactly why it fell so quickly.

### Step 3. Unlock the PDF and Capture the Flag

The recovered password was entered into the PDF viewer's password prompt to unlock the file.

![Entering the recovered password into the PDF's password prompt](screenshots/05-pdf-password-entry.png)
*The cracked password entered into the "Password required" prompt for `My-Locked-PDF1.pdf`.*

![Flag captured after successfully unlocking the PDF](screenshots/06-flag-captured.png)
*The unlocked PDF confirming success and revealing the training flag: `nw{networkwalks_flag1_jtr_270521_1}`.*

---

## Method 2 — Real-World Hash Extraction & John the Ripper (Johnny)

To connect the browser simulation to the actual tool used in the field, the same PDF's hash was re-extracted using **pdf2john** logic and loaded into **Johnny**, the GUI for John the Ripper.

### Step 1. Extract the Hash with pdf2john (via Online HashCrack)

Since `pdf2john.pl` (bundled with John the Ripper on Kali) performs this extraction locally, the **Online HashCrack PDF Hash Extractor** was used here as an accessible equivalent — it runs the same `pdf2john` logic server-side and confirms uploaded files are deleted immediately.

![PDF Hash Extractor tool landing page](screenshots/09-onlinehashcrack-intro.png)
*Online HashCrack's PDF Hash Extractor — built on pdf2john, converts a PDF directly into a crackable hash format.*

![PDF hash extracted, matching the Hash Calculator's output](screenshots/10-onlinehashcrack-hash-output.png)
*`My-Locked-PDF1.pdf` uploaded and converted, producing the identical `$pdf$...` hash seen in Method 1 — confirming both extraction methods are equivalent.*

### Step 2. Load the Hash into Johnny (GUI for John the Ripper)

The extracted hash was saved to a text file (`hash1.txt`) and opened directly in Johnny.

![Opening the extracted hash file in Johnny](screenshots/07-johnny-open-hash-file.png)
*Johnny's Open dialog, selecting `hash1.txt` from the Downloads folder alongside the John the Ripper installation files.*

![Hash successfully loaded into Johnny, ready to crack](screenshots/08-johnny-hash-loaded.png)
*The hash loaded into Johnny's password list, correctly identified with Format: **PDF**, ready for a dictionary or brute-force attack to be started.*

**Outcome:** with the hash loaded and format auto-detected as `PDF`, Johnny is ready to run the identical dictionary attack against the real John the Ripper engine — the same recovery (`password1`) that the browser tool already demonstrated.

---

## 📊 Results Summary

| Item                         | Result                                                        |
|-------------------------------|-------------------------------------------------------------------|
| Target file                   | `My-Locked-PDF1.pdf`                                              |
| Extracted hash format          | `$pdf$4*4*128*...` (pdf2john / hashcat-compatible)               |
| Cracking method                | Dictionary attack (100-word list)                                 |
| Attempts to match               | 91 / 100                                                          |
| **Recovered password**         | **`password1`**                                                   |
| Flag captured                  | `nw{networkwalks_flag1_jtr_270521_1}`                              |
| Cross-verification              | Same hash independently reproduced via pdf2john (Online HashCrack) and loaded into Johnny / John the Ripper |

---

## 💡 What I Learned

- **How PDF encryption is actually cracked:** a password-protected PDF doesn't need to be "decrypted" directly — instead, a hash derived from the password and the file's encryption parameters is extracted, and that hash is attacked offline, exactly like a Linux `/etc/shadow` hash or a Wi-Fi handshake.
- **Dictionary attacks are fast against weak passwords:** `password1` was found in 91 attempts out of a 100-word list — real-world wordlists (like `rockyou.txt`) contain millions of entries and crack similarly weak passwords in seconds.
- **pdf2john and browser-based tools produce identical output:** extracting the hash two different ways (a purpose-built web tool vs. the actual `pdf2john` utility) produced the exact same `$pdf$...` string, confirming both represent the same underlying John the Ripper hash format.
- **Johnny vs. command-line John the Ripper:** Johnny provides the same cracking engine as the `john` CLI tool, but with a GUI for loading hash files, selecting attack modes, and tracking progress — useful for demonstrations and for users less comfortable at the command line.
- **The real lesson is password strength, not encryption strength:** PDF encryption itself (AES/RC4 depending on version) is not what failed here — a common, dictionary-guessable password is what made the file crackable in seconds.

---

## 🔐 Security & Ethical Use

This lab is intended **strictly for educational purposes**.

- `My-Locked-PDF1.pdf` was a training file deliberately created by the instructor with a known, intentionally weak password — never a real, unknown, or third-party document.
- Extracting or cracking the password hash of any file you do not own or do not have explicit written authorization to test is illegal in most jurisdictions, regardless of whether the file is ultimately unlocked.
- Uploading real, sensitive password-protected files to third-party online hash-extraction tools carries genuine privacy risk in practice — this lab used a disposable training file for exactly that reason.
- The purpose of learning this technique is defensive: understanding how quickly weak passwords fail helps justify strong, unique passwords and passphrase policies in real systems.

---

## 🔗 Tools & Resources

- **Hash Calculator** — <https://networkwalks.com/hash-calculator/>
- **Password Cracker (Dictionary Attack Lab)** — <https://networkwalks.com/password-cracker/>
- **Online HashCrack — PDF Hash Extractor** — <https://onlinehashcrack.com/tools-pdf-hash-extractor.php>
- **John the Ripper** — <https://www.openwall.com/john/>
- **Johnny (GUI for John the Ripper)** — <https://openwall.info/wiki/john/johnny>

---

## 👤 Author

**Lakkappa Padmanna Pujer**
Cybersecurity Learner — Ethical Hacking & Penetration Testing

---

## 📌 Project Information

**Program:** Cybersecurity & Ethical Hacking | **Week:** 03 | **Topic:** PDF Password Cracking with pdf2john &amp; John the Ripper | **Course Reference:** Networkwalks Academy
