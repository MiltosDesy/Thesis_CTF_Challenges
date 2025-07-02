# Backup No2

## Challenge Description

This challenge simulates a scenario where an Android user has created a device backup using `adb`. Within this backup, an image file contains a hidden flag using basic steganography techniques. The goal is to extract the backup contents and locate the flag hidden inside the image.

## Tools & Techniques Used

- Android `adb` (Android Debug Bridge)
- Java-based unpacking tool: `abp.jar`
- TAR file manipulation
- Basic steganography (text hidden in image)
- Text editor for binary analysis

## Steps to Solve

1. The user is given a `.ab` file (Android backup).
2. Convert it to `.tar` using:
   ```bash
   java -jar abp.jar unpack backup.ab backup.tar

3. Explore the resulting archive:
4. Locate the image under the shared/ directory.
5. Open the .jpg file using a text editor and scroll through the binary data
6. Find the flag

   
What You Learn
-How Android backups work and are stored
-File format transformation and archive inspection
-Introduction to basic steganography via text injection
-Using Java tools to unpack .ab format

Notes & Reflections

The main difficulty in creating this challenge was ensuring the correct generation and preservation of the backup file with the desired content structure.Additionally, properly injecting the image into the correct shared directory and embedding the flag proved challenging.

