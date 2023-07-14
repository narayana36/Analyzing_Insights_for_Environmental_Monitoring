# Analyzing Insights for Environmental Monitoring

## Project Description
This project will train you how to use SQL to analyze a real-world database, how to extract the most useful information from the dataset, how to pre-process the data using Python for improved performance, and how to use a structured query language to retrieve useful information from the database.

## Module 1 Data Preprocessing  using Python: 
In this module, you will query the dataset using structured query language to gain insights from the database. The problem statements to be solved will be provided to you, and you will need to provide the solution for the same using your logic. Different concepts of SQL will be used in this process, such as aggregating the data, grouping the data, ordering the data, etc

Preprocessing The Data:
Renaming The columns: 
Checking For Null values:
Removing Duplicates:
Exporting The Cleaned Dataset
Generate Tables The Cleaned Dataset:
File: Analyzing insights for environmental monitoring and analysis.ipynb
Uncleaned Dataset: iot_telemetry_data.csv
Cleaned dataset: cleaned environment.csv

## Module 2: Data Analysis using SQL:
   In this module, you will be working on performing data analysis on the pre-processed data from the previous module and conducting Data Analysis using SQL. You will generate queries for given problem statements. 

### Task 1: 
Find the average temperature recorded for each device. The task is to calculate the average temperature recorded for each device in the dataset.
ANS: SELECT device_id, AVG(temperature) FROM cleaned_environment GROUP BY device_id;
### Task 2:
Retrieve the top 5 devices with the highest average carbon monoxide levels. This task involves identifying the devices with the highest average carbon monoxide levels and retrieving the top 5 devices based on this metric.
Ans: SELECT device_id, AVG(carbon_monoxide) as average_carbon_monoxide FROM cleaned_environment group by device_id
order by average_carbon_monoxide limit 5;
### Task 3: 
Calculate the average temperature recorded in the cleaned_environment table. The objective is to Determine the average temperature recorded in the cleaned_environment dataset.
Ans:SELECT AVG(temperature) as average_temperature FROM cleaned_environment
### Task 4: 
Find the timestamp and temperature of the highest recorded temperature for each device. This task requires identifying the highest recorded temperature for each device and retrieving the corresponding timestamp and temperature values.
Ans: SELECT device_id, timestamp, MAX(temperature) FROM cleaned_environment group by device_id;
### Task 5:
   Identify devices where the temperature has increased from the minimum recorded temperature to the maximum recorded temperature
The goal is to Identify devices where the temperature has increased from the minimum recorded temperature to the maximum recorded temperature
Ans: select device_id from cleaned_environment group by device_id having min(temperature) 
### Task 6:
Calculate the exponential moving average of temperature for each device limit to 10 devices. Calculate the exponential moving average (EMA) of the temperature for each device. Retrieve the device ID, timestamp, temperature, and the EMA temperature for the first 10 devices from the 'cleaned_environment' table. The EMA temperature is calculated by partitioning the data based on the device ID, ordering it by the timestamp, and considering all preceding rows up to the current row.
Ans: WITH ema_calculation AS ( SELECT device_id, timestamp, temperature, AVG(temperature) OVER 
(PARTITION BY device_id ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS ema_temperature,
ROW_NUMBER() OVER (PARTITION BY device_id ORDER BY timestamp) AS row_num FROM cleaned_environment )
SELECT device_id, timestamp, temperature, ema_temperature FROM ema_calculation limit 10;
### Task 7: 
 Find the timestamps and devices where carbon monoxide level exceeds the average carbon monoxide level of all devices. The objective is to identify the timestamps and devices where the carbon monoxide level exceeds the average carbon monoxide level across all devices.
Ans:SELECT device_id, timestamp FROM cleaned_environment where carbon_monoxide > (SELECT avg(carbon_monoxide) from cleaned_environment)
### Task 8:
 Retrieve the devices with the highest average temperature recorded. The objective is to identify the devices that have recorded the highest average temperature among all the devices in the dataset.
Ans: SELECT device_id, avg(temperature) FROM cleaned_environment group by device_id
### Task 9:
Calculate the average temperature for each hour of the day across all devices. The goal is to calculate the average temperature for each hour of the day, considering data from all devices.
Ans: SELECT EXTRACT(HOUR FROM timestamp) AS hour_of_day, AVG(temperature) AS average_temperature 
FROM cleaned_environment GROUP BY EXTRACT(HOUR FROM timestamp) ORDER BY hour_of_day
### Task 10:
 Which device(s) in the cleaned environment dataset have recorded only a single distinct temperature value?
Ans: SELECT device_id FROM cleaned_environment GROUP BY device_id HAVING COUNT(DISTINCT temperature) = 1
### Task 11:
ind the devices with the highest humidity levels.
The objective is to identify the devices that have recorded the highest humidity levels.
 Ans: select device_id, max(humidity) as max_humidity from cleaned_environment group by device_id
### Task 12:
 Calculate the average temperature for each device, excluding outliers (temperatures beyond 3 standard deviations). This task requires calculating the average temperature for each device while excluding outliers, which are temperatures beyond 3 standard deviations from the mean.
Ans: SELECT device_id, AVG(temperature) AS avg_temperature FROM cleaned_environment
WHERE temperature > ( SELECT AVG(temperature) - (3 * STDDEV(temperature)) FROM cleaned_environment ) GROUP BY device_id;
### Task 13:
Retrieve the devices that have experienced a sudden change in humidity (greater than 50% difference) within a 30-minute window.
The goal is to identify devices that have undergone a sudden change in humidity, where the difference is greater than 50%, within a 30-minute time window.
Ans:SELECT device_id,timestamp, humidity FROM ( SELECT device_id, humidity, timestamp, LAG(humidity)
OVER (PARTITION BY device_id ORDER BY timestamp) AS prev_humidity, LAG(timestamp) OVER (PARTITION BY device_id ORDER BY timestamp)
AS prev_timestamp FROM cleaned_environment ) AS subquery WHERE ABS(humidity - prev_humidity) > 0.5 AND
TIMESTAMPDIFF(SECOND, prev_timestamp, timestamp) <= 1800
### Task 14:
Find the average temperature for each device during weekdays and weekends separately. This task involves calculating the average temperature for each device separately for weekdays and weekends.
Ans: SELECT device_id, CASE WHEN timestamp = 1 or 7 THEN 'Weekday' ELSE 'Weekend' END AS day_type, AVG(temperature)
AS average_temperature FROM cleaned_environment GROUP BY device_id;
### Task 15:
 Calculate the cumulative sum of temperature for each device, ordered by timestamp limit to 10. The objective is to calculate the cumulative sum of temperature for each device, considering the records ordered by timestamp limit to 10.
Ans: SELECT device_id, timestamp, temperature, SUM(temperature) OVER (PARTITION BY device_id ORDER BY timestamp)
AS cumulative_temperature FROM cleaned_environment LIMIT 10;
