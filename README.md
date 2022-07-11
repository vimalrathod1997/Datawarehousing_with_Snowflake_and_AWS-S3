# Datawarehousing_with_Snowflake_and_AWS-S3
A data warehousing project that pulls data from AWS S3 bucket and puts it in Snowflake. We also create a Snowpipe to automate this process.

## Overview
The data source for this project is kaggle.
https://www.kaggle.com/datasets/victorsoeiro/netflix-tv-shows-and-movies

The dataset consists of two files 
**Titles.csv** : This file contains data about shows and movies on netflix, their imdb votes, pg ratings,tmdb scores ,year of release etc.
**Credits.csv**: This file contains data about the actors and directors for the associated movies and shows in Titles.csv.

The data in the files is ingested in a PostgreSQL database among multiple tables for better granularity.

## Database and Tables
I created a database in postgreSQL named **db_netflix_snowflake**

The following tables are created under this database.

**Credits** This table consists all the data from Credits.csv

**Titles** This table consists all the data from Titles.csv

**IMDB** This table consists of IMDB ID, score and votes 

**TMDB** This table consists of TMDB score and popularity

**Actors** This table consists Actor's name, characters played etc.

**Directors** This table consists of the Directors and the movies he/sge directed.

## Project files

`Creating netflix database in PostgreSQL` : Contains code for connecting to database and creating tables and inserting data from csv file.

`Snowflake_Queries` : Contains Snowflake SQL queries for creating a data warehouse, database, table for warehouse data and building a snowpipe for auto ingestion of data from s3 bucket.

`AWS` : Contains a few commands to store data in S3 bucket, IAM role and policy in json format, Trust relationship, configuring event notifications.

`Data`: Data about netflix shows and movies.


## How to run
Please follow the steps mentioned in Steps_to_execute

## Errors and issues occured

1. The warehouse data created by our POSTGRESQL query created a `.csv` file that for some reason had an extra column(cross checked and the file had no extra column). The extra column was probably created or interpreted by snowflake because our file had data that had commas in it. Like genres, production house, Movie name
<ins>**What I did to resolve it **</ins>
When I created my file format added an extra line in it  `error_on_column_count_mismatch=False` this would ignore the column mismatch. After loading the data I verified and all the columns were uploaded without any data loss


2. The data uploaded in my first try was partially uploaded. From over 77k rows in my csv file, only around 12k rows were uploaded. I troubleshooted and figured out that the rows that didn't load into snowflake were the rows that had more comms. Like multiple genres, or multiple productions houses. Snowflake wasn't able to differentiate the commas that should be considered as delimiter and commas that are part of the value. 

<ins>**What I did to resolve it **</ins>
To fix this, I replaced all commas, that didn't act as delimiters, with backslashes and the data was uploaded successfully without any issues.





