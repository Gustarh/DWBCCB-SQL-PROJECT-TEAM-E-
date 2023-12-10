Question 1:Which host location has the highest listings?

SQL Queries:SELECT host_location, COUNT(*)AS Total_listings
FROM listings
GROUP BY host_location
ORDER by Total_listings DESC
LIMIT 10

Answer:The Host location with the highest listing CapeTown,South AFrica with a total of 13758

Question 2:What are the Bottom 5 Host location listings?

SQL Queries:SELECT host_location, COUNT(*)AS Total_listings
FROM listings
GROUP BY host_location
ORDER by Total_listings 
LIMIT 5

Answer: (Kampala,Uganda),(Emalahleni, SouthAfrica), (Veneto,Italy), (Jersey),(Bruges,Belgium)are the bottom 5 location in terms of listing. They all have 1 listing each

Question 3:Which host locations have the poorest response time(within an hour)?

SQL Queries: 
SELECT host_location, AVG(CASE WHEN host_response_time='within an hour' THEN 1
                               WHEN host_response_time='within a few hours' THEN 2
                               WHEN host_response_time='a few days or more' THEN 3
                               WHEN host_response_time='within a day' THEN 4
                               ELSE 5
                               END) AS Avg_response_time
FROM listings
GROUP BY host_location
ORDER BY Avg_response_time DESC

Answer:(Edinburgh, United kingdom),(Marmaris,Turkey), (Sweden), (Stavanger, Noraway),(Mumbai,India)

Question 4:What are the distinct property type in airbnb?

SQL Queries:SELECT DISTINCT property_type
FROM listings

Answer: Entire Vacation home, private room, hut, entire town house .......

Question 5:What is the most popular neighbourhood?

SQL Queries:SELECT neighbourhood,COUNT(*)AS Total_listings
FROM listings
GROUP BY neighbourhood
ORDER BY Total_listings DESC

Answer:Ward 115 is the most popular neighbourhood with total listing of 4459
