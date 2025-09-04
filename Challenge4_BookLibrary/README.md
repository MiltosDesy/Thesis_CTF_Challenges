# Book Library 

## Challenge Description

In this challenge i am given with an apk that has an add button, fields for input and i must search for the flag.

![Homepage of App](https://github.com/user-attachments/assets/7aa6c1b9-e776-4569-946d-41a90e18b856)

---

## Exploitation 

1. By the looks of the home page, i can't seem to identify any clue that might give me any information about the app. Only that it has something to do with a library. If i press the add button:

   ![Add Book Page](https://github.com/user-attachments/assets/bdeaa3d9-5901-4dcd-8f27-c7509fbaaecb)

2. I am led to a new screen where i can add a books information (probably to be added to a database?) and when i add the book a success message pops up, but i still do not have any valuable information.

   ![Inserting Values](https://github.com/user-attachments/assets/db8d1ca7-e659-4fa4-b233-a5bf936f32e6)

3. After decompiling the apk using `apktool`, and greping for the flag, still no answers.

   ![Grep Flag](https://github.com/user-attachments/assets/521dcbfa-5276-46ec-a6ab-598d56210352)

4. The next step is to search the apps files for any info:

   ![Folder System](https://github.com/user-attachments/assets/3623a5e3-acd6-43ca-975d-a173df1fd13d)

   The MyDatabaseHelper.smali file looks interesting, lets see:

   ![The Method](https://github.com/user-attachments/assets/0c329028-de79-4fd9-8549-2f2ddc037562)

   At line 140 i can see that a table called `my_library` is created for an `SQLite database` with the same fields that where on the add page.

5. Now that i know that `SQLite` is being used for this apk, i can search the `assets` folder where usually any database or files that are needed for the execution of the app are stored. These files are compiled with the apk upon it's creation and are accessible (read-only) when the app is launched on the phone.

   ![Assets](https://github.com/user-attachments/assets/da457b47-7f54-48cf-85cb-44e6f2325e75)

6. Opening the assets folder, i can see a file called `BookLibrary`, so i suspect that it's a database that contains all the books that are being added from the application. I'll open it using `DB Browser for SQLite`.

   ![Table](https://github.com/user-attachments/assets/b184acf0-0203-4cb0-bcb5-985754e92b28)

7. By observing the database, i see that it has 3 tables:
   
   - `android_metadata`: A table that is automatically created when a table is initialized and contains basic meta-data about the database. It generally contains a column, `locale` which typically stores the language and the country in which it was created.
     
     ![Locale](https://github.com/user-attachments/assets/8a8b5fa4-5f19-4f3e-824d-b241ec6f4542)

   - `sqllite_sequence`: A table used internally by SQLite to track table keys that have the AUTOINCREMENT value, meaning fields in tables where their values automatically increase as they grow and are unique for each column (e.g., an identifying ID code for each book). It contains 2 fields: 'name' and 'seq'. The first contains the name of the table that uses such keys, and the second contains the last sequence number used for the automatic increment in the corresponding table.
     
     ![SQLite Sequence](https://github.com/user-attachments/assets/c50006ee-1fa7-46ca-8392-5772233a33cc)

   - `my_library`: Finally, i must search the last table and its contents. I can see that it has the books that the user adds and the entry with id-2 is the flag.

     ![Flag](https://github.com/user-attachments/assets/91166b82-2c47-46c4-a59c-9088e1bf282d)
