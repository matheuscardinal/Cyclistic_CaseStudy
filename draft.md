# Cyclistic_CaseStudy

### This case study is part of the *Google Data Analytics Professional Certificate*. Somewhere where I hope to show how I think and can possibly help to bring some light with data-driven decision-making.


### Scenario:
You are a data analyst working in the marketing analyst team at Cyclistic, a bike-share company in Chicago.


![2022-03-16 (2)](https://user-images.githubusercontent.com/101608594/158488919-b132478a-391d-4a1d-94a4-abd91b89cb83.png)


Cyclistic offers share-bikes that are geotracked and locked into a network of docking stations where the bikes can be unlocked from one station and returned to any other station in the system anytime, compound by:
* +5,800 bycicles and +600 docking stations;
* offers reclining bikes, hand tricycles, and cargo bikes (more inclusive to people);
* 30% of the customers use them to commute to work each day;
* 8% of riders use the assistive options;
* Cyclistic users are more likely to ride for leisure.


## Phase 1 - ASK: Identifying the business task


Cyclistic's marketing strategy relied on building general awareness and appealing to broad consumer segments by its pricing plans flexibility: single-ride passes, full-day passes, and annual memberships.

  A. Cyclistic's finance analysts have concluded that annual members are much more profitable than casual riders.
  
  B. Marketing team believes that maximizing the number of annual members will be key to future growth.
  
  C. Rather than creating a marketing campaign that targets new customers, Marketing team believes there is a very good chance to convert casual riders into members.


#### TASK - DESIGN MARKETING STRATEGIES AIMED AT CONVERTING CASUAL RIDERS INTO MEMBERS


(My instincts) In order to do that:

1. Understand how annual members and casual riders differ - I am going to analyze where and when casual riders most use the bikes so we can target those riders with promotions before hand, specially for repetitive casual rider id's.

2. What could possibly motivate casual riders to buy a membership?
  - Study the possibility of summer season promotion to convert the casual riders into members.
  - Study the possibility of an incentive, rewards, and perks for milestones achieved by casual or members riders with business nearby the most used bike's docking stations, in order to link other business with Cyclistic's company so we can indirectly get to new customers, and also making sure that casual riders would have more benefits if they were members.


## Phase 2 - PREPARE: Preparing the data for analysis


To complete our study in order to identify trends and build a beautiful marketing campaign, I am going to analyse the historical data from Jan-2021 to Dec-2021, so we can get a better understanding on the customer's behaviour during the year and how it differs in different seasons of the year; days of the week; and length of each ride to create a milestone program achievement.

The dataset provided is compounded by 12 '.csv' files for each month of 2021, and is located on the [company's cloud](https://divvy-tripdata.s3.amazonaws.com/index.html) (Amazon Web Services). Each table contains 13 columns and each row consists of a single ride records.

*The dataset has been made available by Motivate International Inc., under this [data license agreement](https://ride.divvybikes.com/data-license-agreement). Note that data-privacy issues prohibit you from using riders’ personally identifiable information. This means that I won’t be able to connect pass purchases to credit card numbers to determine if casual riders live in the Cyclistic service area or if they have purchased multiple single passes.

![csv files](https://user-images.githubusercontent.com/101608594/158493930-72b0d12f-ec89-47b4-bee3-4fc4f953ab7f.png)


In the first impression and summary of the table you can recognise some inconsistencies in the data, such as:
* the date format input (it needs to be in TIMESTAMP type to perform some calculations and not face any problems when uploading the dataset into the databse for further analysis); 
* some empty cells on the columns G ('end_station_name') and H ('end_station_id') which must be the innacuracy of the bikes' geolocation at the end station (refer to columns K 'end_lat' and L 'end_lng').

![summary-raw-data](https://user-images.githubusercontent.com/101608594/158515301-6e3a6f14-066f-4469-b4e1-38a124508fb4.png)


To make sure we won't have any trouble when uploading our data into my database, first I changed the data format into 'YYYY-MM-DD HH:MM:SS' (TIMESTAMP):

![yyyy-mm-dd-resized](https://user-images.githubusercontent.com/101608594/158520703-9e08c387-ce3d-4ac6-bb68-33e279ec687f.png)
![yyyy-mm-dd-2-resized](https://user-images.githubusercontent.com/101608594/158520697-6db38c97-d969-4274-b048-b0f2a9c71b8a.png)


### New columns


To get more in-depth insights, I may as well create new columns into our datasets, having columns ready to perform calculations and analisys focused on the business tasks. A column named as 'ride_length_seconds' and a second new-column named as 'day_of_week'. 


#### 1. 'ride_length_seconds'

![RIDE_LENGTH_SECONDS](https://user-images.githubusercontent.com/101608594/158522164-9379d60f-38a2-4da2-82cb-b5805ad97c30.gif)

*Formula: =(D2-C2)*86400 (=('ended_at' - 'started_at') * 86400)*

The result returns with decimals, so to get some more consistency on my analysis, I formated the column's cells to 'Numbers' with no decimals, turning the data into INTEGER type.

![RIDE_LENGTH_SECONDS_2](https://user-images.githubusercontent.com/101608594/158523900-11ce93c9-f5b9-4700-9838-965cafd19118.gif)


#### 2. 'day_of_week'


Syntax:

=WEEKDAY(*serial_number*, [return type])

The 'serial_number' is a sequential number that represents the date of the day you are trying to find, *bingo* column C.
I used return_type '1' which is '1' for 'Sunday' through '7' for 'Saturday'.

![DAY-OF-WEEK](https://user-images.githubusercontent.com/101608594/158531145-080a346a-3ca8-40c0-b38c-17a55fe509b7.gif)


### Uploading the data into the database (PostgreSQL)

*Download PostgreSQL: [Link](https://www.postgresql.org/download/)*

*Documentation: [Link](https://www.postgresql.org/docs/14/index.html)(Fantastic software, very intuitive, if you are curious you will be able to easily get it set)*


If you haven't leaped over any step above you won't have to spend 14 hours like me to create a one destination table onto your database for further analysis, because all our dataset is consistenly formated to be uploaded onto the respectives datatypes:

*ride_id:CHARACTER VARYING,* 

*rideable_type:CHARACTER VARYING,* 

*started_at:TIMESTAMP WITHOUT TIME ZONE,* 

*ended_at:TIMESTAMP WITHOUT TIME ZONE,* 

*start_station_name:CHARACTER VARYING,* 

*start_station_id:CHARACTER VARYING,* 

*end_station_name:CHARACTER VARYING,* 

*end_station_id:CHARACTER VARYING,* 

*start_lat:NUMERIC,* 

*start_lng:NUMERIC,* 

*end_lat:NUMERIC,* 

*end_lng:NUMERIC,* 

*member_casual:CHARACTER VARYING,* 

*ride_length_seconds:INTEGER,* 

*day_of_week:INTEGER* 


#### Table summary

✨ Now, we have our table uploaded and ready to be used! ✨

Let's check if the data was correctly uploaded:


##### A - Data types

Yes, you can query it! I didn't know either, hey! Haha

![data-types-zoomed](https://user-images.githubusercontent.com/101608594/158545698-e9da2970-ec78-4167-a558-31c0f960be1c.png)

*Extra: Query syntax explanation*

![data-type-query-syntax](https://user-images.githubusercontent.com/101608594/158544361-415b9a8c-a45e-4ae9-8123-7988a024549e.png)


#### B - Number of rows

![Count](https://user-images.githubusercontent.com/101608594/158545269-83aaaf0e-4027-4ec0-a563-229281bfef4d.png)

BEAUTIFUL! ☜(ﾟヮﾟ☜)

## Phase 3 - PROCESS: Data Cleaning

Started pulling the data and as I had realised before, the results are really inconsistent by the amount of duplicates due the different bike geolocations, which also creates a few different 'station names' and 'station id's'.

![1](https://user-images.githubusercontent.com/101608594/158738125-0af07f3e-6387-4eb6-b372-c84203fb7295.png)

#### FIXING

NOTE: I know, by my analysis, that we need to get to 833 unique station id's.

To find the duplicates I grouped the 'station_name' and 'station_id' by the AVG latitude and longitude.

![2](https://user-images.githubusercontent.com/101608594/158739201-0cf32820-a175-4108-88df-1067b3fe3fd4.png)

Copied the table on Excel to check how many duplicates I had on my temp table, which was 18 dupes on 'station_id'.

To check, we query:
```purple
SELECT 
	dupes.station_id
	,COUNT(*) AS duplicates
FROM
(
		SELECT
			names.station_name
			,names.station_id
			,ROUND(AVG(lat),2) AS lat
			,ROUND(AVG(lng),2) AS lng
		FROM
		(
				SELECT
					DISTINCT start_station_name AS station_name
					,start_station_id AS station_id
					,start_lat AS lat
					,start_lng AS lng
				FROM
					cyclistic
				UNION DISTINCT
				SELECT 
					DISTINCT end_station_name AS station_name
					,end_station_id AS station_id
					,end_lat AS lat
					,end_lng AS lng
				FROM
					cyclistic
		) AS names
		GROUP BY
			names.station_name
			,names.station_id
) AS dupes
GROUP BY 
	dupes.station_id
ORDER BY 
	duplicates DESC
```

༼ つ ◕_◕ ༽つ BINGO! 18 duplicates!

![3](https://user-images.githubusercontent.com/101608594/158740616-8d0d869c-1ec7-45ea-a667-eb53184a1cee.png)

### Source of Truth!

Long story short, summary:

1. CREATE TABLE NEW TABLE ("source of truth")
	- First, I inserted only duplicated values
	- Deleted all the biased values or different station names due different locations 
	- Updated station_id value duplicated for two different locations
		
*Observation: I checked on the map all the duplicated values and matched by latitude and longitude, so it's correct! Give me credit, please, it was such a time consuming work and I am really happy I am documenting it all.*

#### _The queries would look like this:_

```purple
-- SOURCE OF TRUTH
--- CREATE TABLE
CREATE TABLE station 
	(station_id CHARACTER VARYING,
	station_name CHARACTER VARYING,
	lat NUMERIC,
	lng NUMERIC)
-- INSERT INTO
INSERT INTO station 
SELECT 
	dupes.*
FROM
(
		SELECT
			names.station_id
			,names.station_name
			,ROUND(AVG(lat),2) AS lat
			,ROUND(AVG(lng),2) AS lng
		FROM
		(
				SELECT
					DISTINCT start_station_name AS station_name
					,start_station_id AS station_id
					,start_lat AS lat
					,start_lng AS lng
				FROM
					cyclistic
				UNION DISTINCT
				SELECT 
					DISTINCT end_station_name AS station_name
					,end_station_id AS station_id
					,end_lat AS lat
					,end_lng AS lng
				FROM
					cyclistic
		) AS names
		GROUP BY
			names.station_name
			,names.station_id
) AS dupes
WHERE dupes.station_id IN
('TA1306000029',
'13300',
'TA1305000039',
'351',
'13074',
'631',
'331',
'LF-005',
'20215',
'KA1503000055',
'KA1504000168',
'TA1309000039',
'WL-008',
'13221',
'13099',
'TA1309000049',
'TA1307000041')


---- DELETING ROWS
DELETE FROM station
WHERE station_name is null
--
DELETE FROM station
WHERE 1=1
AND station_name IN ('Broadway & Wilson - Truman College Vaccination Site',
				'Halsted St & 18th St (Temp)',
				'DuSable Lake Shore Dr & Monroe St',
				'351',
				'Malcolm X College Vaccination Site',
				'Halsted & 63rd - Kennedy-King Vaccination Site',
				'Western & 28th - Velasquez Institute Vaccination Site',
				'DuSable Lake Shore Dr & North Blvd',
				'DuSable Lake Shore Dr & Diversey Pkwy',
				'DuSable Lake Shore Dr & Belmont Ave',
				'DuSable Lake Shore Dr & Ohio St',
				'DuSable Lake Shore Dr & Wellington Ave',
				'Lake Shore Dr & Ohio St',
				'Marshfield Ave & Cortland St')
				
				
-- UPDATING THE STATION ID THAT IS DUPLICATED FOR 2 DIFFERENT LOCATIONS
UPDATE public.station
	SET station_id= '331A'
	WHERE station_name = 'Pulaski Rd & 21st St';
	
	
--
SELECT * FROM station ORDER BY station_id
```

#### RESULT:

![4](https://user-images.githubusercontent.com/101608594/158923657-031dc601-5de5-4100-8f13-863a0476dcb7.png)

ALMOST THERE!

Now we insert the rest of unique 'station_id' and 'station_name' into our new table and hopE to get my table with 833 unique values to start my analysis, who else is also excited here?

```pink
-- INSERTING THE UNIQUE VALUES IN THE SOURCE OF TRUTH TABLE
INSERT INTO station 
SELECT 
	dupes.*
FROM
(
		SELECT
			names.station_id
			,names.station_name
			,ROUND(AVG(lat),2) AS lat
			,ROUND(AVG(lng),2) AS lng
		FROM
		(
				SELECT
					DISTINCT start_station_name AS station_name
					,start_station_id AS station_id
					,start_lat AS lat
					,start_lng AS lng
				FROM
					cyclistic
				UNION DISTINCT
				SELECT 
					DISTINCT end_station_name AS station_name
					,end_station_id AS station_id
					,end_lat AS lat
					,end_lng AS lng
				FROM
					cyclistic
		) AS names
		GROUP BY
			names.station_name
			,names.station_id
) AS dupes
WHERE dupes.station_id NOT IN -- CHANGE 'IN' TO 'NOT IN' AND YOU ARE DONE
('TA1306000029',
'13300',
'TA1305000039',
'351',
'13074',
'631',
'331',
'LF-005',
'20215',
'KA1503000055',
'KA1504000168',
'TA1309000039',
'WL-008',
'13221',
'13099',
'TA1309000049',
'TA1307000041')
```

The return was 834 rows.

![5](https://user-images.githubusercontent.com/101608594/158928168-0257ee7a-d16a-440b-a028-7ffbf4e04355.png)

I discovered that there was propably a human error at the 'station_id' for the 'station_name = Loomis St & 89th St' for the exactly same latitude and longitude.

![6](https://user-images.githubusercontent.com/101608594/158928179-698ab56b-292a-4f65-b22b-cf1a9be076cd.png)

Checked it on my table and found out that there is a number '2' missing onto the inserted first values.

I deleted and DONE! ✔

![7](https://user-images.githubusercontent.com/101608594/158928453-88843cbc-0579-45a4-b24b-77624b927a22.png)

### Does my data ROCCC?





























