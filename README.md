# Password Cracking Lab Report  
**Dictionary Attacks Against Password-Protected PDFs**

> Educational lab report from **Networkwalks Academy**  
> Tools: John the Ripper (Johnny) • Networkwalks Hash Calculator • Dictionary Attack Lab

---

## Overview

| Field              | Details                                                                 |
|--------------------|-------------------------------------------------------------------------|
| **Lab Module**     | Password Cracking – Dictionary / Wordlist Attacks                       |
| **Environment**    | Networkwalks Academy Lab + Johnny (John the Ripper Jumbo 1.9.0)         |
| **Target Files**   | `My-Locked-PDF1.pdf`, `My-Locked-PDF2.pdf`, `My-Locked-PDF3.pdf`        |
| **Hash Format**    | `$pdf$4*4*128*…` (pdf2john / hashcat compatible)                        |
| **Attack Type**    | Offline dictionary attack against extracted PDF password hashes         |
| **Outcome**        | All three PDFs cracked successfully – flags recovered                   |

---

## 1. Objective

Demonstrate the complete offline password recovery workflow against password-protected PDF documents using dictionary (wordlist) attacks. The lab mirrors techniques used by penetration testers and digital forensics practitioners during **authorized** security assessments.

**Learning outcomes:**
- Extract a crackable PDF hash from an encrypted PDF
- Execute a dictionary attack with a standard wordlist
- Use both web-based lab tools and classic John the Ripper (via Johnny GUI)
- Recover the plaintext password, unlock the PDF, and capture the embedded flag

---

## 2. Tools Used

| Tool                              | Purpose                                                      |
|-----------------------------------|--------------------------------------------------------------|
| Networkwalks Hash Calculator      | Extract PDF hash from encrypted PDF (pdf2john format)        |
| PDF Hash Extractor (online)       | Alternative hash extraction producing identical format       |
| Networkwalks Dictionary Attack Lab| Web-based wordlist attack that hashes candidates and matches |
| Johnny + John the Ripper Jumbo    | GUI + core cracker supporting PDF and hundreds of other hashes|
| PDF Reader                        | Open the unlocked PDF to reveal the flag after recovery      |

---

## 3. Methodology

The identical three-step process was applied to every target PDF:

1. **Hash Extraction** – Upload the locked PDF to the Networkwalks Hash Calculator (or PDF Hash Extractor). The tool parses the PDF encryption dictionary and outputs a crackable `$pdf$4*4*128*…` string.
2. **Dictionary Attack** – Paste the hash into the Networkwalks Dictionary Attack Lab or load it into Johnny. The engine tries each wordlist candidate, computes its hash, and compares it to the target.
3. **Unlock & Flag Capture** – Open the PDF with the recovered password and extract the flag string contained inside the document.

---

## 4. Results

All three password-protected PDFs were successfully cracked.

### 4.1 Summary Table

| PDF   | Password    | Primary Tool      | Flag Recovered                                      |
|-------|-------------|-------------------|-----------------------------------------------------|
| PDF1  | `password1` | Dict. Lab / Johnny| `nw{networkwalks_flag1_jtr_270521_1}`               |
| PDF2  | `password1` | Dict. Lab / Johnny| `nw{networkwalks_persistence_jtr_270521}`           |
| PDF3  | `1qaz2wsx`  | Dict. Lab / Johnny| `nw{networkwalks_flag_260821_1}`                    |
| Extra | `good-luck` | Johnny            | `nw{cybersecurity_flag_captured_2608}`              |

### 4.2 Detailed Findings

#### My-Locked-PDF1.pdf

- **Hash** (Networkwalks Hash Calculator):
  ```
  $pdf$4*4*128*-1060*1*16*55d1a5c14175da449753199e44971d32*32*777fd021a7f3c5ae598c0c6495c7f76e00000000000000000000000000000000*32*ceecdac74b19b5a62688d3b3524e1374c955cbb9cc3c45316494d9446ef81af1
  ```
- **Cracked password:** `password1` (matched at ~91 % of wordlist in Dictionary Attack Lab)
- **Johnny also cracked an alternate hash** with password: `good-luck`
- **Flags:**  
  `nw{networkwalks_flag1_jtr_270521_1}`  
  `nw{cybersecurity_flag_captured_2608}`

#### My-Locked-PDF2.pdf (204.7 KB)

- **Hash** (matches hash2.txt):
  ```
  $pdf$4*4*128*-1028*1*16*0853f2cde0ef15b1c0f93ed229d3b1ad*32*8f13ce5aa39ad974364d36a057da76790021446990b9e4114071a4d9104984c1*32*ceecdac74b19b5a62688d3b3524e1374c955cbb9cc3c45316494d9446ef81af1
  ```
- **Cracked password:** `password1` (confirmed in both Dictionary Attack Lab and Johnny)
- **Flag:** `nw{networkwalks_persistence_jtr_270521}`

#### My-Locked-PDF3.pdf (313.5 KB)

- **Hash** (matches hash3.txt):
  ```
  $pdf$4*4*128*-1028*1*16*34eb542eff4e1b0b32d25ce15a9a7281*32*b77872bfc9a24fb2f845066283a8fc1b0021446990b9e4114071a4d9104984c1*32*e7572256e4b552cd57988f5134214b91920d94d7a6bf550ea94a2995c7f2ab02
  ```
- **Cracked password:** `1qaz2wsx` (keyboard-pattern password; matched early at ~35 % of wordlist)
- **Flag:** `nw{networkwalks_flag_260821_1}`

---

## 5. Analysis & Key Observations

- **Weak passwords collapse strong encryption** – AES-128 PDF encryption is cryptographically sound, yet `password1` and `1qaz2wsx` were recovered in seconds from a short wordlist.
- **Password reuse multiplies risk** – The same password (`password1`) protected both PDF1 and PDF2; compromise of one immediately unlocks the other.
- **Keyboard patterns are predictable** – `1qaz2wsx` is a classic “keyboard walk” that appears in virtually every public wordlist and rule set.
- **Offline attacks require no further interaction** – Once the PDF hash is extracted, cracking proceeds entirely offline with no network traffic to the original file.
- **Tool interoperability is high** – Networkwalks tools and John the Ripper share the same pdf2john / hashcat compatible hash format, allowing seamless movement between web labs and local crackers.

---

## 6. Risk Analysis

| # | Finding                      | Evidence                                      | Potential Impact                              | Risk   |
|---|------------------------------|-----------------------------------------------|-----------------------------------------------|--------|
| 1 | Weak PDF passwords           | `password1` & `1qaz2wsx` cracked quickly      | Unauthorized access to document contents      | High   |
| 2 | Password reuse               | Same password on PDF1 and PDF2                | One compromise unlocks multiple files         | Medium |
| 3 | Keyboard pattern password    | `1qaz2wsx` recovered from wordlist            | Easily guessed via common pattern rules       | High   |
| 4 | Hash easily extractable      | Any holder of the PDF can obtain the hash     | Enables fully offline cracking                | Medium |

---

## 7. Recommendations

1. Never use dictionary words, simple sequences, or keyboard patterns for document encryption. Prefer long random passphrases (16+ characters) generated by a password manager.
2. Do not reuse the same password across multiple sensitive documents.
3. Where possible, combine PDF encryption with additional controls (rights management, secure file-sharing platforms, or access-control lists).
4. Organizations should periodically audit password-protected documents and enforce strong password policies for document-level encryption.
5. Perform password recovery **only** on files you own or for which you have explicit written authorization.

---

## 8. Conclusion

This lab successfully demonstrated the end-to-end offline password cracking workflow against PDF documents: hash extraction, dictionary attack, password recovery, and flag capture.

All three target files were cracked using weak, common passwords (`password1`, `1qaz2wsx`, `good-luck`). The results reinforce that the practical strength of PDF password protection depends almost entirely on **password quality**, not on the underlying encryption algorithm.

The combination of Networkwalks Hash Calculator, Dictionary Attack Lab, and John the Ripper (Johnny) forms a practical toolkit applicable to authorized security testing and forensic password recovery.

### Flags Captured
1. `nw{networkwalks_flag1_jtr_270521_1}`
2. `nw{networkwalks_persistence_jtr_270521}`
3. `nw{networkwalks_flag_260821_1}`
4. `nw{cybersecurity_flag_captured_2608}`

---

## 9. Disclaimer

All activities described in this report were performed exclusively on intentionally created lab files supplied by Networkwalks Academy for educational purposes. **Cracking passwords on files or systems without explicit authorization is illegal.** The techniques shown must never be applied against documents or systems you do not own or lack written permission to test.

---

• Confidential – Educational Use Only 
