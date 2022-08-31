-- View and explore crime_details table

SELECT *
FROM la_crime.crime_details

-- Each line is a reported crime event with a unique ID as dr_no. 
-- Identify total number of crimes reported

SELECT COUNT(dr_no)
FROM la_crime.crime_details

-- Total: 546,207
-- Identify timeframe using time_location table

SELECT *
FROM la_crime.time_location

SELECT min(date_occ), max(date_occ)
FROM la_crime.time_location

-- Timeframe: January 1, 2020 - August 15, 2022 
-- Verify with join command

SELECT min(date_occ), max(date_occ)
FROM la_crime.crime_details
FULL JOIN la_crime.time_location
USING (dr_no)

-- Verified timeframe with both tables: January 1, 2020 - August 15, 2020
-- Verify count for both tables

SELECT COUNT(DISTINCT dr_no)
FROM la_crime.crime_details

SELECT COUNT(DISTINCT dr_no)
FROM la_crime.time_location

-- Both results show 546,207

-- Identify number of distinct crime codes

SELECT COUNT(DISTINCT crm_cd_desc)
FROM la_crime.crime_details

-- 137 diff types of crime code
-- Count by crime code

SELECT crm_cd_desc, COUNT(crm_cd_desc)
FROM la_crime.crime_details
GROUP BY crm_cd_desc
ORDER by COUNT(crm_cd_desc) DESC

-- Top five: Stolen vehicle, Battery - simple assault, Vandalism, Burglary from vehicle, burglary

-- Identify most common victim age

SELECT vict_age, COUNT(vict_age) 
FROM la_crime.crime_details
GROUP BY vict_age
ORDER BY COUNT(vict_age) DESC

-- Results show most common age is 0, with a total of 132,743, followed by age 30 with 12,608. Further verification needed... It could be that there was no direct victim in the reported crime (e.g. Stolen vehicle) and so age is 0 when reported.
-- Verify with crime code

SELECT crm_cd_desc, count(crm_cd_desc)
FROM la_crime.crime_details
WHERE vict_age = 0
GROUP BY crm_cd_desc
ORDER BY count(crm_cd_desc) DESC

-- Top 5 results out of 129: Stolen vehicle, theft from motor vehicle, burglary, vandalism and shoplifting do not directly involve a person resulting in the age 0 when reported. However, there are still significant results like Child Neglect with 146 reported, Kidnapping at 64, Child Abuse at 58 where Age 0 would make sense. More analysis is needed.

SELECT MIN(vict_age), MAX(vict_age)
FROM la_crime.crime_details

-- Results: Youngest age = -1 which could be incorrect reporting, while oldest age = 120.

SELECT crm_cd_desc, vict_age
FROM la_crime.crime_details
WHERE vict_age = -1 or vict_age = 120

-- Results show 21 records, -1 = 20, 120 = 1

SELECT CASE 
	WHEN vict_age <= 17 THEN '17 & Under'
	WHEN vict_age BETWEEN 18 and 35 THEN '18 - 35'
	WHEN vict_age BETWEEN 36 and 55 THEN '36 - 55'
	WHEN vict_age > 55 THEN 'Over 55'
	ELSE 'N/A'
	END AS vict_age_group,
	COUNT(vict_age) AS total_vict_in_age_group
FROM la_crime.crime_details
GROUP BY vict_age_group
ORDER BY total_vict_in_age_group DESC

-- The top age group most affected by crime is 18-35, followed by 36-55, then 17 & under, lastly over 55. 

SELECT crm_cd_desc, COUNT(crm_cd_desc)
FROM la_crime.crime_details
WHERE vict_age BETWEEN 18 and 35
GROUP BY crm_cd_desc
ORDER BY COUNT(crm_cd_desc) DESC

-- The top 5 crimes committed for the 18-35 age group is Burglary from vehicle, battery - simple assault, intimate partner - simple assault, assault with deadly weapon, and theft of identity. 
-- Let's do it for all age groups

SELECT CASE 
	WHEN vict_age <= 17 THEN '17 & Under'
	WHEN vict_age BETWEEN 18 and 35 THEN '18 - 35'
	WHEN vict_age BETWEEN 36 and 55 THEN '36 - 55'
	WHEN vict_age > 55 THEN 'Over 55'
	ELSE 'N/A'
	END AS vict_age_group,
	crm_cd_desc,
	COUNT(crm_cd_desc)
FROM la_crime.crime_details
GROUP BY crm_cd_desc, vict_age_group
ORDER BY COUNT(crm_cd_desc) DESC

-- Which sex is most affected by crime?

SELECT vict_sex, COUNT(vict_sex)
FROM la_crime.crime_details
GROUP BY vict_sex
ORDER BY COUNT(vict_sex) DESC

-- The data shows M as most affected with 228817, followed by F with 200660, X with 44258 and H with 62. 
-- Top crime for each sex?

SELECT crm_cd_desc, COUNT(crm_cd_desc), vict_sex
FROM la_crime.crime_details
GROUP BY crm_cd_desc, vict_sex
ORDER BY COUNT(crm_cd_desc) DESC

-- Verify data per sex

SELECT crm_cd_desc, COUNT(crm_cd_desc), vict_sex
FROM la_crime.crime_details
WHERE vict_sex = 'M'
GROUP BY crm_cd_desc, vict_sex
ORDER BY COUNT(crm_cd_desc) DESC

-- 

SELECT crm_cd_desc, COUNT(crm_cd_desc), vict_sex
FROM la_crime.crime_details
WHERE vict_sex = 'F'
GROUP BY crm_cd_desc, vict_sex
ORDER BY COUNT(crm_cd_desc) DESC

-- 

SELECT crm_cd_desc, COUNT(crm_cd_desc), vict_sex
FROM la_crime.crime_details
WHERE vict_sex = 'X'
GROUP BY crm_cd_desc, vict_sex
ORDER BY COUNT(crm_cd_desc) DESC

-- 

SELECT crm_cd_desc, COUNT(crm_cd_desc), vict_sex
FROM la_crime.crime_details
WHERE vict_sex = 'H'
GROUP BY crm_cd_desc, vict_sex
ORDER BY COUNT(crm_cd_desc) DESC

-- First code above without WHERE syntax works, results are just a little messy. 
-- Now diving into the time_location table

SELECT *
FROM la_crime.time_location

-- Data includes: date, time, area, premise description and specific location

SELECT COUNT(DISTINCT premis_desc)
FROM la_crime.time_location

-- There are 306 distinct premises in this dataset.

SELECT premis_desc, COUNT(premis_desc)
FROM la_crime.time_location
GROUP BY premis_desc
ORDER BY COUNT(premis_desc) DESC

-- The top 5 premises where crimes happen are: Street, Single Family Dwelling, Multi-Unit Dwelling, Parking Lot, and Other Business
-- What are the most common crimes happening in these top locations? 

SELECT c.crm_cd_desc, t.premis_desc, COUNT(t.premis_desc)
FROM la_crime.crime_details AS c
LEFT JOIN la_crime.time_location AS t
USING (dr_no)
GROUP BY t.premis_desc, c.crm_cd_desc
ORDER BY COUNT(t.premis_desc) DESC
LIMIT 10

-- The data shows that top crimes predominately involve theft, burglary, assault and vandalism. 
-- Where are crimes typically committed in Los Angeles area?

SELECT DISTINCT(area_name)
FROM la_crime.time_location

-- There are 21 geographical areas aka 'Patrol Divisions'. These areas identify which jurisdication will be responsible for the reported crime.

SELECT area_name, COUNT(area_name)
FROM la_crime.time_location
GROUP BY area_name
ORDER BY COUNT(area_name) DESC

-- Top 5 Patrol Divisions where crime has been reported are: Central, 77th Street, Pacific, Southwest, and Hollywood

SELECT c.crm_cd_desc, t.area_name, COUNT(t.area_name)
FROM la_crime.crime_details AS c
LEFT JOIN la_crime.time_location AS t
USING (dr_no)
GROUP BY t.area_name, c.crm_cd_desc
ORDER BY COUNT(t.area_name) DESC
LIMIT 10

-- Most common crimes committed in high crime areas involve a stolen vehicle, burglary, assault and vandalism.

-- What time do most crimes occur? 

SELECT time_occ
FROM la_crime.time_location

SELECT CASE 
	WHEN time_occ BETWEEN TIME '04:00:00' AND TIME '11:59:59' THEN 'Morning'
	WHEN time_occ BETWEEN TIME '12:00:00' AND TIME '16:59:59' THEN 'Afternoon' 
	WHEN time_occ BETWEEN TIME '17:00:00' AND TIME '21:59:59' THEN 'Evening'
	WHEN time_occ BETWEEN TIME '22:00:00' AND TIME '23:59:59' THEN 'Night'
	WHEN time_occ BETWEEN TIME '00:00:00' AND TIME '3:59:50' THEN 'Night'
	ELSE 'N/A'
	END AS time_groups,
	COUNT(time_occ) AS total_crimes_cmtd
FROM la_crime.time_location
GROUP BY time_groups
ORDER BY total_crimes_cmtd DESC

-- Most crimes occur in the Evening and the Afternoon, so between 12:00 PM - 10:00 PM. 
-- Which crime occurs most frequently in each time group?

SELECT c.crm_cd_desc,
	CASE 
	WHEN t.time_occ BETWEEN TIME '04:00:00' AND TIME '11:59:59' THEN 'Morning'
	WHEN t.time_occ BETWEEN TIME '12:00:00' AND TIME '16:59:59' THEN 'Afternoon' 
	WHEN t.time_occ BETWEEN TIME '17:00:00' AND TIME '21:59:59' THEN 'Evening'
	WHEN t.time_occ BETWEEN TIME '22:00:00' AND TIME '23:59:59' THEN 'Night'
	WHEN t.time_occ BETWEEN TIME '00:00:00' AND TIME '3:59:50' THEN 'Night'
	ELSE 'N/A'
	END AS time_groups,
	COUNT(t.time_occ) AS total_crimes_cmtd
FROM la_crime.time_location AS t
LEFT JOIN la_crime.crime_details AS c
USING (dr_no)
GROUP BY time_groups, crm_cd_desc
ORDER BY total_crimes_cmtd DESC

-- Data shows that for the time groups Morning, Afternoon, Evening and Night, the most reported crime is of a stolen vehicle.

