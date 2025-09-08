# Backup No2

## Challenge Description

This challenge simulates a scenario where an Android user has created a device backup using `adb`. Within this backup, an image file contains a hidden flag using basic steganography techniques. The goal is to extract the backup contents and locate the flag hidden inside the image.

## Tools & Techniques Used

- Android `adb` (Android Debug Bridge)
- Java-based unpacking tool: `abp.jar`
- TAR file manipulation
- Basic steganography (text hidden in image)
- Text editor for binary analysis

## Exploitation

1. The user is given a `.ab` file (Android backup).
![CTF photo](https://github.com/user-attachments/assets/b8590523-55d2-4f7f-87aa-188e0f5d30fb)

ADB (Android Debug Bridge) is a command-line tool that allows a computer to interact with and control an Android device. It can install or modify apps, as well as execute commands such as creating backups.

This is exactly how the challenge file was created. The given file is in the .ab format (Android Backup), which must be converted into a readable format before analysis.

Essentially, an .ab file is a TAR archive (a type of file that bundles one or more files together, similar to a .zip – see: [What is a TAR file](https://www.lifewire.com/tar-file-2622386)
) that has been compressed using Java.

After some research on how to extract data from an Android backup file (see: [Android StackExchange – Extracting .ab files](https://android.stackexchange.com/questions/28481/how-doyou-extract-an-apps-data-from-a-full-backup-made-through-adbbackup)
), I discovered a method that relies on internal Java classes available on Android devices, particularly BackupManagerService.java, and executed the following command:


2. Convert it to `.tar` using:
   ```bash
   java -jar abp.jar unpack backup.ab backup.tar
![runningthescripttocreatetarfile](https://github.com/user-attachments/assets/4f8c1baa-108f-4556-8da0-f1a0e16b403a)

3. Explore the resulting archive:
![theresultofthecommand](https://github.com/user-attachments/assets/178bab9a-4be6-4576-814b-945ad3a4a31c)

4. Locate the image under the shared/ directory.
![searchingthetarfile2](https://github.com/user-attachments/assets/f716afa2-3b9c-47ff-bd6f-b68f0d9059b5)

5. Open the .jpg file using a text editor and scroll through the binary data
![TheFlagIsInThePhoto](https://github.com/user-attachments/assets/2213f3b5-e4af-4864-a81e-72158653b60a)

6. Find the flag

   
## What You Learn

-How Android backups work and are stored

-File format transformation and archive inspection

-Introduction to basic steganography via text injection

-Using Java tools to unpack .ab format

## Notes & Reflections

The main difficulty in creating this challenge was ensuring the correct generation and preservation of the backup file with the desired content structure.Additionally, properly injecting the image into the correct shared directory and embedding the flag proved challenging.

