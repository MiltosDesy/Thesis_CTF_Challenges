# Dirty COW Simulation

## Challenge Description
In this challenge, the goal was to simulate an Android device using Docker and attempt to escalate privileges to root by exploiting the **Dirty COW (CVE-2016-5195)** vulnerability in the Linux kernel. This bug affects kernels from version 2.6.22 up to 4.8, including Android devices that rely on these kernels.

---

## Exploitation

1. **Environment Setup**
   - Traditional Android emulators (BlueStacks, Android Studio Emulator, Genymotion) were evaluated but not suitable for creating a portable CTF challenge.
   - Instead, a **Docker-based Android image** was built using a custom `Dockerfile`, provided to me by the University of Piaeus Digital Systems Laboratory. The image defined an Android emulator with API level 27 and x86_64 architecture.
     <img width="694" height="453" alt="DockerFile1-25" src="https://github.com/user-attachments/assets/56047731-2f9c-4acf-b1b4-c76a62a0970a" />

   - After building with `docker build -t android_image .`, the container was run and accessed with:
     ```bash
     docker run android_image
     ```
      <img width="1639" height="569" alt="dockerBuild" src="https://github.com/user-attachments/assets/996843af-a3c8-4435-84a9-6e774e3fc76b" />

2. **Dirty COW Exploit**
   - The Dirty COW exploit abuses a flaw in the Linux kernel’s Copy-On-Write mechanism to allow a non-privileged user to overwrite read-only files, gaining root access.
   - For Android, the exploit can provide root privileges, enabling modifications to system files, installation of malicious apps, or access to sensitive data.
   - The exploit implementation used was from [timwr/CVE-2016-5195](https://github.com/timwr/CVE-2016-5195), requiring the **Android NDK**.

3. **Execution Attempt**
   - Installed dependencies inside the container (`git`, `zip`, `make`).
   - Downloaded and unpacked Android NDK:
     ```bash
     wget https://dl.google.com/android/repository/android-ndk-r27-beta1-linux.zip
     unzip android-ndk-r27-beta1-linux.zip
     ```
     <img width="929" height="313" alt="wget" src="https://github.com/user-attachments/assets/402b9420-07c1-4f96-879e-c24fdda364cb" />

   - Cloned exploit repository:
     ```bash
     git clone https://github.com/timwr/CVE-2016-5195.git
     ```
   - Configured environment (moved NDK files into exploit directory, set architecture `x86_64` and API level `27`).
     <img width="956" height="119" alt="lsafterall" src="https://github.com/user-attachments/assets/3b21fa3a-4314-45f7-8351-c389025f6e94" />

   - Compiled exploit with:
     ```bash
     ndk-build NDK_PROJECT_PATH=. APP_BUILD_SCRIPT=./Android.mk APP_ABI=x86_64 APP_PLATFORM=android-27
     make root
     ```
     <img width="937" height="210" alt="ndkbuild" src="https://github.com/user-attachments/assets/207355d9-c8b5-4d16-9b3d-adb41d0b3311" />


4. **Result**
   - The exploit did not succeed in gaining root, most likely because the Android image was patched against Dirty COW.
     <img width="934" height="577" alt="makeRoot" src="https://github.com/user-attachments/assets/fdf4ad45-0716-4794-9e55-8f35ee0fbfe3" />

---

## What You Learn
- **Docker for Android emulation**: Lightweight, reproducible environments suitable for CTF challenges.
- **Privilege escalation**: Understanding how kernel exploits like Dirty COW work.
- **Cross-compilation** with the Android NDK for exploit development.

---

## Notes & Reflections
- The challenge demonstrated the process of setting up an Android environment via Docker and applying a real-world kernel exploit.
- Even though the exploit was patched and did not result in root access, the exercise highlighted the importance of kernel-level vulnerabilities and patching.

---

## Flag
This challenge was **unintendedly unsolvable** due to the patched kernel.  
If successful, the expected outcome would have been root shell access, revealing the flag.

