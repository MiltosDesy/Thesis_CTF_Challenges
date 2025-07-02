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
3. Identify Smali code in `MainActivity.smali` and other Smali files.
4. When no useful information is found statically, decompile the APK using `apktool`:
   apktool d First_Challenge_Mobile.apk
5.Use grep to search recursively for the flag: grep -R "CTF" First_Challenge_Mobile/
6. The flag is located inside res/value/strings.xml as: CTFLIB{wellthatwaseasy}
