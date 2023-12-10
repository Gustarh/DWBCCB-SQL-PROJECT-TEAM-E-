Answer the following questions and provide the SQL queries used to find the answer.

Question 1: How many total listings are there in the Capetown AirBnB data? Would you say there are inactive listings in the data? If yes, What percentage of total listing is active or inactive?

SQL Queries:
SELECT
    COUNT(*) AS total_listings,
    SUM(CASE WHEN host_total_listings_count > 0 THEN 1 ELSE 0 END) AS active_listings,
    SUM(CASE WHEN host_total_listings_count = 0 THEN 1 ELSE 0 END) AS inactive_listings,
    (SUM(CASE WHEN host_total_listings_count = 0 THEN 1 ELSE 0 END) * 100.0 / COUNT(*)) AS percentage_inactive
FROM listings;

 Answer:The total listings is 21120 with active listings also pegged at 21120 indicating the absence of inactive listings.

Question 2: How many distinct hosts are present in the CapeTown AirBnB listing? How many listings does each of this distinct hosts have? Which host have the most listings

SQL Queries:

-- Count of distinct hosts
SELECT
    COUNT(DISTINCT host_id) AS distinct_hosts_count
FROM listings;

-- Listings count for each distinct host and the host with the most listings
SELECT
    host_id,
    COUNT(*) AS listings_count
FROM listings
GROUP BY host_id
ORDER BY listings_count DESC
LIMIT 10;


Answers: There are 11303 distinct hosts in the CapeTown AirBnB listing. The host with the id, 487012580 has the most listings with a count of 155 followed closely by host 3961453 with a count of 150. 



Question 3: On Availability, What is the total number of listings available for more than 90 days a year? How many listings have availability for less than 30 days a year? What percentage of listings are available for instant booking?

SQL Queries:
-- Total number of listings available for more than 90 days a year
SELECT
    COUNT(*) AS total_listings_more_than_90_days
FROM listings
WHERE CAST(availability_365 AS INTEGER) > 90;
-- Number of listings with availability for less than 30 days a year
SELECT
    COUNT(*) AS listings_less_than_30_days
FROM listings
WHERE CAST(availability_365 AS INTEGER) < 30;

-- Percentage of listings available for instant booking
SELECT
    (SUM(CASE WHEN instant_bookable = 't' THEN 1 ELSE 0 END) * 100.0 / COUNT(*)) AS percentage_instant_bookable
FROM listings;

Answer: There are 14998 total listings available for more than 90 days a year. There are also 3842 listings available for less than 30 days a year. Also, the percentage of listings available for instant booking is 27.8125 %.




Question 4: What are the different types of properties available and their counts? Which property type has the highest average price? Write a query to show the correlation between review and pricing.

SQL Queries:
SELECT 
property_type,
	COUNT(*) AS property_type_count
FROM listings
GROUP BY 1
ORDER BY 2 DESC;
-- Property Type with Highest Average Price
SELECT
    property_type,
    AVG(CAST(REPLACE(REPLACE(price, '$', ''), ',', '') AS NUMERIC)) AS average_price
FROM listings
GROUP BY property_type
ORDER BY average_price DESC
LIMIT 10;
-- Correlation between Review Scores and Pricing
SELECT
	AVG(review_score_rating) AS average_review_scores_rating,
	AVG(CAST(REPLACE(REPLACE(price, '$', ''), ',', '') AS NUMERIC)) AS average_price
FROM listings
WHERE review_score_rating IS NOT NULL


Answer: TThe top property types with the most counts include: Entire rental unit (7932), Entire Home (4063), Private room in home (1375), Entire condo (1060), Entire guest suite (1011). The Entire Villa also has the highest average price at $13742.745891276865. The correlation between review scores ratings and pricing on the average is 4.6958293224731581 Vs 2328.1151425398000740




Question 5: How do individual listings or hosts perform compared to the average or top-performing ones in terms of ratings, pricing, and booking frequency?

SQL Queries:
SELECT 
	id,
	review_score_rating
FROM listings
WHERE review_score_rating IS NOT NULL

-- To find the average rating across all listings
SELECT
	AVG(review_score_rating) AS average_review_score_rating
FROM listings
WHERE review_score_rating IS NOT NULL

-- Compare Pricing
SELECT
    id,
	AVG(CAST(REPLACE(REPLACE(price, '$', ''), ',', '') AS NUMERIC)) AS average_price,
    CAST(REPLACE(REPLACE(price, '$', ''), ',', '') AS NUMERIC) AS listing_price
FROM listings
GROUP BY 1, 3;

-- Compare Booking Frequency
SELECT
    id,
    instant_bookable
FROM listings;

-- To find the average instant booking frequency across all listings
SELECT
    AVG(CASE WHEN instant_bookable = 't' THEN 1 ELSE 0 END) AS average_instant_booking_frequency
FROM listings;



Answer:With an average score of 4.6958293224731581 across all ratings, the review does well. The listing id of 52754312 also has the highest listing price with the average price also pegged at a similar value. The average instant booking frequency across all listings is also 0.278125



