# Telecom-Network-Analysis
Analyzing the strength of a telecom network involves a comprehensive examination of various performance metrics to ensure an optimal and reliable user experience. Leveraging SQL queries is an effective way to derive meaningful insights from telecom network data. By employing queries such as calculating the average rating by operator, network type, and state, network analysts can gain valuable information about the overall performance trends and regional variations within the network. Additionally, delving into call drop analysis, both by network type and operator, provides crucial details about potential areas for improvement and helps prioritize network optimization efforts. Identifying the top-performing states based on average ratings allows for a focused approach in enhancing network quality in specific geographical regions. Furthermore, assessing the variability in ratings among operators aids in pinpointing those with consistently high or low performance, facilitating targeted interventions. The ability to calculate the percentage of call drops by operator offers insights into the relative impact of network issues across different service providers. Exploring the distribution of network types in each state provides a granular understanding of the technological landscape, enabling network planners to tailor improvements based on specific infrastructure needs. These SQL queries serve as powerful tools for telecom network analysts, guiding them in making informed decisions and optimizing network strength to meet the ever-evolving demands of users in a dynamic telecommunications landscape.

--Display the records of VI having network type 4G and rating 
SELECT operator, network_type, rating
FROM network_performance
WHERE operator = 'VI' AND network_type = '4G';
-- Find the average rating of each operator 
SELECT operator, AVG(rating) AS avg_rating
FROM network_performance
GROUP BY operator;
--
SELECT operator, AVG(rating) AS avg_rating
FROM network_performance
GROUP BY operator
HAVING AVG(rating) = (SELECT MAX(AVG(rating)) FROM network_performance GROUP BY operator);



--Display the count of reason for call drop 
SELECT calldrop_category, COUNT(*) AS count
FROM network_performance
GROUP BY calldrop_category;
--Retrieve the states with Highest and lowest average ratings 
SELECT state_name, AVG(rating) AS avg_rating
FROM network_performance
GROUP BY state_name
ORDER BY avg_rating DESC
LIMIT 1;

SELECT state_name, AVG(rating) AS avg_rating
FROM network_performance
GROUP BY state_name
ORDER BY avg_rating
LIMIT 1;

SELECT operator, network_type, COUNT(*) AS type_count
FROM network_performance
GROUP BY operator, network_type;

SELECT
    CASE
        WHEN state_name IN ('Bihar', 'West Bengal', 'Nagaland') THEN 'Northeast'
        WHEN state_name IN ('Maharashtra', 'Rajasthan') THEN 'West'
         WHEN state_name IN ('Himachal Pradesh', 'Uttar Pradesh') THEN 'Central'
        ELSE 'Other'
    END AS Region,
    AVG(rating) AS avg_rating
FROM network_performance
GROUP BY region;

SELECT operator,latitude,longitude
FROM network_performance 
WHERE latitude > 20 AND state_name LIKE 'Maharashtra';

SELECT operator,network_type,calldrop_category,state_name
FROM network_performance
WHERE calldrop_category LIKE '_atisfactory';
-- Count of different network types in each state
SELECT state_name, network_type, COUNT(*) AS network_type_count
FROM network_performance
GROUP BY state_name, network_type;

-- Operators with high variability in ratings
SELECT operator, STDDEV(rating) AS rating_deviation
FROM network_performance
GROUP BY operator
ORDER BY rating_deviation DESC
LIMIT 5;
-- Count of Network Performance Records by Operator and State
SELECT operator, state_name, COUNT(*) AS record_count
FROM network_performance
GROUP BY operator, state_name
ORDER BY record_count DESC;

