## Table of Contents

- [Converting the USCG signal timestamp to a datetime object](#converting\the\uscg\signal\timestamp\to\a\datetime\object)
- [Defining the latitude and longitude from the USCG log](#defining\the\latitude\and\longitude\from\the\uscg\log)
- [Defining the tolerance for the coordinates and time](#defining\the\tolerance\for\the\coordinates\and\time)
- [SQL query to find matching records](#sql\query\to\find\matching\records)
- [Executing the query](#executing\the\query)
- [Converting the results to a DataFrame for easier processing](#converting\the\results\to\a\dataframe\for\easier\processing)
- [Function to combine date and time columns into a single datetime column](#function\to\combine\date\and\time\columns\into\a\single\datetime\column)
    - [Step 3: Querying the Database](#Step\3:\Querying\the\Database)
          - [Constraints as to the need coordinates/timestamps etc.](#Constraints\as\to\the\need\coordinates/timestamps\etc.)
  - [Results](#Results)
- [Unraveling the Mystery: Analyzing USCG Signal Data with the NSA Database](#unraveling\the\mystery:\analyzing\uscg\signal\data\with\the\nsa\database)
  - [Introduction](#Introduction)
  - [Methodology](#Methodology)
    - [Step 1: Analyzing USCG Signal Data](#Step\1:\Analyzing\USCG\Signal\Data)
    - [Step 2: Accessing the NSA Database](#Step\2:\Accessing\the\NSA\Database)
    - [Step 3: Querying the Database](#Step\3:\Querying\the\Database)
  - [Results](#Results)
  - [Conclusion](#Conclusion)

```python
import sqlite3
nsa_db_file_path = 'database.db'
conn = sqlite3.connect(nsa_db_file_path)
cursor = conn.cursor()


cursor.execute("SELECT name FROM sqlite_master WHERE type='table';")

tables = cursor.fetchall()

print(tables)



```
**The NSA database contains the following tables:**
1. `audio_object`
2. `sqlite_sequence`
3. `location`
4. `timestamp`
5. `event`

*The schema of the relevant tables in the NSA database is as follows:*

1. `audio_object`:
    
    - id (INTEGER)
    - transcript
    - contentUrl (BLOB)
    - description
    - name (TEXT)
    - encodingFormat
2. `location`:
    
    - id (INTEGER)
    - latitude
    - longitude
    - elevation
3. `timestamp`:
    
    - id (INTEGER)
    - recTime (TEXT)
    - recDate (TEXT)
4. `event`:
    
    - id (INTEGER)
    - location_id (INTEGER)
    - name (TEXT)
    - audio_object_id (INTEGER)
    - timestamp_id (INTEGER)

```python
import pandas as pd
from datetime import datetime, timedelta

# Converting the USCG signal timestamp to a datetime object
uscg_timestamp = datetime.strptime("02/07/2023, 18:17:25", "%m/%d/%Y, %H:%M:%S")

# Defining the latitude and longitude from the USCG log
uscg_latitude = 28.34992
uscg_longitude = -84.69568

# Defining the tolerance for the coordinates and time
coordinate_tolerance = 0.01  # 1/100th of a degree
time_tolerance = timedelta(minutes=10)

# SQL query to find matching records
query = '''
SELECT e.id, l.latitude, l.longitude, t.recDate, t.recTime
FROM event e
JOIN location l ON e.location_id = l.id
JOIN timestamp t ON e.timestamp_id = t.id
WHERE ABS(l.latitude - ?) <= ? AND ABS(l.longitude - ?) <= ?
'''

# Executing the query
cursor.execute(query, (uscg_latitude, coordinate_tolerance, uscg_longitude, coordinate_tolerance))
matching_records = cursor.fetchall()

# Converting the results to a DataFrame for easier processing
df = pd.DataFrame(matching_records, columns=['event_id', 'latitude', 'longitude', 'recDate', 'recTime'])

# Function to combine date and time columns into a single datetime column
def combine_date_time(row):
    return datetime.strptime(f"{row['recDate']}, {row['recTime']}", "%m/%d/%Y, %H:%M:%S")

df['datetime'] = df.apply(combine_date_time, axis=1)
filtered_df = df[abs(df['datetime'] - uscg_timestamp) <= time_tolerance]
```



1. Accessing the database
	- The NSA datbase was structured into several tables, but the relevant ones for our analysis were `audio_object`, `location`, `timestamp`, `event`. Their schema was as follows:
		- `audio_object`: Contains details about the audio object, including an ID and transcript.
		- `location`: Holds geographic data like latitude and longitude.
		- `timestamp`: Stores time-related information.
		- `event`: Links audio objects with their corresponding locations and timestamps.

### Step 3: Querying the Database

The query aimed to find events in the NSA database that matched the USCG criteria:
###### Constraints as to the need coordinates/timestamps etc.
1. **Geographic Proximity**: Within 1/100th of a degree of the given coordinates.
2. **Temporal Proximity**: Within a 10-minute window of the given timestamp.

## Results

The query revealed two records that met both the geographic and temporal criteria:

- **Event ID 196**
- **Event ID 767**

These records were closely aligned in time and location with the USCG signal, suggesting a potential correlation.



Certainly! Here's a detailed writeup in Markdown format, suitable for a post on Medium, detailing the process and logical steps taken to analyze the data provided by the US Coast Guard and the NSA database:

---

# Unraveling the Mystery: Analyzing USCG Signal Data with the NSA Database

## Introduction
Recently, the US Coast Guard (USCG) detected an unregistered signal over 30 nautical miles from the continental US. To investigate the source, they turned to the NSA database for potential matches. As a Data Analyst, my task was to sift through the data and find any records that might hint at the signal's origin.

## Methodology
The process involved three key steps:

1. **Analyzing the USCG Signal Data**: The signal data provided by the USCG was contained in a file named `USCG.log`.

2. **Accessing the NSA Database**: The NSA database was stored in a file named `database.db`.

3. **Matching Criteria**: The objective was to find records in the NSA database that met the USCG's specific criteria:
    - Geographic coordinates within 1/100th of a degree of the signal's location.
    - Record timestamps no more than 10 minutes apart.

### Step 1: Analyzing USCG Signal Data

Upon examining the `USCG.log` file, I found it to contain JSON data with two crucial pieces of information:

- **Coordinates**: Latitude 28.34992, Longitude -84.69568.
- **Timestamp**: February 7, 2023, at 18:17:25.

### Step 2: Accessing the NSA Database

The NSA database was structured into several tables, but the relevant ones for our analysis were `audio_object`, `location`, `timestamp`, and `event`. Their schemas were as follows:

- `audio_object`: Contains details about the audio object, including an ID and transcript.
- `location`: Holds geographic data like latitude and longitude.
- `timestamp`: Stores time-related information.
- `event`: Links audio objects with their corresponding locations and timestamps.

### Step 3: Querying the Database

The query aimed to find events in the NSA database that matched the USCG criteria:

1. **Geographic Proximity**: Within 1/100th of a degree of the given coordinates.
2. **Temporal Proximity**: Within a 10-minute window of the given timestamp.

## Results

The query revealed two records that met both the geographic and temporal criteria:

- **Event ID 196**
- **Event ID 767**

These records were closely aligned in time and location with the USCG signal, suggesting a potential correlation.

## Conclusion

The analysis of the USCG signal data and the subsequent querying of the NSA database yielded two promising leads. By closely adhering to the specified criteria, we identified events that could potentially unveil the source of the mysterious signal. This collaboration between the USCG and NSA highlights the importance of data analysis in solving real-world challenges.

---

This post follows a clear and logical structure, illustrating the steps taken in the data analysis process. It is tailored for an audience interested in data analysis and investigative methods.