# Try Me

## Challenge Description

In this challenge, the user is given an APK that hides the flag using **AES encryption**.  
The main task is to reverse engineer the app, understand how the encryption is applied, and recover the plaintext flag.  

The challenge is named *Try Me* because the app simply invites the user to “try” inputting something, without offering any explicit clues. This requires looking under the hood of the APK to reveal its hidden logic.

---

## Exploitation

1. Run the APK in the emulator.  
   The app launches but provides no obvious clue through its user interface.

   <img src="https://github.com/user-attachments/assets/7a643b74-b507-49d4-8598-273eea7afa40" alt="AppsMainScreen" style="max-width:70%; height:auto;" />

2. Decompile the APK using `apktool`:
   ```bash
   apktool d TryMe.apk
   ```

   <img src="https://github.com/user-attachments/assets/7feca669-50f6-4dfe-8704-c08874783e59" alt="Decompiling APK" style="max-width:70%; height:auto;" />

3. After grepping for the flag, still no obvious info is found.  
   I move to: `TryMe\smali_classes3\com\example\tryme`, where the app’s basic files are stored.  
   However, these are in **smali language**, which makes them hard to read. To better understand the logic, I restore the APK into Java classes using [JADX](https://github.com/skylot/jadx).

   <img src="https://github.com/user-attachments/assets/a6e08c17-664d-4135-8708-17f42df9f749" alt="JADX Interface" style="max-width:70%; height:auto;" />

4. In the Decompiled code, I open the `MainActivity.java` file.  
   At the top, I see important imports:

   - `android.util.Base64`: provides Base64 encoding/decoding.  
   - `javax.crypto.Cipher`: provides encryption/decryption functions.  
   - `javax.crypto.spec.SecretKeySpec`: creates an encryption key from a byte array.  

   I also notice three defined `String` variables:

   ```java
   private static final String ALGORITHM = "AES"; 
   private static final String EF = "3EEMlxQbUw0N5GBSfI5tu5HwTbWpmJB2Wqrobu9USB8="; 
   private static final String HDK = "ewfwsdfhgdxjkvgc";
   ```

   The first variable (ALGORITHM) confirms the use of **AES encryption**.  
   The other two (`EF` and `HDK`) are the ciphertext and the key.

   <img src="https://github.com/user-attachments/assets/89c76716-47c4-4400-984a-1cc7a2a7ecc7" alt="Java snippet" style="max-width:70%; height:auto;" />

   I also observe that the user input is compared against the decrypted version of `EF` through a `checking()` method.

5. Diving into the `checking()` method, the logic becomes clear:

   <img src="https://github.com/user-attachments/assets/cf3dd3ef-3ee8-4a51-ade0-edc520ca14a7" alt="Checking method" style="max-width:70%; height:auto;" />

   - A new `SecretKeySpec` object is created using the string `HDK` as the key.  
   - A `Cipher` object is initialized in `DECRYPT_MODE`.  
   - The Base64-encoded string `EF` is decoded and decrypted.  
   - The decrypted result is compared with the user input.  
   - If they match, the validation succeeds.  

   This reveals that the APK already stores both the **key** and the **encrypted flag**, so replicating the same decryption process will directly yield the flag.

6. I use the tool [AnyCrypt](https://anycript.com/) to decrypt the text with AES using the key `HDK`:

   <img src="https://github.com/user-attachments/assets/622bdff4-7fbf-418f-a20a-bed588c27d86" alt="Anycrypt Decryption" style="max-width:70%; height:auto;" />

7. The decryption works, and I successfully retrieve the flag:

   ```txt
   CTFlearn{AESisNotSoHidden}
   ```

---

## Conclusion

This challenge demonstrates how **hardcoded keys and encrypted strings in an APK** can be easily exposed by decompiling the app. Even though AES is a strong encryption algorithm, if the key and ciphertext are shipped together inside the code, the encryption provides no real security.
