# Try Me

## Challenge Description

In this challenge, the user is given an APK that hides the flag using **AES encryption**. The main task is to reverse engineer the app, understand how the encryption is applied, and recover the plaintext flag.

---

## Exploitation

1. Run the APK in the emulator.  
   The app launches but provides no obvious clue through its user interface.

   
    <img width="395" height="748" alt="AppsMainScreen" src="https://github.com/user-attachments/assets/7a643b74-b507-49d4-8598-273eea7afa40" />

3. Decompile the APK using `apktool`:
   ```bash
   apktool d TryMe.apk
<img width="935" height="287" alt="decompilingTheAPk" src="https://github.com/user-attachments/assets/7feca669-50f6-4dfe-8704-c08874783e59" />

4. After greping for the flag, still no obvious info.I will move to: `TryMe\smali_classes3\com\example\tryme 
`, where the apps basic files are to search for further clues.However, these are in smali language, which makes them difficult to read and I will not be able to fully understand how the application works. In this solution, I will try to restore the APK application, that is, a file that is a result of compression and encoding to its original form, into Java classes. I will do this with the tool [JADX](https://github.com/skylot/jadx).
<img width="620" height="353" alt="JADX" src="https://github.com/user-attachments/assets/a6e08c17-664d-4135-8708-17f42df9f749" />
