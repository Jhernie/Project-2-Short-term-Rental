# Project-2-Short-term-Rental

## Summary
Our group (Jhernie Evangelista, Georgia Myers, Lija Hoffman, Niurika Gonzalez) focused on exploring the concentration of Air B&B listings in Chicago, IL. To do this, we utilized RapidAPI, a third party API hub that hosts a number of API connections. In conjunction, we found a Kaggle dataset in .csv form which included Air B&B listings for all US geographies. These resources together told a compelling story about each Air B&B listing in the Chicago area and gave us information that we could use for further analysis. 


### Extract:

1. We used the AirB&B API hosted in the RapidAPI hub to query using the [Search Property by Place end point.](https://rapidapi.com/DataCrawler/api/airbnb19/)

<img width="1103" alt="Screenshot 2022-11-22 at 6 29 50 PM" src="https://user-images.githubusercontent.com/37047605/203448590-f272b365-95c5-4d02-81af-70e2d9de3857.png">

2. We did this 5x because the specified endpoint would not allow a range of guest limits, but rather specified a fixed guest count per query.

3. We then found a Kaggle dataset that showed AirB&B listings in .csv form for [all US geographies](https://www.kaggle.com/datasets/kritikseth/us-airbnb-open-data).

4. Finally, we identified fields that we found only in the API resource and not in the Kaggle dataset, justifying the need to identify more than one datasource. The fields are listed below:
    * "Rating" 
    * "Adults"
    * "Baths" 
    * "Beds"

### Transform: 

* Initially we aimed to build our database using the PostgreSQL tool at the onset. We struggled to import the .csv file into pgAdmin, though. We suspect that this is likely due to a number of special characters that were used in the "listing name" field. Thus, even though we specified the data type of the aforementioned field as "VARCHAR," the import failed multiple times.

* Instead, we converted all datasets into Pandas dataframes by pulling them into a jupyter notebook file. 
   * We used the ".read_csv" Pandas method for the Kaggle data after removing fields from the Kaggle .csv file that weren't relevant to our pursuits (such as "neighbourhood-group" which presented no data, and "neighbourhood" which presented data as both an integer and as a string).
   
   * We created a function using a "for loop" to extract the relevant datapoints from the Air B&B api.

* We appended multiple dataframes pulled from the RapidAPI AirB&B "Search Property by Place" end point using the Pandas .concat method. This created one main table for all of the AirB&B API data pulls.

* In order to merge, it was required that we: 
    1. Identify the field that was consistent between all of the tables (primary key). In this case, we used the "unique listing ID."

    2. Ensure that both fields were the same data type. Upon ingest, one field was an "object" and the other was an "int." To remedy this, we cast both fields to "int" using the .astype(int) function in Pandas.

* Finally, we merged the API dataframe and Kaggle dataframe using an inner merge to create a new table, preserving only the content that existed in the "Chicago" area and dropping all values from the kaggle dataset that were associated with geographies other than "Chicago." 


### Load:

* With the objective of eventually being able to share the freshly structured tables externally, we created a new database using PostgreSQL and the pgAdmin tool.
* Using SQLAlchemy, we created an engine to load the main table into the database using the following steps:
   1. Create a "config.py" file with necessary credentials
   2. Build the "engine" by configuring the connection string with credentials, referencing the config file
   3. Run "df.to_sql" to initiate the connection
   4. Run the code to push the dataframe into the database
* Once connected, a user can perform queries on the table using SQL, such as the query depicted below.
![Screenshot (65)](https://user-images.githubusercontent.com/37047605/203453422-db1a4174-e23e-4f77-900d-7f81638dfaa3.png)

## Considerations:

* *Later in the assignment, we realized that it would be more architecturally sound to preserve each table from the API pull as it's own dataframe to then load into our specified database (instead of appending each table to each other, and merging the tables together). This would have allowed for queryies to be performed on each table, or joins that accomplished the same goals, while preserving the integrity of each data pull. With more time, we could explore this method of data architecting.

* RapidAPI is an opensource tool that anyone can contribute to. As such, accuracy of the data varies. Our API was created by user "DataCrawler." We explored a different way to access the AirB&B API via another 3rd party resource. However, [this method](https://github.com/nderkach/airbnb-python) required digging into the html on the AirB&B site to find the relevant API end points used by AirB&B internally. As such, we left that method to be explored another day.

* RapidAPI has a paid tier. On the "freemium" version, an individual is heavily limited by the number of queries they can pull. We purchased more queries for our purposes, but it's worth noting for anyone who is interested in replicating the project that it can quickly become costly to query this API resource.

* With more time, we would have web-scraped the AirBnb platform to gain missing data and add it as a table into the database.

* Though we did not use the time to analyze the datasets, analysis could be performed on a number of metrics included in the tables procured. Below is a list of a few questions that could be explored with this data: 
    * Is there a trend to be examined amongst listing titles and rate of occupancy per listing?
    * Is there a trend to be examined amongst listing titles and overall rating?
    * Does the guest count cap affect the rate of bookings per month for each listing?

* Further analysis could include additional datasources such as median home prices to examine the relationship between the concentration of Air B&B listings in a particular geography, and the average median home price/value.


