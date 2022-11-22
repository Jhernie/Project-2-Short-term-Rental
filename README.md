# Project-2-Short-term-Rental

## Summary
Our group focused on exploring the concentration of Air B&B listings in Chicago, IL. To do this, we utilized RapidAPI, a third party API hub that hosts a number of API connections. In conjunction, we found a Kaggle dataset in .csv form which included Air B&B listings for all US geographies 


### Extract:

* Use AirB&B api hosted in RapidAPI hub to query using the [Search Property by Place end point](https://rapidapi.com/DataCrawler/api/airbnb19/)

* Do this 5 times because specified endpoint does not allow a range of bedroom counts, but rather specifies a fixed bedroom count per query

* Find a Kaggle dataset that shows AirB&B listings in .csv form for [all US geographies](https://www.kaggle.com/datasets/kritikseth/us-airbnb-open-data).

* Review both datasources to see if there were columns or datapoints that overlapped, and to see which pieces were new

* Fields that we found in API and not Kaggle dataset = "Rating" 


### Transform: 

* Initially we aimed to build our database using the PostGreSQL tool. We struggled to import the .csv file into PGAdmin, though. We suspect that this is likely due to a number of special characters that were used in the "listing name" field. Thus, even though we specified the data type of the aforementioned field as "VARCHAR," the import failed multiple times.

* Instead, we pulled both datasets into jupyter notebook file as pandas dataframes to begin cleaning. First we removed fields from the Kaggle .csv file that weren't relevant to our pursuits (such as 

* Append multiple dataframes pulled from the RapidAPI AirB&B "Search Property by Place" end point using Pandas.append (because the end point ran on a fixed bedroom count and thus, needed to be run 5 times for 1 - 5 bedrooms

* Merged API dataframe and kaggle dataframe using an inner merge to create a new table, preserving only the content that existed in the "Chicago" area and dropping all values from the kaggle dataset that were associated with geographies other than "Chicago."


### Load:

* Create postgres database
* Load into table
* Once created, add UN and PW into connection string
* Run last part of the code to push dataframes into postgres database
* If you have dataframes, use df.to_sql


## Considerations:

* RapidAPI is an opensource tool that anyone can contribute to. As such, accuracy of the data varies. Our API was created by user "DataCrawler."


