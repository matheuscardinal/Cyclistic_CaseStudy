# Cyclistic_CaseStudy

### This case study is part of the *Google Data Analytics Professional Certificate*. Somewhere where I hope to show how I think and can possibly help to bring some light with data-driven decision-making.

#### Scenario:
You are a data analyst working in the marketing analyst team at Cyclistic, a bike-share company in Chicago.

![2022-03-16 (2)](https://user-images.githubusercontent.com/101608594/158488919-b132478a-391d-4a1d-94a4-abd91b89cb83.png)

Cyclistic offers share-bikes that are geotracked and locked into a network of docking stations where the bikes can be unlocked from one station and returned to any other station in the system anytime, compound by:
* +5,800 bycicles and 600 docking stations;
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
1. Understand how annual members and casual riders differ - I am going to analyze where and when casual riders most use the bikes so we can target those riders with promotions before hand, specially for repetitive member id's.

2. What could possibly motivate casual riders to buy a membership?
  - Study the possibility of summer season promotion to convert the casual riders into members.
  - Study the possibility of an incentive, rewards, and perks for milestones achieved by casual or members riders with business nearby the most used bike's docking stations, in order to link other business with Cyclistic's company so we can indirectly get to new customers, and also making sure that casual riders would have more benefits if they were members.

## Phase 2 - PREPARE: Preparing the data for analysis

To complete our study in order to identify trends and build a beautiful marketing campaign, I am going to analyse the historical data from Jan-2021 to Dec-2021, so we can get a better understanding on the customer's behaviour during the year and how it differs in different seasons of the year; days of the week; and length of each ride to create a milestone program achievement.

The dataset provided is compounded by 12 '.csv' files for each month of 2021, and is located on the [company's cloud](https://divvy-tripdata.s3.amazonaws.com/index.html) (Amazon Web Services). Each table contains 13 columns and each row consists of a single ride records.
![csv files](https://user-images.githubusercontent.com/101608594/158493930-72b0d12f-ec89-47b4-bee3-4fc4f953ab7f.png)

In the first impression and summary of the table you can recognise some inconsistencies in the data, such as:
* the date format input (it needs to be in TIMESTAMP type to perform some calculations and not face any problems when uploading the dataset into the databse for further analysis); 
* some empty cells on the columns G ('end_station_name') and H ('end_station_id') which must be the innacuracy of the bikes' geolocation at the end station (refer to columns K 'end_lat' and L 'end_lng').

![summary-raw-data](https://user-images.githubusercontent.com/101608594/158515301-6e3a6f14-066f-4469-b4e1-38a124508fb4.png)

To make sure we won't have any trouble when uploading our data into my database, first I changed the data format into 'YYYY-MM-DD HH:MM:SS' (TIMESTAMP):

![yyyy-mm-dd-resized](https://user-images.githubusercontent.com/101608594/158520703-9e08c387-ce3d-4ac6-bb68-33e279ec687f.png)
![yyyy-mm-dd-2-resized](https://user-images.githubusercontent.com/101608594/158520697-6db38c97-d969-4274-b048-b0f2a9c71b8a.png)

And may as well create new columns into our datasets to make my future 'me' happier because we will have columns ready to perform some calculations and analisys.
A column named as 'ride_length_seconds' and a second new-column named as 'day_of_week'. 

#### 1. 'ride_length_seconds'
![RIDE_LENGTH_SECONDS](https://user-images.githubusercontent.com/101608594/158522164-9379d60f-38a2-4da2-82cb-b5805ad97c30.gif)

*Formula: =(D2-C2)*86400 (=('ended_at' - 'started_at') * 86400)*

The result returns with decimals, so to get some more consistency on my analysis, I formated the column's cells to 'Numbers' with no decimals, turning the data into INTEGER type.

![RIDE_LENGTH_SECONDS_2](https://user-images.githubusercontent.com/101608594/158523900-11ce93c9-f5b9-4700-9838-965cafd19118.gif)

#### 2. 'day_of_week'





























