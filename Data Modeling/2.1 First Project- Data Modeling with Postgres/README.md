# ETL - Sparkify User Activity 


## Purpose

These Python Scripts take information of songs and artists in individual Jsons for each one, from the Million Song Dataset and log information (page NextSong) from Sparkify users and load it into a PostgreSQL relational data base, in order to allow Sparkify data analysts to better use the data to understand the bussiness and its users. 

## Files in the Repository

* **create_tables.py**: Creates the database for Sparkify and all tables according to the designed data model. If they already exist, the script will drop the existing tables. The created tables are Songplay, User, Song, Artist and Time
* **etl.py**: Loads the information of the sources (Log and Song data) into the tables of the Sparkify database. 
* **sql_queries.py**: Contains all the SQL queries that are used by the create_tables.py and etl.py scripts, to create tables, drop tables and insert information in tables. 
* **test.ipynb**: Notebook that contains some sanity tests to check if the ETL worked as expected.


## How to run the scripts

It's necessary to open a SQL instance, run create_tables.py first, and then etl.py. create_tables.py should only be run once, because it resets the database. 

## The sparkify Data Model

#### Fact Table
* **songplays**: Records in log data associated with song plays. 
songplay_id, start_time, user_id, level, song_id, artist_id, session_id, location, user_agent

#### Dimension Tables
* **users** - users in the app
user_id, first_name, last_name, gender, level
* **songs** - songs in music database
song_id, title, artist_id, year, duration
* **artists** - artists in music database
artist_id, name, location, latitude, longitude
* **time** - timestamps of records in songplays broken down into specific units
start_time, hour, day, week, month, year, weekday

<iframe width="560" height="315" src='https://dbdiagram.io/embed/65111d82ffbf5169f06a764f'> </iframe>

![Data Model Diagram](https://dbdiagram.io/embed/65111d82ffbf5169f06a764f)

The datamodel is designed as a Snowflake schema in a normalized (3NF) way so constant information about independent entities like Users, Songs and Artists are saved in separate tables so data redundancy is reduced and data integrity is prioritized.  All the tables have their primary keys and relationships between them are possible due to foreign keys. Thus, in the ETL we made sure SongPlays table saved the ArtistID and SongID even though this information was not in the log files. 

## Query example

```SELECT * 
FROM SONGPLAYS SP
JOIN SONGS S ON S.SONG_ID=SP.SONG_ID
JOIN ARTIST A ON A.ARTIST_ID=SP.ARTIST_ID
JOIN USERS U ON U.USER_ID=SP.USER_ID
JOIN TIME T ON T.START_TIME=SP.START_TIME
```
