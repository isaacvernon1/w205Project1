# Project 1: Query Project

- In the Query Project, you will get practice with SQL while learning about
  Google Cloud Platform (GCP) and BiqQuery. You'll answer business-driven
  questions using public datasets housed in GCP. To give you experience with
  different ways to use those datasets, you will use the web UI (BiqQuery) and
  the command-line tools, and work with them in Jupyter Notebooks.

#### Problem Statement

- You're a data scientist at Lyft Bay Wheels (https://www.lyft.com/bikes/bay-wheels), formerly known as Ford GoBike, the
  company running Bay Area Bikeshare. You are trying to increase ridership, and
  you want to offer deals through the mobile app to do so. 
  
- What deals do you offer though? Currently, your company has several options which can change over time.  Please visit the website to see the current offers and other marketing information. Frequent offers include: 
  * Single Ride 
  * Monthly Membership
  * Annual Membership
  * Bike Share for All
  * Access Pass
  * Corporate Membership
  * etc.

- Through this project, you will answer these questions: 

  * What are the 5 most popular trips that you would call "commuter trips"? 
  
  * What are your recommendations for offers (justify based on your findings)?

- Please note that there are no exact answers to the above questions, just like in the proverbial real world.  This is not a simple exercise where each question above will have a simple SQL query. It is an exercise in analytics over inexact and dirty data. 

- You won't find a column in a table labeled "commuter trip".  You will find you need to do quite a bit of data exploration using SQL queries to determine your own definition of a communter trip.  In data exploration process, you will find a lot of dirty data, that you will need to either clean or filter out. You will then write SQL queries to find the communter trips.

- Likewise to make your recommendations, you will need to do data exploration, cleaning or filtering dirty data, etc. to come up with the final queries that will give you the supporting data for your recommendations. You can make any recommendations regarding the offers, including, but not limited to: 
  * market offers differently to generate more revenue 
  * remove offers that are not working 
  * modify exising offers to generate more revenue
  * create new offers for hidden business opportunities you have found
  * etc. 

#### All Work MUST be done in the Google Cloud Platform (GCP) / The Majority of Work MUST be done using BigQuery SQL / Usage of Temporary Tables, Views, Pandas, Data Visualizations

A couple of the goals of w205 are for students to learn how to work in a cloud environment (such as GCP) and how to use SQL against a big data data platform (such as Google BigQuery).  In keeping with these goals, please do all of your work in GCP, and the majority of your analytics work using BigQuery SQL queries.

You can make intermediate temporary tables or views in your own dataset in BigQuery as you like.  Actually, this is a great way to work!  These make data exploration much easier.  It's much easier when you have made temporary tables or views with only clean data, filtered rows, filtered columns, new columns, summary data, etc.  If you use intermediate temporary tables or views, you should include the SQL used to create these, along with a brief note mentioning that you used the temporary table or view.

In the final Jupyter Notebook, the results of your BigQuery SQL will be read into Pandas, where you will use the skills you learned in the Python class to print formatted Pandas tables, simple data visualizations using Seaborn / Matplotlib, etc.  You can use Pandas for simple transformations, but please remember the bulk of work should be done using Google BigQuery SQL.

#### GitHub Procedures

In your Python class you used GitHub, with a single repo for all assignments, where you committed without doing a pull request.  In this class, we will try to mimic the real world more closely, so our procedures will be enhanced. 

Each project, including this one, will have it's own repo.

Important:  In w205, please never merge your assignment branch to the master branch. 

Using the git command line: clone down the repo, leave the master branch untouched, create an assignment branch, and move to that branch:
- Open a linux command line to your virtual machine and be sure you are logged in as jupyter.
- Create a ~/w205 directory if it does not already exist `mkdir ~/w205`
- Change directory into the ~/w205 directory `cd ~/w205`
- Clone down your repo `git clone <https url for your repo>`
- Change directory into the repo `cd <repo name>`
- Create an assignment branch `git branch assignment`
- Checkout the assignment branch `git checkout assignment`

The previous steps only need to be done once.  Once you your clone is on the assignment branch it will remain on that branch unless you checkout another branch.

The project workflow follows this pattern, which may be repeated as many times as needed.  In fact it's best to do this frequently as it saves your work into GitHub in case your virtual machine becomes corrupt:
- Make changes to existing files as needed.
- Add new files as needed
- Stage modified files `git add <filename>`
- Commit staged files `git commit -m "<meaningful comment about your changes>"`
- Push the commit on your assignment branch from your clone to GitHub `git push origin assignment`

Once you are done, go to the GitHub web interface and create a pull request comparing the assignment branch to the master branch.  Add your instructor, and only your instructor, as the reviewer.  The date and time stamp of the pull request is considered the submission time for late penalties. 

If you decide to make more changes after you have created a pull request, you can simply close the pull request (without merge!), make more changes, stage, commit, push, and create a final pull request when you are done.  Note that the last data and time stamp of the last pull request will be considered the submission time for late penalties.

---

## Parts 1, 2, 3

We have broken down this project into 3 parts, about 1 week's work each to help you stay on track.

**You will only turn in the project once  at the end of part 3!**

- In Part 1, we will query using the Google BigQuery GUI interface in the cloud.

- In Part 2, we will query using the Linux command line from our virtual machine in the cloud.

- In Part 3, we will query from a Jupyter Notebook in our virtual machine in the cloud, save the results into Pandas, and present a report enhanced by Pandas output tables and simple data visualizations using Seaborn / Matplotlib.

---

## Part 1 - Querying Data with BigQuery

### SQL Tutorial

Please go through this SQL tutorial to help you learn the basics of SQL to help you complete this project.

SQL tutorial: https://www.w3schools.com/sql/default.asp

### Google Cloud Helpful Links

Read: https://cloud.google.com/docs/overview/

BigQuery: https://cloud.google.com/bigquery/

Public Datasets: Bring up your Google BigQuery console, open the menu for the public datasets, and navigate to the the dataset san_francisco.

- The Bay Bike Share has two datasets: a static one and a dynamic one.  The static one covers an historic period of about 3 years.  The dynamic one updates every 10 minutes or so.  THE STATIC ONE IS THE ONE WE WILL USE IN CLASS AND IN THE PROJECT. The reason is that is much easier to learn SQL against a static target instead of a moving target.

- (USE THESE TABLES!) The static tables we will be using in this class are in the dataset **san_francisco** :

  * bikeshare_stations

  * bikeshare_status

  * bikeshare_trips

- The dynamic tables are found in the dataset **san_francisco_bikeshare**

### Some initial queries

Paste your SQL query and answer the question in a sentence.  Be sure you properly format your queries and results using markdown. 

- What's the size of this dataset? (i.e., how many trips)

```sql
SELECT count(*) 
FROM `bigquery-public-data.san_francisco.bikeshare_trips`
```
```
+--------+
|  f0_   |
+--------+
| 983648 |
+--------+
```

After running our query we find that there are 983,648 trips in the dataset.

- What is the earliest start date and time and latest end date and time for a trip?

```sql
SELECT min(start_date), max(end_date)
FROM `bigquery-public-data.san_francisco.bikeshare_trips`
```

```
+---------------------+---------------------+
|         f0_         |         f1_         |
+---------------------+---------------------+
| 2013-08-29 09:08:00 | 2016-08-31 23:48:00 |
+---------------------+---------------------+
```

The earliest trip in the dataset was on August 29th, 2013 at 9:08 am and the latest trip ended at 11:48 pm on August 31, 2016. 

- How many bikes are there?

```sql
SELECT count(distinct bike_number)
FROM `bigquery-public-data.san_francisco.bikeshare_trips`
```

```
+-----+
| f0_ |
+-----+
| 700 |
+-----+
```
There are a total of 700 bikes across all of the stations.

### Questions of your own
- Make up 3 questions and answer them using the Bay Area Bike Share Trips Data.  These questions MUST be different than any of the questions and queries you ran above.

- Question 1: How many trips were between 5 minutes and 1 hour?
  * Answer: We find that there were 776,865 trips that lasted between 5 minutes and 1 hour.
  * SQL query:
```sql
SELECT
  count(*) as Number_of_Trips,
FROM
  `bigquery-public-data.san_francisco.bikeshare_trips`
WHERE duration_sec >= 300 AND duration_sec <= 3600
```
```
+-----------------+
| Number_of_Trips |
+-----------------+
|          776865 |
+-----------------+
```

- Question 2: What station had the greatest number of trips start from there?
  * Answer: When we look at the five stations that had the greatest number of trips started from there, we can find that the San Francisco Caltrain station (Townsend at 4th) had the most, meaning that the San Francisco Caltrain station (Townsend at 4th) saw the most bike trips started of all stations.
  * SQL query:
```sql
SELECT
  count(*) as Number_of_Trips,
  start_station_name, 
FROM
  `bigquery-public-data.san_francisco.bikeshare_trips`
GROUP BY start_station_name
ORDER BY Number_of_Trips DESC
LIMIT 5
```
```
+-----------------+------------------------------------------+
| Number_of_Trips |            start_station_name            |
+-----------------+------------------------------------------+
|           72683 | San Francisco Caltrain (Townsend at 4th) |
|           56100 | San Francisco Caltrain 2 (330 Townsend)  |
|           49062 | Harry Bridges Plaza (Ferry Building)     |
|           41137 | Embarcadero at Sansome                   |
|           39936 | 2nd at Townsend                          |
+-----------------+------------------------------------------+
```

- Question 3: What day of the week had the greatest total number of trips?
  * Answer: When we look at the number of trips for each day, we find that Tuesday had the most occurences at 184,405 trips. In addition, we can see that this value is relatively close to Wednesday, Thursday, and Monday as well, with Friday a little bit behind those. Finally, the least trips by a significant margin are on Saturday and Sunday.
  * SQL query:
```sql
SELECT
    count(*) as Number_of_Trips,
        CASE EXTRACT(DAYOFWEEK FROM start_date)
             WHEN 1 THEN "Sunday"
             WHEN 2 THEN "Monday"
             WHEN 3 THEN "Tuesday"
             WHEN 4 THEN "Wednesday"
             WHEN 5 THEN "Thursday"
             WHEN 6 THEN "Friday"
             WHEN 7 THEN "Saturday"
         END AS Day_of_Week,
FROM
    (SELECT start_date FROM `bigquery-public-data.san_francisco.bikeshare_trips`)
GROUP BY Day_of_Week
ORDER BY Number_of_Trips DESC
```

```
+-----------------+-------------+
| Number_of_Trips | Day_of_Week |
+-----------------+-------------+
|          184405 | Tuesday     |
|          180767 | Wednesday   |
|          176908 | Thursday    |
|          169937 | Monday      |
|          159977 | Friday      |
|           60279 | Saturday    |
|           51375 | Sunday      |
+-----------------+-------------+
```

### Bonus activity queries (optional - not graded - just this section is optional, all other sections are required)

The bike share dynamic dataset offers multiple tables that can be joined to learn more interesting facts about the bike share business across all regions. These advanced queries are designed to challenge you to explore the other tables, using only the available metadata to create views that give you a broader understanding of the overall volumes across the regions(each region has multiple stations)

We can create a temporary table or view against the dynamic dataset to join to our static dataset.

Here is some SQL to pull the region_id and station_id from the dynamic dataset.  You can save the results of this query to a temporary table or view.  You can then join the static tables to this table or view to find the region:
```sql
#standardSQL
select distinct region_id, station_id
from `bigquery-public-data.san_francisco_bikeshare.bikeshare_station_info`
```

- Top 5 popular station pairs in each region

- Top 3 most popular regions(stations belong within 1 region)

- Total trips for each short station name in each region

- What are the top 10 used bikes in each of the top 3 region. these bikes could be in need of more frequent maintenance.

---

## Part 2 - Querying data from the BigQuery CLI 

- Use BQ from the Linux command line:

  * General query structure

    ```
    bq query --use_legacy_sql=false '
        SELECT count(*)
        FROM
           `bigquery-public-data.san_francisco.bikeshare_trips`'
    ```

### Queries

1. Rerun the first 3 queries from Part 1 using bq command line tool (Paste your bq
   queries and results here, using properly formatted markdown):

  * What's the size of this dataset? (i.e., how many trips)
  
```
    bq query --use_legacy_sql=false '
        SELECT count(*)
        FROM
           `bigquery-public-data.san_francisco.bikeshare_trips`'
```

```
+--------+
|  f0_   |
+--------+
| 983648 |
+--------+
```

  * What is the earliest start time and latest end time for a trip?

```
    bq query --use_legacy_sql=false '
        SELECT min(start_date), max(end_date)
        FROM
            `bigquery-public-data.san_francisco.bikeshare_trips`'

```

```
+---------------------+---------------------+
|         f0_         |         f1_         |
+---------------------+---------------------+
| 2013-08-29 09:08:00 | 2016-08-31 23:48:00 |
+---------------------+---------------------+
```

  * How many bikes are there?
  
```
    bq query --use_legacy_sql=false '
        SELECT count(distinct bike_number)
        FROM 
            `bigquery-public-data.san_francisco.bikeshare_trips`'
```

```
+-----+
| f0_ |
+-----+
| 700 |
+-----+
```

2. New Query (Run using bq and paste your SQL query and answer the question in a sentence, using properly formatted markdown):

  * How many trips are in the morning vs in the afternoon?
  
```
    bq query --use_legacy_sql=false '
        SELECT
            count(*) as Number_of_Trips,
            CASE 
               WHEN EXTRACT(HOUR FROM start_date) <= 12 THEN "Morning"
               WHEN EXTRACT(HOUR FROM start_date) > 12 THEN "Afternoon"
            END as Time_of_Day
        FROM
          `bigquery-public-data.san_francisco.bikeshare_trips`
        GROUP BY Time_of_Day'
```

```
+-----------------+-------------+
| Number_of_Trips | Time_of_Day |
+-----------------+-------------+
|          524359 | Afternoon   |
|          459289 | Morning     |
+-----------------+-------------+
```

### Project Questions
Identify the main questions you'll need to answer to make recommendations (list
below, add as many questions as you need).

- Question 1: Are there more trips shorter than an hour or longer than an hour?

- Question 2: Is there an hour in the day that we have low ridership?

- Question 3: Is there a day in the week that we have low ridership?

- Question 4: How many of our trips were what we intially define as commuter trips (Mon - Fri, 7-10am, 4-7pm, Different start and end stations, between 5 minutes and 1 hour)?

- Question 5: How many of the trips in the dataset are from subscribers?

- Question 6: Which stations have the most amount of trips starting from them?

- Question 7: Which stations have the least amount of trips starting from them?

- Question 8: For those stations that had few trips starting from them, what was their total amount of trips?

- Question 9: For these stations with a small number of trips, when were the built/implemented?

- ...

- Question n: What are our definitions for a commuter trip?

### Answers

Answer at least 4 of the questions you identified above You can use either
BigQuery or the bq command line tool.  Paste your questions, queries and
answers below.

- Question 1: Are there more trips shorter than an hour or longer than an hour?
  * Answer: We find that the vast majority of trips that people take are shorter than an hour.
  * SQL query:
```sql
SELECT
  COUNT(*) AS Trips,
  Length_of_Trip
FROM (
  SELECT
    CASE
      WHEN duration_sec <= 3600 THEN "Less_Than_Hour"
    ELSE
    "More_Than_Hour"
  END
    AS Length_of_Trip
  FROM
    `bigquery-public-data.san_francisco.bikeshare_trips`)
GROUP BY
  Length_of_Trip
ORDER BY
  Trips DESC
```
```
+--------+----------------+
| Trips  | Length_of_Trip |
+--------+----------------+
| 955557 | Less_Than_Hour |
|  28091 | More_Than_Hour |
+--------+----------------+
```

- Question 2: Is there an hour in the day that we have low ridership (People don't start trips during the hour)?
  * Answer: We find that we have the lowest ridership for the hours between 11pm and 5am (with 3am being the least utilized). There are also few riders around 9 and 10pm, with a little more (but still few) around 8pm and 6am.
  * SQL query:
```sql
SELECT
  COUNT(*) AS Number_of_Trips,
  EXTRACT(HOUR from start_date) as Start_Hour
FROM (
  SELECT
    start_date
  FROM
    `bigquery-public-data.san_francisco.bikeshare_trips`)
GROUP BY
  Start_Hour
ORDER BY
  Number_of_Trips ASC
```
```
+-----------------+------------+
| Number_of_Trips | Start_Hour |
+-----------------+------------+
|             605 |          3 |
|             877 |          2 |
|            1398 |          4 |
|            1611 |          1 |
|            2929 |          0 |
|            5098 |          5 |
|            6195 |         23 |
|           10270 |         22 |
|           15258 |         21 |
|           20519 |          6 |
|           22747 |         20 |
|           37852 |         14 |
|           40407 |         11 |
|           41071 |         19 |
|           42782 |         10 |
|           43714 |         13 |
|           46950 |         12 |
|           47626 |         15 |
|           67531 |          7 |
|           84569 |         18 |
|           88755 |         16 |
|           96118 |          9 |
|          126302 |         17 |
|          132464 |          8 |
+-----------------+------------+
```

- Question 3: Is there a day in the week for which we have low ridership?
  * Answer: Similar to what we found in a previous investigation, we have significantly lower ridership on the weekends than the weekdays. In addition, we can also see that there is also slightly less ridership on Friday as compared to the other weekdays. Overall though, Sunday has the least ridership closely followed by Saturday.
  * SQL query:
```sql
SELECT
    count(*) as Number_of_Trips,
        CASE EXTRACT(DAYOFWEEK FROM start_date)
             WHEN 1 THEN "Sunday"
             WHEN 2 THEN "Monday"
             WHEN 3 THEN "Tuesday"
             WHEN 4 THEN "Wednesday"
             WHEN 5 THEN "Thursday"
             WHEN 6 THEN "Friday"
             WHEN 7 THEN "Saturday"
         END AS Day_of_Week,
FROM
    (SELECT start_date FROM `bigquery-public-data.san_francisco.bikeshare_trips`)
GROUP BY Day_of_Week
ORDER BY Number_of_Trips ASC
```
```
+-----------------+-------------+
| Number_of_Trips | Day_of_Week |
+-----------------+-------------+
|           51375 | Sunday      |
|           60279 | Saturday    |
|          159977 | Friday      |
|          169937 | Monday      |
|          176908 | Thursday    |
|          180767 | Wednesday   |
|          184405 | Tuesday     |
+-----------------+-------------+
```

- Question 4: How many of our trips were what we intially define as commuter trips (Mon - Fri, 7-10am, 4-7pm, Different start and end stations, between 5 minutes and 1 hour)?
  * Answer: With our initial definition of commuter trips, we find that there are 291,397 trips that fit our description of commuter trips.
  * SQL query:
```sql
SELECT
  COUNT(*) AS Number_of_Trips,
FROM (
  SELECT
    start_date,
    start_station_name,
    end_station_name,
    CAST(ROUND(duration_sec / 60.0) AS INT64) AS duration_minutes,
    EXTRACT(HOUR
    FROM
      start_date) AS Start_Hour,
    EXTRACT(DAYOFWEEK
    FROM
      start_date) AS dow,
  FROM
    `bigquery-public-data.san_francisco.bikeshare_trips`)
WHERE
  start_station_name <> end_station_name
  AND ((Start_Hour > 6
      AND Start_Hour < 11)
    OR (Start_Hour > 4
      AND Start_Hour < 8))
  AND (dow > 1
    AND dow < 7)
  AND (duration_minutes >= 5
    AND duration_minutes <= 60)
```
```
+-----------------+
| Number_of_Trips |
+-----------------+
|          291397 |
+-----------------+
```

- Question 5: How many of the trips in the dataset are from subscribers?
  * Answer: We find that 846,839 trips in the dataset are subscribers, which we could also think of as around 86% of trips.
  * SQL query:
```sql
SELECT
  COUNT(*) AS Number_of_Trips,
  subscriber_type
FROM
  `bigquery-public-data.san_francisco.bikeshare_trips`
GROUP BY
  subscriber_type
```
```
+-----------------+-----------------+
| Number_of_Trips | subscriber_type |
+-----------------+-----------------+
|          136809 | Customer        |
|          846839 | Subscriber      |
+-----------------+-----------------+
```

- Question 6: Which stations have the most amount of trips starting from them?
  * Answer: We find that the stations that have the most trips starting from them are San Francisco Caltrain (Townsend at 4th), San Francisco Caltrain 2 (330 Townsend), Harry Bridges Plaza (Ferry Building), Embarcadero at Sansome, and 2nd at Townsend.
  * SQL query:
```sql
SELECT
  Count(*) as Number_of_Trips,
  start_station_name as Station
FROM
  `bigquery-public-data.san_francisco.bikeshare_trips`
GROUP BY
  start_station_name
ORDER BY
  Number_of_Trips DESC
LIMIT
  5
```
```
+-----------------+------------------------------------------+
| Number_of_Trips |                 Station                  |
+-----------------+------------------------------------------+
|           72683 | San Francisco Caltrain (Townsend at 4th) |
|           56100 | San Francisco Caltrain 2 (330 Townsend)  |
|           49062 | Harry Bridges Plaza (Ferry Building)     |
|           41137 | Embarcadero at Sansome                   |
|           39936 | 2nd at Townsend                          |
+-----------------+------------------------------------------+
```

- Question 7: Which stations have the least amount of trips starting from them?
  * Answer: We find that the stations that have the least number of trips starting from them are 5th St at E. San Salvador St, Sequoia Hospital, 5th S at E. San Salvador St, San Jose Government Center, and Middlefield Light Rail Station.
  * SQL query:
```sql
SELECT
  Count(*) as Number_of_Trips,
  start_station_name as Station
FROM
  `bigquery-public-data.san_francisco.bikeshare_trips`
GROUP BY
  start_station_name
ORDER BY
  Number_of_Trips ASC
LIMIT
  5
```
```
+-----------------+--------------------------------+
| Number_of_Trips |            Station             |
+-----------------+--------------------------------+
|               1 | 5th St at E. San Salvador St   |
|              15 | Sequoia Hospital               |
|              19 | 5th S at E. San Salvador St    |
|              23 | San Jose Government Center     |
|              66 | Middlefield Light Rail Station |
+-----------------+--------------------------------+
```

- Question 8: For those stations that had few trips starting from them, what was their total amount of trips?
  * Answer: We find that 5th St at E. San Salvador St had 2 total trips, Sequoia Hospital had 29 total trips, 5th S at E. San Salvador St had 43 total trips, San Jose Government Center had 46 total trips, and Middlefield Light Rail Station had 159 total trips.
  * SQL query:
```sql
SELECT
  table1.Num_Trips + table2.Number_of_Trips AS Trip_Total,
  table1.Station1 AS Station
FROM (
  SELECT
    COUNT(*) AS Num_Trips,
    start_station_name AS Station1
  FROM
    `bigquery-public-data.san_francisco.bikeshare_trips`
  GROUP BY
    start_station_name
  ORDER BY
    Num_Trips ASC
  LIMIT
    5)AS table1
LEFT JOIN (
  SELECT
    COUNT(*) AS Number_of_Trips,
    end_station_name AS Station2
  FROM
    `bigquery-public-data.san_francisco.bikeshare_trips`
  WHERE
    end_station_name IN ("5th St at E. San Salvador St",
      "Sequoia Hospital",
      "5th S at E. San Salvador St",
      "San Jose Government Center",
      "Middlefield Light Rail Station")
  GROUP BY
    end_station_name
  ORDER BY
    Number_of_Trips ASC) AS table2
ON
  table1.Station1 = table2.Station2
```
```
+------------+--------------------------------+
| Trip_Total |            Station             |
+------------+--------------------------------+
|          2 | 5th St at E. San Salvador St   |
|         29 | Sequoia Hospital               |
|         43 | 5th S at E. San Salvador St    |
|         46 | San Jose Government Center     |
|        159 | Middlefield Light Rail Station |
+------------+--------------------------------+
```
  
- Question 9: For these stations with a small number of trips, when were the built/implemented?
  * Answer:
  * SQL query:
```sql

```
```

```

---

## Part 3 - Employ notebooks to synthesize query project results

### Get Going

Create a Jupyter Notebook against a Python 3 kernel named Project_1.ipynb in the assignment branch of your repo.

#### Run queries in the notebook 

At the end of this document is an example Jupyter Notebook you can take a look at and run.  

You can run queries using the "bang" command to shell out, such as this:

```
! bq query --use_legacy_sql=FALSE '<your-query-here>'
```

- NOTE: 
- Queries that return over 16K rows will not run this way, 
- Run groupbys etc in the bq web interface and save that as a table in BQ. 
- Max rows is defaulted to 100, use the command line parameter `--max_rows=1000000` to make it larger
- Query those tables the same way as in `example.ipynb`

Or you can use the magic commands, such as this:

```sql
%%bigquery my_panda_data_frame

select start_station_name, end_station_name
from `bigquery-public-data.san_francisco.bikeshare_trips`
where start_station_name <> end_station_name
limit 10
```

```python
my_panda_data_frame
```

#### Report in the form of the Jupter Notebook named Project_1.ipynb

- Using markdown cells, MUST definitively state and answer the two project questions:

  * What are the 5 most popular trips that you would call "commuter trips"? 
  
  * What are your recommendations for offers (justify based on your findings)?

- For any temporary tables (or views) that you created, include the SQL in markdown cells

- Use code cells for SQL you ran to load into Pandas, either using the !bq or the magic commands

- Use code cells to create Pandas formatted output tables (at least 3) to present or support your findings

- Use code cells to create simple data visualizations using Seaborn / Matplotlib (at least 2) to present or support your findings

### Resource: see example .ipynb file 

[Example Notebook](example.ipynb)

