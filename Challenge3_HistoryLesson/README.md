# History Lesson

## Challenge Description

In this challenge, the user is given an Android APK file. The application displays a field to enter a flag, a button labeled *Submit Flag*, and a background image of the Colosseum with the title *Hail Caesar*. The task is to analyze the APK and retrieve the hidden flag.

## Tools & Techniques Used

- Android Emulator
- APKTool
- Smali code inspection
- Caesar Cipher (classical cryptography)
- Online Caesar Cipher brute force tool ([dcode.fr](https://www.dcode.fr/caesar-cipher))

## Exploitation

1. Install and run the APK in the Android Emulator.  
   - The app displays a text field for flag input and returns *Wrong* on incorrect submissions.
  

<img width="355" height="721" alt="Screenshot_1" src="https://github.com/user-attachments/assets/6e9f894d-27f2-4fd9-b532-5dbd11d9e6dc" />

I can see that the app has an input field for the user to enter the flag.There is also a colosseum photo, so the app indicates the ancient rome era, but it doesnt make sense, for now.

2. Next step is to decompile the APK with `apktool`:  
   ```bash
   apktool d HailCeasar.apk
and then search for the flag using `grep` 

<img width="1121" height="172" alt="grep Flag" src="https://github.com/user-attachments/assets/fce0ff0b-0c12-458d-8352-86cd683ee016" />

3. I can't find the flag inside the apps files.Based on the information i have gained from the home screen photo, i will search internally the apps code to understand its function.The file will be in smali language, which is not that practical, but at least i might get a basic idea of what the app is about.

<img width="1001" height="407" alt="Screenshot_3" src="https://github.com/user-attachments/assets/13112083-c7d2-47b4-9982-888eec50a1b8" />

4. Here is the file system after the decompilation of the apk.Next step is to check the `MainActivity.smali` file (found at:  `smali_classes3/com/example/hailceasar`) 


<img width="856" height="870" alt="smaliMain" src="https://github.com/user-attachments/assets/9fd8ef27-1fba-415d-8452-540091910763" />

Observing the file, i can see a string variable with a familiar format: `JAMSPI{OpzavyfHukJyfwavPzMbu}`.The format resembles a `CTF flag (CTFLIB{...})`, but it is clearly encoded.
The app title Hail Caesar and the Colosseum image hint at Caesar Cipher encryption.

5. Using an online Caesar Cipher decoder with brute force attack (dcode.fr/caesar-cipher
), use the string and let the tool try all 25 possible shifts.


<img width="1183" height="580" alt="Screenshot_5" src="https://github.com/user-attachments/assets/2cb7c598-6e20-4f1f-a092-fd3fc2920f47" />

6. The decoded message reveals the correct flag: `CTFLIB{HistoryAndCryptoIsFun}`

## What You Learn

Reverse engineering APK files with APKTool

Inspecting Smali code for hardcoded strings

Recognizing classical cryptographic schemes from contextual hints

Using brute force techniques to decode Caesar Cipher


## Notes & Reflections

The challenge was designed to guide the user into identifying Caesar Cipher through contextual clues (app title and imagery).The main difficulty during implementation was embedding the encoded flag securely within the app logic.
