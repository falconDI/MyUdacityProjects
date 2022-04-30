# Documentation for Sparkify ETL Imposed Database for Song Analytics<br>
#### Ethan Coolidge | Data Modeling Project | WGU-Data Analist<br>

___
---

## Purpose of Database for Sparkify<br>

The startup, Sparkify, desire to analyse popular songs on there music streaming app.<br>
Their user and song data is stored in various files of two JSON datasets, *Song* and *Log_data*, that are in no condition to be analyzed.<br>
The new database's data is extraceted from the two JSON datasets to create beautiful tables that are an data analyst delight.<br>
___

## Database design<br>

### Schema<br>

The database is constructed in star schema. The center fact table is *songplays* which contains records from the log and song data.<br>
The dimension tables are *users*, *songs*, *arists*, and *time* which records timestamp data.<br>
<br>
The **star** schema provides a confortable level of normalization, yet still allows queries on multiple tables with out numerous joins, as with a **snowflake** schema.<br>

### ETL pipline<br>

The ETL pipline starts by creating the tables that are neaded for the new database. I will need my container to store my data before I start extracting it.<br>
Next, the data is extated, transformed, and loaded, in a intertive process.<br>
<br>
I, First, start with the song data to extract my songs and artists information and store them in there respective tables. this that will be retrived from the *Song* dataset. <br>
I need this data first to conduct further ETL later on the *Log_data* dataset.<br>
<br>
The *Log_data* is loaded next to supply required data for the *songplays*, *users*, and *time* tables.<br>
The *users* table is simple ETL operation, but the *time* table requires more effort the convert the timestamp from milliseconds to accual datetime, that I can extract time information from.<br>
The *songplays* table is even more complcated. Because the *log_data* does not contain the song or artist IDs that I need, I have to run a special SELECT querey on the previously filled *songs* and *artists* tables to extract the IDs and insert them with the other *log_data* into the *song_plays* table.

### Code<br>

The program code abides in distict Python files.<br>
The ***sql_queries.py*** file contains the all the nessesary SQL for the create table and query statements. It also hold drop table statments to refresh the data ETL process.<br>
<br>
The ***create_tables.py*** file contains functions to create the SQL database.<br>
It first creates a connection to the **Sparkify** database and executes the SQL DROP TABLE statements in ***sql_queries.py*** to clear the way for the new tables.<br>
After it executes the CREATE TABLE statements, it closes the connectcion.<br>
<br>
Now that all the table are created, the ***etl.py*** file is executed to run ETL process described in the *ETL Process* section through a series of functions.<br>
It utilizes the SQL INSERT ans SELECT scripts that are predefined in the ***sql_queries.py*** script.<br>
<br>
To Execute theses scripts, Open the Terminal and execute them using the the syntax **'Python3 filname'**. The ***create_tables.py*** is executed first to create the connections and database.<br>
Next, the ***etl.py*** script is run to extract, transform and load the data into the new database.<br>
The ***sql_queries.py*** script is not run in the terminal because it is called in the other two python files.
<br>
---

####  Other Data Modifications<br>

Though it was not specified in the project requriements, I would place the user_agent column, in the *songplays* table, by itself in it own table.<br>
There are a lot of OS and browser congifurations that are the same across many rows.