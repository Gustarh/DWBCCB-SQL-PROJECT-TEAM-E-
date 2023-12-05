Fill out this file with a description of the issues that will be addressed by cleaning the data if there are any
Include the queries used to clean the data
Explore the data to reveal shape, patterns and its overview 

Data Quality Issue was identified in the Airbnb Table Dataset. Below is a summary of the data quality issue:

1. Duplicate Values. Duplicates Values were removed using this Syntax:
 SELECT DISTINCT(*) FROM listings

2. Handling Missing Values. Syntax:
SELECT* FROM listings WHERE

3. Deleted Amenities Column as it is irrelevant to the analysis. Syntax:
ALTER TABLE listings
DROP COLUMN amenities
			


