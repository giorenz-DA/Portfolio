# Data Exploration

Combining all of the imported datasets:

  ```SQL
  CREATE TABLE combined_trip AS 	
    SELECT * FROM trip202601	
      UNION ALL 	
    SELECT * FROM trip202512	
      UNION ALL	
    SELECT * FROM trip202511	
      UNION ALL	
    SELECT * FROM trip202510	
      UNION ALL	
    SELECT * FROM trip202509	
      UNION ALL	
    SELECT * FROM trip202508	
      UNION ALL	
    SELECT * FROM trip202507	
      UNION ALL	
    SELECT * FROM trip202506	
      UNION ALL	
    SELECT * FROM trip202505	
      UNION ALL	
    SELECT * FROM trip202504	
      UNION ALL	
    SELECT * FROM trip202503	
      UNION ALL	
    SELECT * FROM trip202502;
```

Checking for null values:

```SQL
SELECT 	
	COUNT (*) FILTER (WHERE ride_id IS NULL) AS ride_id_null,
	COUNT (*) FILTER (WHERE rideable_type IS NULL) AS rideable_type_null, 
	COUNT (*) FILTER (WHERE started_at IS NULL) AS started_at_null,
	COUNT (*) FILTER (WHERE ended_at IS NULL) AS ended_at_null,
	COUNT (*) FILTER (WHERE start_station_name IS NULL) AS start_station_name_null,
	COUNT (*) FILTER (WHERE start_station_id IS NULL) AS start_station_id_null,
	COUNT (*) FILTER (WHERE end_station_name IS NULL) AS end_station_name_null,
	COUNT (*) FILTER (WHERE end_station_id IS NULL) AS end_station_id_null,
	COUNT (*) FILTER (WHERE start_lat IS NULL) AS start_lat_null,
	COUNT (*) FILTER (WHERE start_lng IS NULL) AS start_lng_null,
	COUNT (*) FILTER (WHERE end_lat IS NULL) AS end_lat_null,
	COUNT (*) FILTER (WHERE end_lng IS NULL) AS end_lng_null,
	COUNT (*) FILTER (WHERE member_casual IS NULL) AS member_casual_null
FROM combined_trip;
```
Checking for duplicate values on primary key:

```SQL
SELECT 	
	DISTINCT COUNT (*) ride_id
FROM combined_trip;
```

Checking for ambiguity of primary key:

```SQL
SELECT 	
	LENGTH (ride_id) AS len_ride_id,	
	COUNT (ride_id)
FROM combined_trip
GROUP BY len_ride_id;
```

 

# Data Cleaning

```SQL
DROP TABLE IF EXISTS combined_trip_cleaned;

CREATE TABLE combined_trip_cleaned AS (	
	SELECT		
		ride_id, rideable_type, started_at, ended_at,start_station_name,
		end_station_name, start_lat, start_lng, end_lat, end_lng, member_casual,
		ended_at - started_at AS ride_length,
		TRIM (TO_CHAR (started_at, 'Day')) AS day_of_week,
		TRIM (TO_CHAR (started_at, 'Month')) AS month	
	FROM combined_trip	
	WHERE 		
		start_station_name IS NOT NULL AND
		end_station_name IS NOT NULL AND
		end_lat IS NOT NULL AND
		end_lng IS NOT NULL AND
		ended_at - started_at BETWEEN INTERVAL '1 minute' AND INTERVAL '24 hour’);
```



# Analysis

Ride count by month:

```SQL
  SELECT 		
		month,	
		COUNT (member_casual) FILTER (WHERE member_casual = 'member') AS member_count,		
		COUNT (member_casual) FILTER (WHERE member_casual = 'casual') AS casual_count	
	FROM combined_trip_cleaned
	GROUP BY month
	ORDER BY 	
		CASE month	
		WHEN 'January' THEN 1        
		WHEN 'February' THEN 2        
		WHEN 'March' THEN 3        
		WHEN 'April' THEN 4        
		WHEN 'May' THEN 5        
		WHEN 'June' THEN 6        
		WHEN 'July' THEN 7		
		WHEN 'August' THEN 8		
		WHEN 'September' THEN 9		
		WHEN 'October' THEN 10		
		WHEN 'Nobember' THEN 11		
		WHEN 'December' THEN 12
	END ASC;
```

Ride count by day:

```SQL
SELECT 		
		day_of_week,
		COUNT (member_casual) FILTER (WHERE member_casual = 'member’),
		COUNT (member_casual) FILTER (WHERE member_casual = 'casual’)
	FROM combined_trip_cleaned
	GROUP BY day_of_week
	ORDER BY    
		CASE day_of_week        
			WHEN 'Monday' THEN 1        
			WHEN 'Tuesday' THEN 2        
			WHEN 'Wednesday' THEN 3        
			WHEN 'Thursday' THEN 4        
			WHEN 'Friday' THEN 5        
			WHEN 'Saturday' THEN 6        
			WHEN 'Sunday' THEN 7    
	END ASC;
```

Ride count by hour:

```SQL
SELECT 		
		EXTRACT (HOUR FROM started_at) AS hour_of_day,	
		COUNT (member_casual) FILTER (WHERE member_casual = 'member') AS member_count,
    COUNT (member_casual) FILTER (WHERE member_casual = 'casual') AS casual_count	FROM combined_trip_cleaned
	GROUP BY hour_of_day
	ORDER BY hour_of_day ASC
```

Average ride duration by month:

```SQL
  SELECT 		
		month,	
		AVG (ride_length) FILTER (WHERE member_casual = 'member') AS member_count,		
		AVG (ride_length) FILTER (WHERE member_casual = 'casual') AS casual_count	
	FROM combined_trip_cleaned
	GROUP BY month
	ORDER BY 	
		CASE month	
		WHEN 'January' THEN 1        
		WHEN 'February' THEN 2        
		WHEN 'March' THEN 3        
		WHEN 'April' THEN 4        
		WHEN 'May' THEN 5        
		WHEN 'June' THEN 6        
		WHEN 'July' THEN 7		
		WHEN 'August' THEN 8		
		WHEN 'September' THEN 9		
		WHEN 'October' THEN 10		
		WHEN 'Nobember' THEN 11		
		WHEN 'December' THEN 12
	END ASC;
```

Average ride duration by day:

```SQL
SELECT 
	day_of_week,
	AVG (ride_length) FILTER (WHERE member_casual = 'member') AS member_count,		
  AVG (ride_length) FILTER (WHERE member_casual = 'casual') AS casual_count	
FROM combined_trip_cleaned
GROUP BY day_of_week
ORDER BY    
	CASE day_of_week        
		WHEN 'Monday' THEN 1        
		WHEN 'Tuesday' THEN 2        
		WHEN 'Wednesday' THEN 3        
		WHEN 'Thursday' THEN 4        
		WHEN 'Friday' THEN 5        
		WHEN 'Saturday' THEN 6        
		WHEN 'Sunday' THEN 7    
END ASC;
```

Average ride duration by hour:

```SQL
SELECT 
	EXTRACT (HOUR FROM started_at) AS hour_of_day,
	AVG (ride_length) FILTER (WHERE member_casual = 'member') AS member_count,		
  AVG (ride_length) FILTER (WHERE member_casual = 'casual') AS casual_count	
FROM combined_trip_cleaned
GROUP BY hour_of_day
ORDER BY hour_of_day
```

Most common starting station for members:

```SQL
SELECT 		
	start_station_name,	
	COUNT (ride_id) FILTER (WHERE member_casual = 'member') AS member_count		
FROM combined_trip_cleaned
GROUP BY start_station_name
ORDER BY member_count DESC

LIMIT 20;
```

Most common starting station for casual:

```SQL
SELECT 		
	start_station_name,			
	COUNT (ride_id) FILTER (WHERE member_casual = 'casual') AS casual_count	
FROM combined_trip_cleaned
GROUP BY start_station_name
ORDER BY casual_count DESC

LIMIT 20;
```

Most common ending station for members:

```SQL
SELECT 		
	end_station_name,	
	COUNT (ride_id) FILTER (WHERE member_casual = 'member') AS member_count		
FROM combined_trip_cleaned
GROUP BY end_station_name
ORDER BY member_count DESC

LIMIT 20;
```

Most common ending station for casual:

```SQL
SELECT 		
	end_station_name,			
	COUNT (ride_id) FILTER (WHERE member_casual = 'casual') AS casual_count	
FROM combined_trip_cleaned
GROUP BY end_station_name
ORDER BY casual_count DESC

LIMIT 20;
```















