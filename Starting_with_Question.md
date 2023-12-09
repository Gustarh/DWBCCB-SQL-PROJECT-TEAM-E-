Answer the following questions and provide the SQL queries used to find the answer.

Question 1: How many total listings are there in the Capetown AirBnB data? Would you say there are inactive listings in the data? If yes, What percentage of total listing is active or inactive?

SQL Queries:
SELECT
    COUNT(*) AS total_listings,
    SUM(CASE WHEN host_total_listings_count > 0 THEN 1 ELSE 0 END) AS active_listings,
    SUM(CASE WHEN host_total_listings_count = 0 THEN 1 ELSE 0 END) AS inactive_listings,
    (SUM(CASE WHEN host_total_listings_count = 0 THEN 1 ELSE 0 END) * 100/ COUNT(*)) AS percentage_inactive
FROM listings;

 Answer:The total listings is 8832 with active listings also pegged at 8832 indicating the absence of inactive listings.


Question 2: How many distinct hosts are present in the CapeTown AirBnB listing? How many listings does each of this distinct hosts have? Which host have the most listings

SQL Queries:

-- Count of distinct hosts:
SELECT
    COUNT(DISTINCT host_id) AS distinct_hosts_count
FROM listings;

-- Listings count for each distinct host and the host with the most listings:
SELECT
    host_id,
    COUNT(*) AS listings_count
FROM listings
GROUP BY host_id
ORDER BY listings_count DESC
LIMIT 10;

Answers: There are 4714 distinct hosts in the CapeTown AirBnB listing. The host with the id, 57218252 has the most listings with a count of 115.



Question 3: On Availability, What is the total number of listings available for more than 90 days a year? How many listings have availability for less than 30 days a year? What percentage of listings are available for instant booking?

SQL Queries:
-- Total number of listings available for more than 90 days a year:
SELECT
    COUNT(*) AS total_listings_more_than_90_days
FROM listings
WHERE CAST(availability_365 AS INTEGER) > 90;

-- Number of listings with availability for less than 30 days a year:
SELECT
    COUNT(*) AS listings_less_than_30_days
FROM listings
WHERE CAST(availability_365 AS INTEGER) < 30;

-- Percentage of listings available for instant booking:
SELECT
    (SUM(CASE WHEN instant_bookable = 't' THEN 1 ELSE 0 END) * 100 / COUNT(*)) AS percentage_instant_bookable
FROM listings;

Answer: There are 6806 total listings available for more than 90 days a year. There are also 954 listings available for less than 30 days a year. Also, the percentage of listings available for instant booking is 24.64% (to 2 d.p)




Question 4: What are the different types of properties available and their counts? Which property type has the highest average price? Write a query to show the correlation between review and pricing.

SQL Queries:
-- types of property and their counts:
SELECT 
	property_type,
	COUNT(*) AS property_type_count
FROM listings
GROUP BY property_type
ORDER BY property_type_count DESC;

-- Property Type with Highest Average Price:
SELECT
    property_type,
    AVG(CAST(REPLACE(REPLACE(price, '$', ''), ',', '') AS NUMERIC)) AS average_price
FROM listings
GROUP BY property_type
ORDER BY average_price DESC
LIMIT 10;
-- Correlation between Review Scores and Pricing:
SELECT
	AVG(review_score_rating) AS average_review_scores_rating,
	AVG(CAST(REPLACE(REPLACE(price, '$', ''), ',', '') AS NUMERIC)) AS average_price
FROM listings

Answer: The Entire Villa also has the highest average price at $9870.62. The correlation between review scores ratings and pricing on the average is 4.78 Vs $2728.76 (to @ d.p)



Question 5: How do individual listings or hosts perform compared to the average or top-performing ones in terms of ratings, pricing, and booking frequency?

SQL Queries:
SELECT 
	id,
	review_score_rating
FROM listings

-- To find the average rating across all listings:
SELECT
	AVG(review_score_rating) AS average_review_score_rating
FROM listings

-- Compare Pricing:
SELECT
    id,
	AVG(CAST(REPLACE(REPLACE(price, '$', ''), ',', '') AS NUMERIC)) AS average_price,
    CAST(REPLACE(REPLACE(price, '$', ''), ',', '') AS NUMERIC) AS listing_price
FROM listings
GROUP BY 1, 3;

-- Compare Booking Frequency:
SELECT
    id,
    instant_bookable
FROM listings;

-- To find the average instant booking frequency across all listings:
SELECT
    AVG(CASE WHEN instant_bookable = 't' THEN 1 ELSE 0 END) AS average_instant_booking_frequency
FROM listings;

Answer: With an average score of 4.78 across all ratings, the review does well. The listing id of 859186027628984540 also has the highest listing price with the average price also pegged at a similar value. The average instant booking frequency across all listings is also 0.25 (t0 2 d.p)



