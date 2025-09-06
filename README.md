# Thesis CTF Challenges

This repository contains a collection of Capture The Flag (CTF) challenges developed for my undergraduate thesis at the Department of Digital Systems, University of Piraeus.

The challenges are focused on Android application security and cover various topics such as reverse engineering, steganography, cryptography, exploitation, mobile forensics, and APK decompilation. Each challenge is designed to simulate real-world attack scenarios and aims to help learners understand core principles of mobile security through hands-on exploration.

## Project Summary

The thesis explores the use of CTFs as an educational and skill-assessment tool in the context of cybersecurity, particularly in mobile environments.  
It introduces custom-made challenges that reflect realistic threat models, supported by modern tools and techniques such as:
- APKTool, Jadx
- Android Studio & Emulators
- SQLite, adb shell, backup analysis
- Base64, Caesar Cipher, AES cryptography logic

Each challenge folder includes:
- A detailed problem description
- The original APK or relevant artifacts
- A step-by-step write-up explaining the exploitation or resolution path
- Notes and tools used

## Covered Topics

- Reverse Engineering (Smali / Java)
- Steganography in Android backups
- Static & dynamic APK analysis
- SQLite database extraction and analysis
- Basic cryptographic attacks (Caesar, AES, Base64)
- Android app internals
- adb and emulator tooling
- Flag extraction via scripting and automation

## Challenge List 

| Challenge ID | Name                  | Main Concept                      |
|--------------|-----------------------|-----------------------------------|
| 01           | [Easy Peasy](Challenge1_EasyPeasy/README.md)            | APK reverse / static flag search  |
| 02           | [Backup No2](02_backup-no2/README.md)            | adb + image steganography         |
| 03           | [History Lesson](03_history-lesson/README.md)    | Caesar cipher decoding            |
| 04           | [Book Library](04_book-library/README.md)        | SQLite analysis inside APK        |
| 05           | [Try Me](05_try-me/README.md)                    | AES-encrypted flag with key reuse |
| 06           | [Dirty COW Simulation](06_dirty-cow-simulation/README.md) | Linux memory manipulation (PoC)   |

## Tools Used

- **APKTool**
- **JADX**
- **DB Browser for SQLite**
- **Android Studio**
- **adb shell**
- **Linux terminal (Kali, WSL)**
