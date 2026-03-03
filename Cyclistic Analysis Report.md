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
       <img width="182" height="104" alt="image" src="https://github.com/user-attachments/assets/b03e90a7-e93e-4e02-ab34-fc97eee26ae8" />
   </p>
   
   Since the resulting count is equal to the number of rows, therefore, there are no duplicate values for ride_id.
  
3.  Checking for ambiguity in ride_id
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


Upon checking the added ride_length column, there are ambiguous values such as negative values and values exceed 24 hours. 

## Data Cleaning
1. Removed entries with null values
2. Removed ride_length with less than 1 minute and more than 24 hours.
3. Added ride_lenth, day_of_week, and month columns
4. A total of 3,748,445 rows was removed and 7,217,952 rows remained
   
<p align="center">
<img width="1989" height="399" alt="image" src="https://github.com/user-attachments/assets/4426634e-95e0-49ac-b763-b1a0946c2d2e" />
</p>

## Analysis

- Ride count by number of members and casual riders
<p align="center">
  <img width="647" height="509" alt="image" src="https://github.com/user-attachments/assets/ce59a4d7-db40-411f-9031-5624dd338825" />
</p>

  From this data, the majority of riders using Cyclistic are those with memberships as it comprises 63.94% of the users.

- Ride count by month, day of week and hours
<p align="center">
  <img width="647" height="509" alt="image" src="https://github.com/user-attachments/assets/8b03dfb9-eefd-4203-93f6-08c490ff6af9" />
</p>









   












