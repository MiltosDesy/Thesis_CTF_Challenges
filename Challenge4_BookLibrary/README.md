# Book Library

##  Challenge Description

In this challenge, the user is given an APK with an "Add" button and input fields. The goal is to investigate the app and extract the hidden flag.

<img src="https://github.com/user-attachments/assets/7aa6c1b9-e776-4569-946d-41a90e18b856" alt="Homepage of App" width="300"/>

---

## Exploitation

1. By the looks of the home page, no direct clue is visible. The app seems to represent a library. Pressing the "Add" button:

   <img src="https://github.com/user-attachments/assets/bdeaa3d9-5901-4dcd-8f27-c7509fbaaecb" alt="Add Book Page" width="300"/>

2. The next screen allows adding book information, which produces a success message. Still, no meaningful information is revealed.

   <img src="https://github.com/user-attachments/assets/db8d1ca7-e659-4fa4-b233-a5bf936f32e6" alt="Inserting Values" width="300"/>

3. After decompiling the APK using `apktool` and searching with `grep`, no flag is found.

   ![Grep Flag](https://github.com/user-attachments/assets/521dcbfa-5276-46ec-a6ab-598d56210352)

4. Inspecting the decoded files reveals a `MyDatabaseHelper.smali` file. At line 140, it shows that a table named `my_library` is created in an `SQLite` database with the same fields as in the "Add" page.

   ![The Method](https://github.com/user-attachments/assets/0c329028-de79-4fd9-8549-2f2ddc037562)

5. Knowing the app uses `SQLite`, I check the `assets` folder. This folder usually contains databases or files bundled into the APK that are accessible in read-only mode during runtime.

   ![Assets](https://github.com/user-attachments/assets/da457b47-7f54-48cf-85cb-44e6f2325e75)

6. Inside the assets folder, I find a file called `BookLibrary`. This is most likely the database. I open it with **DB Browser for SQLite**.

   ![Table](https://github.com/user-attachments/assets/b184acf0-0203-4cb0-bcb5-985754e92b28)

7. The database contains three tables:
   - `android_metadata`: Stores locale/language information.
   - `sqlite_sequence`: Tracks auto-increment values for tables.
   - `my_library`: Contains the user-added books. Entry with `id=2` is the flag.

     ![Flag](https://github.com/user-attachments/assets/91166b82-2c47-46c4-a59c-9088e1bf282d)

---

## What You Learn

- How Android apps can use embedded SQLite databases.
- Locating and inspecting databases in the `assets` folder of an APK.
- Using **DB Browser for SQLite** to explore tables and extract hidden data.
- Understanding standard SQLite internal tables (`android_metadata`, `sqlite_sequence`).

---

## Notes & Reflections

This challenge highlights how sensitive information can be hidden inside app databases. The tricky part during development was embedding the pre-populated database into the APK so that it would be accessible while keeping the flag non-obvious to the solver.

---

## 🏁 Flag

CTFLIB{BooksAndFlags}
