# EDA On TeleCom Data
It is a Group Project done at INSAID.
## Summarization of Learning

### 9 steps of EDA
- Define Problem
- Choose right tools
- Collection of data
- Pre-profile
- Pre processing of data (Clean, remove unnecessary, add relevant data)
- Post-profile
- Ask right Questions
- Conclusion or Summarization
- Actionable Insights

## Introduction
### Problem Statement
InsaidTelecom, one of the leading telecom players, understands that customizing offering is very important for its business to stay competitive. Currently, InsaidTelecom is seeking to leverage behavioral data from more than 60% of the 50 million mobile devices active daily in India to help its clients better understand and interact with their audiences.

In this consulting assignment, Insaidians are expected to build a dashboard to understand user's demographic characteristics based on their mobile usage, geolocation, and mobile device properties. Doing so will help millions of developers and brand advertisers around the world pursue data-driven marketing efforts which are relevant to their users and catered to their preferences

*Please remember this is an analytics consulting hence, your efforts in terms of finding user behavior is going to directly impact the company's offerings. Do help the company understand what is the right way forward and suggest actionable insights from marketing and product terms. *

## Summary Of the Data
In this assignment, you are going to study the demographics of a user (gender and age) based on their app download and usage behaviors.

The data schema can be represented in the following table:

gender_age_train - Devices and their respective user gender, age and age_group

phone_brand_device_model - device ids, brand, and models phone_brand: note that few brands are in Chinese

Brand Name	Brand English Mapping
'华为'	'Huawei'
'小米'	'Xiaomi'
'三星'	'Samsung'
'vivo'	'vivo'
'OPPO'	'OPPO'
'魅族'	'Meizu'
'酷派'	'Coolpad'
'乐视'	'LeEco'
'联想'	'Lenovo'
'HTC'	'HTC'

**events_data** - when a user uses mobile on INSAID Telecom network, the event gets logged in this data. Each event has an event id, location (lat/long), and the event corresponds to frequency of mobile usage. timestamp: when the user is using the mobile.

Download the DataSets for events_data from : events_data.csv

Download the DataSets for gender_age_train & phone_brand_device_model onto Python by connecting to the below provided MySQL instance.

host -----> 'cpanel.insaid.co'

user -----> 'student'

passwd -----> 'student'

database -----> 'Capstone1'

## Review Basic Information on the available Data 
- Events data has 7 columns and 3252950 rows
- 4 numeric fields and 3 object type
- There is a timestamp which denotes when a particular event occurred.
- Maximum number of events occurred in Delhi which means Delhi users are using their   phone maximum number of times
- Missing values present in fields device_id, longitude, latitude and state

## gender_age_train
- Total 4 columns and 74645 rows (no missing values)
- 2 numeric type and 2 object type
- There are 2 types of gender who are using mobile with this network
- Males are the most mobile users
- age field is right skewed, mean : 31.41 and median : 29
- 50% of the mobile users are in the age range between 25 and 36
- Males in the age between 23 and 26 are using their mobiles maximum number of times

## phone_brand_device_model
- Total 3 columns and 87726 rows (no missing values)
- One numeric and 2 object types
- Both phone_brand and device_model fields have Chinese names which need to be converted to equivalent English

## Pandas profiling before data pre-processing
**Dataset 1** : **InsaidTelecom Events DataFrame**

**Dataset Statistics**
Number of variables : 7
Number of observations : 3252950
Missing cells : 1676
Missing cells (%) : <0.1%

**Variable types**
Categorical : 3
Numerical : 4

**Observations**
- event_id is unique which means no duplicate events.
- device_id has only 60865 distinct values. It has 453 missing values.
- device_id needs to be converted to float
- timestamp has no missing values
- There are missing values in longitude and latitude. It has 423 missing values
- city has a total 933 distinct values and no missing values
- Delhi has maximum number of events per day
- state has 32 distinct values and 377 missing values

**Dataset 2** :** InsaidTelecom Demographic(gender_age) DataFrame**

**Dataset Statistics**
Number of variables : 4
Number of observations : 74645
Missing cells : No missing values

**Variable types**
Categorical : 2
Numerical : 2

**Observations**
- device_id is unique which means no duplicate device.
- More Male mobile users compared to Female
- age of mobile users , minimum age : 1; maximum age : 96
- age is right skewed so use median instead of mean
- Male 23-26 age group of people are maximum using the mobile

**Dataset 3** : **InsaidTelecom Phone Details(phone_brand_device_model) DataFrame**

**Dataset Statistics**
Number of variables : 3
Number of observations : 87726
Missing cells : No missing values

**Variable types**
Categorical : 2
Numerical : 1

**Observations**
- device_id is unique which means no duplicate device.
- phone_brand and device_model has no missing values
- Total 116 phone brand out of which Xiaomi is preffered the most by the users. Almost 25% market is owned by Xiaomi followed by Samsung
- Total 1467 different mobile models are used as per the data
- Volume of Phone details is more than demographic details as one user may have multiple mobile device.
- Both brand and model columns have some Chinese words which need to converted to English.

## Data Pre-Processing
Following Data fixes need to be applied :-

- Replace Chinese words present in the fields phone_brand and device_model of phone_brand_device_model dataset with equivalent English words given.

- Replace missing values present in State column of events_data with appropriate value. Use corresponding city value to populate the missing states

- Replace missing values present in longitude and latitude columns of events_data with the help of device_id

- Fix the data issue (inappropriate value) present in longitude and latitude fields of events_data with the help of Plotly library (for TamilNadu and UttarPradesh)

- Update the state from UttarPradesh to Gujarat for city "Kadi"

- Replace the missing values present in device_id column of events_data with the help of corresponding longitude and latitude values

- Analyse Lattitude and Longitude information using Plotly and Folium Packages (Inappropriate Data Fix)

- Convert timestamp column to Datetime

- Filtering the states based upon problem statement

- Merging all Three Datasets for EDA

## Pandas profiling after data pre-processing
Following Data fixes have been applied :-

- Replaced Chinese words present in the fields phone_brand and device_model of phone_brand_device_model dataset with equivalent English words given.

- Replaced missing values present in State column of events_date with appropriate value. Use corresponding city value to populate the missing states.

- Replaced missing values present in longitude and latitude columns of events_data with the help of device_id.

- Found the data issue (inappropriate value) present in longitude and latitude fields of events_data with the help of Plotly and Folium libraries. This co-ordinates issue of only 'TamilNadu','Manipur','Chandigarh','Tripura','UttarPradesh', 'ArunachalPradesh' states have been analyzed and resolved. We have only found outliers for TamilNadu and UttarPradesh. For TamilNadu, the incorrect data (longitude and latitude) present in 9 rows have been repleced with the correct longitudes and latitudes for the same device_id. For UtterPradesh, fix has been explained in below point.

- After analyzing the data of UttarPradesh, we found that one of the cities named 'Kadi' has incorrect state. It is part of Gujarat state but as per data it is UttarPradesh. So we updated the state of 947 rows having city=Kadi from UttarPradesh to Gujarat.

- Replaced the missing values present in device_id column of events_data with the help of corresponding longitude and latitude values.

- Converted timestamp column to Datetime.

- Filtered the states based upon problem statement i.e. only keep these states for our further EDA ('TamilNadu','Manipur','Chandigarh','Tripura','UttarPradesh','ArunachalPradesh'.

- Merged all Three Datasets into a single DataFrame for EDA

**Observations**

**Dataset Statistics**
Number of variables : 12
Number of observations : 533515
Missing cells : 0
Missing cells (%) : 0.0%
Duplicate rows : 0

**Variable types**
Categorical : 6
Numerical : 5
Date : 1

**Observations**
- event_id is unique which means no duplicate events.
- device_id has only 9538 distinct values. It has no missing values.
- timestamp is of Datetime type
- longitude and latitude have no missing values
- Both longitude and latitude have 9481 and 9476 distinct values respectively.
- Data has 201 different cities out of which maximum users are found in Chennai (almost 69%)
- State has 6 distinct values.
- Maximum users for this Telecom are from TamilNadu followed by UttarPradesh. Both these states contribute more than 99% of the users
- Phone_Brand contains 81 different brand information out of which Xiaomi is preferred by most users followed by Samsung, Huawei, vivo and then Meizu
- More than 80% market in these states are taken by top 5 Phone brands
- More Male mobile users compared to Female (68% Male users and 32% Female users)
- Age column is right-skewed with minimum value of 6 and maximum value of 89
Usage of the network is increasing with increase in age till 35years. After this age the usage is gradually reducing.
- Maximum users for this network are Male which are aged between 32 to 38. Female are not preferring this network much.

## Exploratory Data Analysis on Processed Data
- Maximum number of users are from TamilNadu(60.49%) followed by UttarPradesh(38.11%). Both these states accounts for 98.6% of the total users in these 6 states
- Remaining 4 states (Tripura, Chandigarh, ArunachalPradesh and Manipur) have only 133 users
- There are no duplicate values, we have 2499 Rows and 14 Columns
- Xiaomi(26.69%), Samsung(23.67%) and Huawei(16.67%) are the top 3 most used phone brands. These 3 companies comprise more than 67% of market share in these 6 states.
- Insaid Telecom has more Males users(62.65%) compared to Female users(37.35%) in these 6 states.
- People in the range of 20 yrs to 26 yrs prefer this network the most compared to other age group.
- Most of the users(more than 80%) are of the age 20yrs to 40yrs.
- Calcutta has the maximum number of users followed by Delhi, Bangalore, Mumbai, Chennai
- This network has users mainly in the top 10 metropolitean cities as shown in the graph
- Xiaomi is the most preferred mobile brand followed by Samsung for each age segments except 41+. Samsung has a slight more preference over Xiaomi for Age segment 41+
- TN and UP have the maximum number of users and it seems these people prefer Xiaomi phone followed by Samsung phones
- Both Male and Female prefer Xiaomi phones followed by Samsung. Huawei comes in 3rd number

## Conclusion
- Maximum number of users are from TamilNadu(60.49%) followed by UttarPradesh(38.11%). Both these states accounts for 98.6% of the total users in these 6 states. This could be because they are the larger states and have more towers and better network coverage.Remaining 4 states (Tripura, Chandigarh, ArunachalPradesh and Manipur) have only 133 users

- Xiaomi, Samsung, Huawei, Vivo, OPPO, Meizu, Coolpad, HTC, Lenovo, LeEco are the 10 most used phone brands

- Xiaomi(26.69%), Samsung(23.67%) and Huawei(16.67%) are the top 3 most used phone brands. These 3 companies comprise more than 67% of market share in these 6 states.Xiaomi and Samsung are market leaders when it comes to mobile as they launch models regularly as per market and customer demands. Also they launch more budget friendly models with more service centers.

- Insaid Telecom has more Males users(62.65%) compared to Female users(37.35%)

- People in the range of 20 yrs to 26 yrs prefer this network the most compared to other age group.Most of the users(more than 80%) are of the age 20yrs to 40yrs.Young working class are the main users.

- Calcutta has the maximum number of users followed by Delhi, Bangalore, Mumbai, Chennai.This network has users mainly in the top 10 metropolitean cities showed in the graph

- Xiaomi is the most preferred mobile brand closely followed by Samsung for each age segments except 40+. Samsung has a slight more preference over Xiaomi for Age segment 40+.

- TamilNadu and UttarPradesh have the maximum number of users and it seems these people prefer Xiaomi phone followed by Samsung phones

- Both Male and Female prefer Xiaomi phones followed by Samsung. Huawei comes in 3rd number.

- In TamilNadu, Users of almost all age groups except 33yrs-40yrs and 40+ prefer Xiaomi phone brand closely followed by Samsung. However, users in group 33-40 and 40+ prefer Samsung.

- In UttarPradesh, Users of almost all age groups except 40+ prefer Xiaomi phone brand followed by Samsung. However, users in group 40+ prefer Samsung.

- More Male users compared to Female users in these 6 states

- All age segments have majority Male users. More male users compared to female for all age segments if we compare statewise (except Tripura)

- If we see the distribution of genders across top 10 phone brand, you would notice majority users for each brands are Male

- For all 6 states, majority of users belong to the age segment between 20yrs to 40yrs.

- Across top 10 phone brands, maximum users belong to the age group of 20yrs to 26yrs. Majority users come between 20yrs and 40yrs.

- Call Volume is Highest at night after 6:00 PM till 1:00 AM

- Call Volume is the lowest in the early hours between 2:00 AM and 7:00 AM after which it again starts rising

## Actionable Insights
- Scope to increase user base particularly in Manipur, Chandigarh, Tripura & Arunachal Pradesh through better network coverage and attractive plans particularly aimed at the age segment 20-40 years

- Xiaomi & Samsung being the most popular brands, we could tie up with them to provide InsaidTelecom connection along with the handset at an attractive price

- Special offers/ plans for females, launched on specials occasions like Women's Day, Mother's day etc could help increase Female user base

- Introduce special plans/offers for usage between 2:00 AM and 7:00 AM to incentivize usage at this time and avoid network congestion during peak hours.

- There is scope of improve Business in UttarPradesh and TamilNadu as well. These are densely populated area so we need to improve our customer base by providing more offers.

**Please find the notebook [[here](https://github.com/kasturi-sahu/Capstone_Project_Insaid_Telecom/blob/main/Insaid_CDF_Capstone_project_1_Grp_1003_Final.ipynb "here")]**




