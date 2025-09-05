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

5. After opening and decoding the apk, i run into its java classes and the `MainActivity.java` file.
   -Initially, I see that some classes are being imported:
   
      -- `android.util.Base64`: The Base64 class provides methods for encoding and decoding data in Base64 format.

      -- `javax.crypto.Cipher`: The Cipher class provides encryption and decryption functions. It is used for encrypting and            decrypting data.

      -- `javax.crypto.spec.SecretKeySpec`:
            The SecretKeySpec class is used to create an encryption key from a byte array.It is used to                                  define the key that is used in encryption and decryption. Therefore, there is a use of cryptography in the                   application.
   -Next, I notice that 3 String  variables are defined.

         ```bash
            private static final String ALGORITHM = "AES"; 
            private static final String EF = 
            "3EEMlxQbUw0N5GBSfI5tu5HwTbWpmJB2Wqrobu9USB8="; 
            private static final String HDK = "ewfwsdfhgdxjkvgc";
   
   The first variable (ALGORITHM) contains the words 'AES', so I understand that the encryption algorithm is AES.
   
   Continuing the analysis of the code, i understand that the other 2 variables are structural elements of the AES algorithm    being applied.
   
   <img width="476" height="82" alt="ιφ" src="https://github.com/user-attachments/assets/8994b8fd-00f1-4439-8c87-               fd7da6fc0b21" />
   
   In this section of code, I observe that the String variable userInput contains the values of the application field (user     input).Then there is an if condition that checks the user's input with the checking method (defined below), based on         which the success or failure message is issued accordingly.

   6. Diving deeper into the `checking()` method, I uncover the logic behind the validation process:


      <img width="753" height="195" alt="τσεκινγκ" src="https://github.com/user-attachments/assets/5d5b31ce-35e8-4a51-ade0-edc520ca14a7" />


      - A new `SecretKeySpec` object is instantiated using the `HDK` string as the key and the AES algorithm (`ALGORITHM`)          as the encryption standard.
      - A `Cipher` object is created and configured to use AES.
      - The cipher is initialized in `DECRYPT_MODE`, using the previously generated key.
      - The encrypted string `EF`, which is Base64-encoded, is decoded and decrypted.
      - The decrypted result is then compared against the `userInput` value.
      - If the values match, the method returns `true`, indicating a successful validation. Otherwise, it returns `false`.

      This reveals that the application internally stores both the encryption key and the encrypted message. By replicating          the decryption process with the same key and algorithm, one can retrieve the original message and validate or                reverse-engineer the logic.

   
   
