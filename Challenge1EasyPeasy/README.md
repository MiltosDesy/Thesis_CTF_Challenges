# Easy Peasy

## Challenge Description

In this introductory challenge, the user is provided with an Android APK file. The goal is to find a hidden flag embedded within the app. The scenario simulates a beginner Android developer who has accidentally left sensitive information inside the application package.

## Tools & Techniques Used

- Android Studio Emulator
- APKTool
- Static analysis of AndroidManifest and Smali files
- Linux terminal (Kali) with `grep`
- Understanding of Android APK file structure

## Steps to Solve

1. Load the APK in Android Studio using `Profile or Debug APK`.
![ASwithTheAPK](https://github.com/user-attachments/assets/f4da7755-aeaa-431f-a138-8ce1c266692e)


2. Explore the app structure and look into `AndroidManifest.xml` and other resources.

![manifest](https://github.com/user-attachments/assets/c5b7a20a-cce7-49b8-a73e-38bbb5580e81)

3. Identify Smali code in the app.
![FileSystemAS](https://github.com/user-attachments/assets/8214a989-849c-4af6-b432-1a0417596c36)

4. When no useful information is found statically, decompile the APK using `apktool`:
  
   ![316748838_693235655479461_1235365210818659807_n](https://github.com/user-attachments/assets/ebfe1c20-05ae-4caf-9a18-8ece2aad6506)

5.Use grep to search recursively for the flag: grep -R "CTF" First_Challenge_Mobile/
![flag](https://github.com/user-attachments/assets/ee36633a-3c71-4a8f-943a-c89b600502c3)

6. The flag is located inside res/value/strings.xml as: CTFLIB{wellthatwaseasy}
