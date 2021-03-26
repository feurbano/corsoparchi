1. Inserimento di nuovi dati: INSERT
2. Inserimento di nuovi dati: COPY
3. Inserimento di nuovi dati: /COPY
4. Aggiornamento di dati: UPDATE
5. Cancellazione di dati: DELETE
6. Subquery con FROM
7. Subquery con WHERE
8. Funzioni WINDOW






To import a new table with raw data into the database, you have to create an empty, temporary table in the PostgreSQL database in the temp schema, giving a meaningful name (e.g. import_studyarea_dataset_version) and possibly adding a short comment so that these tables can be removed later on. If you prefer (for example because you want to work without Internet connection), you can process raw data in a local database on your computer, then run a backup of the table(s) and restore them in the temp schema of the euro*_db database (create a temp schema on your local PostgreSQL to match the name of the schema). You can use the backup/restore utility in PgAdmin, or create first the structure and then import the data (in this case, you can export first as .csv from your local db, then import the csv in the master database OR you can back up your data with PgAdmin using format Plain and Use Column Insert and Use Insert Commands as options so you can then copy past the backup as a standard SQL that will populate the new table).
To import raw data into a database (euro*_db or local), you have to create a csv files (if data owners provided data in a different format, e.g. Excel or MS Access). If you use “,” or “;” as separator, check that no text use these characters. If this is the case, include text in quotes to import it correctly (this option is offered by import/export tools). You can clean NULL values (if they are marked by a special text like N/A) once data are imported into the database.
In the first instance, the best option is to replicate the structure of the original file into the database. To do so quickly, you can get the list of column names from the first line of the csv file and use them to generate sql of the table. For example, you can use the layout:

CREATE TABLE temp.import_datagroup_dataset_raw
(
  id serial,
 [...]
  CONSTRAINT yourtable_pkey PRIMARY KEY (id)
  );


Adding all the columns. For example, if the list of column names is (be sure you remove all the spaces in the column names, if any):

animal_name, age, sex, note


In the text editor, you can replace “,” with “ character varying, \n” (/n is usually used for return line). Then you can copy past it into the SQL code to get:

CREATE TABLE temp.import_datagroup_dataset_raw
(
  id serial,
  animal_name character varying,
  age character varying,
  sex character varying,
  note character varying,
  CONSTRAINT yourtable_pkey PRIMARY KEY (id)
  );


Now you can run the code in the database and create the table that us ready to store your raw data. In the code, there is an additional column “id”. It is a serial number used as primary key. It is possible that in the original dataset there is a primary key, but the serial “id” grants that one exist. Without a primary key you cannot update a table using a graphical interface.
As you can see, the data type of all columns is set to character varying, also when it is supposed to be something else (integer, timestamp and so on). This is because with character varying you can import everything, while if the data type is integer and you have some records with text (e.g. “NULL” or “?”), with integer the whole import will break. It is better to import everything and then check inside the database if all the values are correct.
First of all, you can check if data type are correct using a cast expression (“::”).
For example:

SELECT date_capture::date FROM temp.yourtable;


If there are records with an invalid date, you will get an error form Postgres. You can then list all the existing values to look for the potential problems. For example:

SELECT DISTINCT date_capture FROM temp.import_datagroup_dataset_raw ORDER BY date_capture;


If you have many records to change, it is better to use an UPDATE query instead of manually modify all the concerned record. For example if in the original data set the NULL records are marked with “N/A”:

UPDATE  temp.import_datagroup_dataset_raw
SET age = NULL
WHERE age = 'N/A';


The “LIKE” command in the WHERE statement is often useful to search string (see postgres manual).
When all the columns are properly formatted you can upload to the main tables casting them to the correct data type (otherwise, if you try to insert a character varying into a numeric or date field, you will get an error message and the import will fail).

To import data into the database, you can use the import/export tool in PgAdmin (3 or 4). Here you can specify that the field “id” is not included.
You cannot use the SQL command “copy” because the path to the file will be relative to the server, not to your local machine. You can use it if you upload data on Gdrive that is mirrored on the server (if you know the local path, you can ask for support). This is a good solution also for very large files (e.g. accelerometer data).
Another option is to use the “/copy” command from psql interface (command line, you can open it for example from pgadmin/plugins):

\COPY schema.table(list_of columns) FROM 'C:\your_local_folder\file.csv' with CSV HEADER delimiter ';' NULL'';


It is similar to SQL command “copy” but the source file is on your local computer. Another option is to send the file and the "copy" command to run to the database administrator and he will run it from within the server.

Note for import using psql.exe
For more information refer to the following: https://www.postgresql.org/docs/9.1/app-psql.html
In brief, to use psql in the command line, locate “psql.exe” and navigate to it in (e.g.) cmd. Below based on defaults for pgadmin4 installation:
cd C:\Program Files\pgAdmin 4\v4\runtime

Then launch, inserting your username:
psql -h eurodeer2.fmach.it -p 5432 -d eurolynx_db -U <username>
Enter password when prompted.

A command to import data into a given table could look as follows (based on columns of vhf_data_animals):
\COPY temp.vhf_data_animals_UPLOAD (animals_id, vhf_sensors_id, acquisition_time, latitude, longitude, vhf_validity_code, notes) FROM 'C:/path/to/file/to_import.csv' WITH DELIMITER','  NULL'NA' CSV HEADER



If data generated by sensors (e.g. GPS data) are provided in separate, multiple files, i.e. one per sensor/animal, you might want to find some fast way to import them in the same table where they can be processed together. First of all, you have to check all files have the same columns. If not, you should try to harmonize them. Then you can join them using a batch file that import them into the same table using the “copy” command. In some cases, as “copy” can be used only on the local machine, it is convenient to create a table on your local installation of postgres and then move it to the server (for example with a dump/restore or with an export to .csv). If the sensor data file does not contain the code of the sensor itself and it is only stored in the name of the file, you can retrieve the list of .csv files using a dos command or a tool like “Directory Lister”, then you can use a spreadsheet to generate the set of “copy” statements followed (one by one) by an update that set the value of an additional column “sensor_code” to the name of the sensor derived from the file name. This is just one of the many tricks. You can do something similar using R functionalities. The goal is simply to speed up the process.

Once in the database, you can run all possible controls. Below there are some checklists, one per data set. Once you discover new issues, please add them to the list. The database codes for study areas, animals and sensors are assigned only when data are imported into the master tables (they are automatically assigned by the database). During the import and data check process, the original name or code can be used and, once everything is ready to be imported, the original code can be replaced by the database code using the original code as key to link to the database code. This will be illustrated in the section “Data upload to main tables”.
Please note that while some (limited) SQL code is reported here, in general the SQL repository with code for processing data is stored in the GitHub account of the project (github.com/feurbano/eurodeer_db/tree/master/data_management). Every data curator is invited to contribute when additional code is created.
