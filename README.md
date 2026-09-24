# Wiseguy — Forensics CTF

> A forensic investigation of an archive containing hidden configuration data, multiple false flags, and an encoded transmission.

## Overview

**Wiseguy** is a Forensics CTF focused on archive analysis and evidence validation.

A suspicious archive was recovered from an abandoned server. After extraction, several strings appeared to be valid flags. However, the challenge intentionally included multiple fake flags as decoys.

The objective was therefore not simply to find a flag-shaped string, but to:

- Enumerate the archive completely
- Discover hidden or unusual files
- Identify the relevant encoded data
- Decode the transmission
- Validate the recovered flag

**Category:** Forensics / Archive Analysis

**Key Techniques:** Archive enumeration, hidden-file inspection, hexadecimal decoding, evidence validation

The main clue was:

> `find command maybe useful`

---

## Investigation Workflow

```text
Archive
   ↓
Extract
   ↓
Enumerate Files
   ↓
Discover Hidden .config
   ↓
Inspect CONFIG_TRANSMISSION_CODE
   ↓
Reject Decoy Flags
   ↓
Hexadecimal Decoding
   ↓
Validate Final Flag
```

---

## 01 — Enumerate Everything

The archive was first extracted into a dedicated directory.

```bash
unzip challenge.zip -d challenge
cd challenge
find . -type f
```

The use of `find` was important because the challenge specifically hinted toward recursive file enumeration.

Several strings appeared to follow the expected flag format:

```text
H4S-CTF{hidden_somewhere_else}
H4S-CTF{$3crets_aren0t_h3r3}
H4S-CTF{n0t_@_r34l_fl4g}
H4S-CTF{pl4c3h0ld3r_0nly}
```

These were not immediately accepted as valid flags.

A correctly formatted flag is not necessarily valid evidence.

---

## 02 — Discover the Hidden Configuration

Further enumeration revealed a hidden:

```text
.config
```

Inside the configuration file was a suspicious entry:

```text
CONFIG_TRANSMISSION_CODE
```

The value consisted of a long sequence of hexadecimal byte pairs.

This provided a stronger evidence path than the flag-shaped strings found earlier.

The hexadecimal sequence was treated as encoded data rather than as another flag or password.

---

## 03 — Decode the Transmission

The hexadecimal value could be decoded directly using `xxd`.

```bash
echo "<hex_value>" | xxd -r -p
```

The same operation could also be performed using CyberChef with:

```text
From Hex
```

The decoded output produced the readable flag.

---

## 04 — Evidence Validation

The key lesson of the challenge was not simply decoding hexadecimal.

The archive deliberately contained multiple believable false flags.

The investigation therefore required correlation between:

1. The challenge clue
2. Complete archive enumeration
3. Discovery of the hidden `.config`
4. Identification of `CONFIG_TRANSMISSION_CODE`
5. Successful hexadecimal decoding
6. Validation of the resulting flag

This provided a defensible evidence chain instead of relying on the first flag-shaped string encountered.

---

## Command Summary

```bash
unzip challenge.zip -d challenge
cd challenge
find . -type f
cat .config
echo "<hex_value>" | xxd -r -p
```

---

## Key Takeaway

This challenge demonstrates an important forensic principle:

> **Finding something that looks correct is not the same as proving that it is correct.**

The archive contained multiple false flags specifically designed to reward premature conclusions.

The relevant evidence was the encoded:

```text
CONFIG_TRANSMISSION_CODE
```

stored inside the hidden `.config` file.

Decoding that value produced the final flag.

---

## Skills Demonstrated

- Archive analysis
- Recursive file enumeration
- Hidden-file discovery
- Configuration-file analysis
- Hexadecimal decoding
- Evidence validation
- Decoy identification
- Forensic reasoning

---

## Write-up

The complete evidence walkthrough is available here:

`writeup/Wiseguy-Forensics-CTF-Walkthrough.pdf`

---

## Disclaimer

This investigation was performed in a CTF environment for educational purposes.

All techniques and commands were used against the provided challenge data.
