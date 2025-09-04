# Book Library

## Challenge Description

In this challenge, you are given an Android APK file. The application displays a home screen with an *Add* button and fields to input book information. The goal is to explore the app and retrieve the hidden flag.

![Home Page](https://github.com/user-attachments/assets/7aa6c1b9-e776-4569-946d-41a90e18b856)

---

## Tools & Techniques Used

- Android Emulator  
- APKTool  
- Smali code inspection  
- SQLite Database inspection (`DB Browser for SQLite`)  

---

## Exploitation

1. **Exploring the Home Screen**  
   The home page doesn't provide obvious clues, except that it relates to a library. Pressing the *Add* button leads to a page where books can be added:

   ![Add Book Page](https://github.com/user-attachments/assets/bdeaa3d9-5901-4dcd-8f27-c7509fbaaecb)

2. **Adding a Book**  
   On the add book page, entering book information triggers a success message, but no further clues appear:

   ![Inserting Values](https://github.com/user-attachments/assets/db8d1ca7-e659-4fa4-b233-a5bf936f32e6)

3. **Decompiling the APK**  
   Using `apktool` and searching for the flag with `grep` yields no results:

   ![Grep Flag](https://github.com/user-attachments/assets/521dcbfa-5276-46ec-a6ab-598d56210352)

4. **Inspecting App Files**  
   Browsing the app's folder structure reveals potential files of interest. The `MyDatabaseHelper.smali` file seems relevant:

   ![Smali Method](https://github.com/user-attachments/assets/0c329028-de79-4fd9-8549-2f2ddc037562)

   At line 140, a table called `my_library` is created in an SQLite database with the same fields as the add book page.

5. **Analyzing the Assets Folder**  
   The `assets` folder contains a file called `BookLibrary`, likely the database used by the app:

   ![Assets Folder](https://github.com/user-attachments/assets/da457b47-7f54-48cf-85cb-44e6f2325e75)

6. **Inspecting the Database**  
   Opening the database with `DB Browser for SQLite` reveals three tables:

   - **android_metadata**: Stores locale information.  
     
     ![Android Metadata](https://github.com/user-attachments/assets/8a8b5fa4-5f19-4f3e-824d-b241ec6f4542)

   - **sqlite_sequence**: Tracks AUTOINCREMENT values for tables.  

     ![SQLite Sequence](https://github.com/user-attachments/assets/c50006ee-1fa7-46ca-8392-5772233a33cc)

   - **my_library**: Contains user-added books. The flag is stored as an entry in this table (id-2):

     ![Flag Entry](https://github.com/user-attachments/assets/91166b82-2c47-46c4-a59c-9088e1bf282d)

---

## What You Learn

- Reverse engineering APK files with `apktool`  
- Inspecting Smali code for hidden logic  
- Understanding SQLite database structures within Android apps  
- Locating sensitive information stored in app assets  

---

## Notes & Reflections

The challenge guides the user to understand that the app stores data in an SQLite database within the assets folder. The main challenge is locating the correct table and entry containing the flag. Following the workflow of APK decompilation, database inspection, and Smali code analysis reveals the hidden information efficiently.

