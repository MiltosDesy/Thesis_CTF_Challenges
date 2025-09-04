# History Lesson

## Challenge Description

In this challenge, the user is given an Android APK file. The application displays a field to enter a flag, a button labeled *Submit Flag*, and a background image of the Colosseum with the title *Hail Caesar*. The task is to analyze the APK and retrieve the hidden flag.

## Tools & Techniques Used

- Android Emulator
- APKTool
- Smali code inspection
- Caesar Cipher (classical cryptography)
- Online Caesar Cipher brute force tool ([dcode.fr](https://www.dcode.fr/caesar-cipher))

## Steps to Solve

1. Install and run the APK in the Android Emulator.  
   - The app displays a text field for flag input and returns *Wrong* on incorrect submissions.
  

<img width="355" height="721" alt="Screenshot_1" src="https://github.com/user-attachments/assets/6e9f894d-27f2-4fd9-b532-5dbd11d9e6dc" />

I can see that the app has an input field for the user to enter the flag.There is also a colosseum photo, so the app indicates the ancient rome era, but it doesnt make sense, for now.

2. Next step is to decompile the APK with `apktool`:  
   ```bash
   apktool d HailCeasar.apk and then search for the flag using grep 

