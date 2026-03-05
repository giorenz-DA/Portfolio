# Project 1: Cyclistic

## Introduction

In this case study, I act as a junior data analyst working on the marketing analyst team for a fictional bike-share service company Cyclistic based in Chicago. The director of marketing believes the company’s future success depends on maximizing the number of annual memberships. Therefore, the team wants to understand how casual riders and annual members use Cyclistic bikes differently. From these insights, the team will design a new marketing strategy to convert casual riders into annual members. But first, Cyclistic executives must approve the recommendations, so they must be backed up with compelling data insights and professional data visualizations. 



## Background

In 2016, Cyclistic launched a successful bike-share offering. Since then, the program has grown to a fleet of 5,824 bicycles that are geotracked and locked into a network of 692 stations across Chicago. The bikes can be unlocked from one station and returned to any other station in the system anytime. 

Until now, Cyclistic’s marketing strategy relied on building general awareness and appealing to broad consumer segments. One approach that helped make these things possible was the flexibility of its pricing plans: single-ride passes, full-day passes, and annual memberships. Customers who purchase single-ride or full-day passes are referred to as casual riders. Customers who purchase annual memberships are Cyclistic members. 

Cyclistic’s finance analysts have concluded that annual members are much more profitable than casual riders. Although the pricing flexibility helps Cyclistic attract more customers, Moreno believes that maximizing the number of annual members will be key to future growth. Rather than creating a marketing campaign that targets all-new customers, Moreno believes there is a solid opportunity to convert casual riders into members. She notes that casual riders are already aware of the Cyclistic program and have chosen Cyclistic for their mobility needs.

## Stakeholders

- Lily Moreno: The director of marketing and your manager. Moreno is responsible for the development of campaigns and initiatives to promote the bike-share program. These may include email, social media, and other channels.
- Cyclistic marketing analytics team: A team of data analysts who are responsible for collecting, analyzing, and reporting data that helps guide Cyclistic marketing strategy. You joined this team six months ago and have been busy learning about Cyclistic’s mission and business goals—as well as how you, as a junior data analyst, can help Cyclistic achieve them.
- Cyclistic executive team: The notoriously detail-oriented executive team will decide whether to approve the recommended marketing program.

## Busineess Task

Three questions will guide the future marketing program: 
1. How do annual members and casual riders use Cyclistic bikes differently?
2. Why would casual riders buy Cyclistic annual memberships?
3. How can Cyclistic use digital media to influence casual riders to become members?

Moreno has assigned you the first question to answer: __How do annual members and casual riders use Cyclistic bikes differently?__

## About the Data 

The data used for this analysis is downloaded from this [__link.__](https://divvy-tripdata.s3.amazonaws.com/index.html) The data used are from the past 12 months prior to the analysis (February 20205 - January 2026)
This is public data that we can use to explore how different customer types are using Cyclistic bikes. But note that data-privacy issues prohibit us from using riders’ personally identifiable information. This means that we won’t be able to connect pass purchases to credit card numbers to determine if casual riders live in the Cyclistic service area or if they have purchased multiple single passes.

(Note: The datasets have a different name because Cyclistic is a fictional company. For the purposes of this case study, the datasets are appropriate and will enable us to answer the business questions. The data has been made available by Motivate International Inc. under this [__license__](https://divvybikes.com/data-license-agreement).)

The data is consist of 12 tables with the same columns which are:

ride_id - 16 character id that identifies every booking of the bike-share service.
rideable_type - different types of bikes that are available.
started_at - timestamp of when a bike was used.
ended_at - timestamp of when a bike was done being used.
start_station_name - name of the station where the bike started.
start_station_id - id of the station where the bike started.
end_station_name - name of the station where the bike ended.
end_station_id - id of the station where the bike ended.
start_lat - latitude of the location of the starting station.
start_lng - longitude of the location of the starting station.
end_lat - latitude of the location of the ending station.
end_lng - longitude of the location of the ending station.
member_casual - membership type of the rider. 

## Tools used for Analysis

1. PostgreSQL - for data cleaning and aggregation
2. Tableau - for visualization


## Data Preparation

1. PgAdmin interface was used to import and create tables in PostgreSQL
 <p align="center">
   <img width="496" height="307" alt="image" src="https://github.com/user-attachments/assets/0ba62dc0-fe99-406f-873e-3b393c14b429" />
   <img width="499" height="307" alt="image" src="https://github.com/user-attachments/assets/9cd096f7-60c4-4c28-b8eb-4c4e70efa254" />
   <img width="526" height="615" alt="image" src="https://github.com/user-attachments/assets/0404b914-7893-458b-a355-b91b20c1a3c5" />
  </p>

2. After importing the csv files, all tables were combined to 1 table named combined_trip containing 10,966,397 rows
<p align="center">
  <img width="1997" height="397" alt="image" src="https://github.com/user-attachments/assets/e39d758a-088f-418d-96d7-82fe8c4675d6" />
</p>

## Data Exploration
1. Checking for null values on every column:
<p align="center">
    <img width="1664" height="71" alt="image" src="https://github.com/user-attachments/assets/3e4bad3d-a50d-43cb-a158-de420d24d0f2" />
</p>
   
2. Checking for duplicate values. Checking for duplicates is performed on ride_id column as it is the primary key.
   <p align="center">
       <img width="259" height="75" alt="image" src="https://github.com/user-attachments/assets/81434789-220f-4155-8dd2-6e8615d8140a" />
   </p>

   Upon checking, a total of 5,414,305 (49.37%) rows are found to have duplicate values for ride_id. This is too many data for us to just omit.

   For further investigation, we have to check if the duplicated values of ride_id have different values on other columns or just the ride_id
   <p align="center">
     <img width="1485" height="297" alt="image" src="https://github.com/user-attachments/assets/496e9999-b3a5-4b51-ab95-fd1b31f19d40" />
   </p>
   <p align="center">
     <img width="1486" height="308" alt="image" src="https://github.com/user-attachments/assets/490921da-b995-4cd0-b8da-0c116675fa25" />
   </p>
   
   These show that the entries with duplicate values for ride_id is actually a duplicate data and needs to be removed for proper analysis. 
    
4.  Checking for ambiguity in ride_id
   <p align="center">
     <img width="332" height="129" alt="image" src="https://github.com/user-attachments/assets/2477e1be-c2d5-4523-8cfa-5a9df3c9a115" />
   </p>
     All of the rows shows a 16 character on ride_id. This means that there are no ambiguity.

4. Checking for member_casual values
  <p align="center">
   <img width="294" height="118" alt="image" src="https://github.com/user-attachments/assets/2fd14ffa-480e-489f-9fe8-1c641188b82c" />
  </p>

  There are no ambiguity in the member_casual column and all entries are accounted for, totaling to 10,966,397 

5. Adding ride_length column to show duration of every ride by subtracting ended_at by started_at:
<p align="center">
   <img width="1825" height="172" alt="image" src="https://github.com/user-attachments/assets/9e9188a6-0229-4dec-913c-d6f2f716bd90" />
</p>

Upon checking the added ride_length column, there are ambiguous values such as negative values and values exceed 24 hours. 

## Data Cleaning
1. Removed entries with null values
2. Removed duplicate rows
3. Removed ride_length with less than 1 minute and more than 24 hours.
4. Added ride_lenth, day_of_week, and month columns
5. A total of 7,309,353 rows was removed.
   
<p align="center">
<img width="2011" height="397" alt="image" src="https://github.com/user-attachments/assets/659f9255-d28d-4c65-80da-4776c287de08" />
</p>

## Analysis

Going back to the question that is assigned to us by the Director of Marketing Department. _How do annual members and casual riders use Cyclistic bikes differently?_


To give us insights how casual riders use Cyclistic differently with members, we need to see the number of riders with membership and the casual riders. We will also check their respective usage count and average duration of usage per month, day of week and by hour of the day.
Finalyy, we need to check which start and end stations are mostly used by members and casual riders to see which places they frequently go in. 

- Ride count by number of members and casual riders
<p align="center">
  <img width="819" height="658" alt="image" src="https://github.com/user-attachments/assets/0de06a05-631a-40c1-8ee2-31f58ff4f6ad" />
</p>

  From this data, the majority of riders using Cyclistic are those with memberships as it comprises 63.94% of the users and almost twice as many as casual riders. 

- Ride count by month, day of week and hours
<p align="center">
  <img width="820" height="658" alt="image" src="https://github.com/user-attachments/assets/52643791-1cc6-4b6f-ad12-7271fdfdff86" />
</p>

  __Month:__ For riders with membership, a steady increase can be seen from January to June and peaking at August. Although casual riders are smaller in count compared to members, the same can be observed in the ride count through the months and peaking also at the month of August. One factor that could affect this is the season and weather during these months. Summer and Fall are in the months of June to November that is why this is when the ride count peaked, when the weather is just right for riding a bike. While the months of January and December are with the lowest ride count as this is winter season in Chicago. Aside from the cold, the streets are slippery and may pose a threat to safety if bicycles are still used for travel.  
  
  __Day of Week:__ A steady ride count during the weekdays and a decline during the weekends especially on Sundays. This could mean that members use Cyclistic for their daily commute to their work, school and other daily activities on weekdays. The casual riders also have a steady ride count during the weekdays until Wednesdays. There is a gradual increase increase on Thursdays to Saturdays peaking on Saturdays and then decreasing on Sundays. This could mean that casual riders use Cyclistic mainly for leisure purposes since the highest ride count is during Saturdays.  
  
  __Hour of Day:__ The highest ride count for members are during 8:00 AM and 5:00 PM. This supports our hypothesis that members use Cyclistic for daily commute as these hours are the most common times for clocking in and clocking out of work for employees and also when classes starts amd emds for students. For casual riders, a gradual increase from 7:00 AM to 5:00 PM, peaking at 5:00 PM, can be observed from the graph. They prefer using Cyclistic during the afternoon as this could be the best time to ride around tourist spots.  
  

- Average ride duration by month, day of week and hours
  <p align="center">
    <img width="994" height="795" alt="image" src="https://github.com/user-attachments/assets/cb7e7e9e-e413-47da-bf35-d58970081a18" />
  </p>

  __Month:__  Although ride count for members are higher than casual riders, casual riders tend to ride longer. For the Winter Months, the average ride duration are pretty much the same for members while for casual riders these are the months they tend to ride the shortest. The climate and temperature could be a factor why casual riders do not want to ride longer. From June to August are the months when casual riders spend longer riding bikes as these months are Summer season in Chicago.
  
  __Day of Week:__ Average ride duration for members during the week is nearly the same with very little movement. But for casual riders, aside from their count being the most in weekends, they also tend to ride the longest during these days. This supports our hypothesis from the ride count graph where casual riders mainly use Cyclistic for leisure purposes as riders that are sight seeing ride longer.
  
  __Hour of Day:__ The average ride duration for members are mostly the same at approximately 12 minutes. For casual riders, they ride the longest during 9:00 AM to 4:00 PM. These times are also the times when tourist spots mostly open so this also supports are hypothesis from before that casual riders use Cyclistic for leisure.  
  

- Mostly used Starting Station
  
  <p align="center">
    <img width="822" height="658" alt="image" src="https://github.com/user-attachments/assets/4e9aa085-dae4-42bc-ae9d-c3c8b65a265c" />
  </p> 

  __Members:__ Members mostly start at streets where Business Districts are located. 

Kingsbury St & Kinzie St:

<p align="center">
<img width="1173" height="659" alt="image" src="https://github.com/user-attachments/assets/aa49a385-1d29-49cb-a1c2-1fa89018dd4c" />
</p> 

Clinton St & Washington Blvd:

<p align="center">
<img width="1208" height="659" alt="image" src="https://github.com/user-attachments/assets/64b4f829-6a69-4a52-95c7-fc8d3d2c7dd1" />
</p> 

Wells St & Huron St:

<p align="center">
<img width="1183" height="659" alt="image" src="https://github.com/user-attachments/assets/92587b10-26f4-45f8-b46d-69ccab864e22" />
</p>


  __Casual Riders:__ Casual riders mostly start on stations with tourist spots in the vicinity like parks, piers, aquarium, etc. 


DuSable Lake Shore Dr & Monroe St

<p align="center">
<img width="1173" height="660" alt="image" src="https://github.com/user-attachments/assets/6bb8c9c3-77d3-4c0e-9962-96f907eaae3c" />
</p> 

Michigan Ave & Oak St

<p align="center">
<img width="1172" height="659" alt="image" src="https://github.com/user-attachments/assets/15413942-7347-4310-9c34-a81bfd0b421c" />
</p> 

DuSable Lake Shore Dr & North Blvd

<p align="center">
<img width="1164" height="658" alt="image" src="https://github.com/user-attachments/assets/0ae78be5-6bf7-4abe-9727-c8bbf730864c" />
</p> 

- Mostly used End Stations
  
   <p align="center">
    <img width="817" height="658" alt="image" src="https://github.com/user-attachments/assets/8ed76cf1-abe9-432e-8c9e-804199e910a8" />
   </p> 

   __Members:__ The end stations mostly used by members are also the same as the starting stations which are in the vicinity of business establihments. This means that members mostly use Cyclistic for their commute going to and from their workplace.  
  __Casual Riders:__ For casual riders, the end stations mostly used is also the same as the mostly used starting stations which are in the vicinity of tourist spots and leisure activities like parks, piers and aquariums. This concludes that casual riders mainly use Cyclistic for leisure purposes and sight-seeing.  


Note: All of the visualizations used in this report can be accessed in Tableau Public titled [Google Analytics Capstone Project: Cyclistic](https://public.tableau.com/views/GoogleAnalyticsCapstoneProjectCyclistic/EndStation?:language=en-GB&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link)

## Key Insights

Going back to the question assigned to us, _How do annual members and casual riders use Cyclistic bikes differently?_  
To answer this, we performed analysis on how many annual members and casual riders aree there in the span of the last 12 months. We also analyzed their average ride duration and the mostly used starting and ending stations.

1. Casual riders use Cyclistic bikes mainly for visiting tourist attractions and leisure activities while members use Cyclistic bikes mainly for commute.
2. Casual riders prefer to use Cyclistic and ride longer during Fridays and weekends.
3. Summer to Fall are the seasons where ride count increases for both member and casual riders.
4. Casual riders prefer using Cyclistic when tourist or leisure spot opens.


## Recomendations
1. Implementing time-wise memberships such us Seasonal and Weekend Membership.
2. A referral rewards system where if a casual rider was referred by a member, both will get rewards or discounts.
3. A collaboration with local tourist spots where riders with membership can get rewards or discounts.












   












