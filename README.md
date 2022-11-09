# CDP-Iceberg-Demo
Example of Iceberg usage on CDP using CDW


## Hive

### Create Iceberg Tables

```sql
drop database if exists airlines cascade;

create database airlines;

create table airlines.unique_tickets
STORED BY ICEBERG AS
select * from airlinedata.unique_tickets_csv;

create table airlines.airlines
STORED BY ICEBERG AS
select * from airlinedata.airlines_csv;

create table airlines.airports
STORED BY ICEBERG AS
select * from airlinedata.airports_csv;

create table airlines.planes
STORED BY ICEBERG AS
select * from airlinedata.planes_csv;

create table airlines.flights
STORED BY ICEBERG AS
select * from airlinedata.flights_csv;

```

## Impala

### Create Iceberg Tables

```sql
drop database if exists airlines cascade;

create database airlines;

create table airlines.unique_tickets
STORED AS ICEBERG AS
select * from airlinedata.unique_tickets_csv;

create table airlines.airlines
STORED AS ICEBERG AS
select * from airlinedata.airlines_csv;

create table airlines.airports
STORED AS ICEBERG AS
select * from airlinedata.airports_csv;

create table airlines.planes
STORED AS ICEBERG AS
select * from airlinedata.planes_csv;

create table airlines.flights
STORED AS ICEBERG AS
select * from airlinedata.flights_csv;

```

Show Table fligths decription

```sql
SHOW CREATE TABLE airlines.flights;
```

### Run simple query

```sql
SELECT year, count(*) 
FROM airlines.flights
GROUP BY year
ORDER BY year desc;

```

### Change Table Partition

```sql
ALTER TABLE airlines.flights
SET PARTITION spec (year );
```

### Show new Table description

```sql
SHOW CREATE TABLE airlines.flights;
```

### Time Travel


```sql
select * from airlines.flights.history;
 
select year, count(*) from airlines.flights
FOR SYSTEM_VERSION AS OF <FirstSNAPSHOTID>
group by year
order by year desc;
  
select year, count(*) from airlines.flights
FOR SYSTEM_VERSION AS OF <SecondSNAPSHOTID>
group by year
order by year desc;
```


### Changing colunm type

```sql
ALTER TABLE airlines.flights CHANGE COLUMN flightnum flightnum bigint;
```

Inserting new data for 2021 

```sql
INSERT INTO TABLE airlines.flights VALUES (
7,
22,
3,
1606,
1300,
8,
2131,
'NW',
842,
'N540US',
302,
331,
286,
157,
186,
'ANC',
'MSP',
2518,
3,
13,
0,
'NA',
0,
NULL,
NULL,
NULL,
NULL,
NULL,
2021);
```

Check new snapshot


```sql
select * from airlines.flights.history;
 
select year, count(*) from airlines.flights
FOR SYSTEM_VERSION AS OF <FirstSNAPSHOTID>
group by year
order by year desc;
  
select year, count(*) from airlines.flights
FOR SYSTEM_VERSION AS OF <SecondSNAPSHOTID>
group by year
order by year desc;
```

### Deleting column

deleting column cancellationcode

```sql
ALTER TABLE airlines.flights REPLACE COLUMNS ( 
month int,
dayofmonth int,
dayofweek int,
deptime int,
crsdeptime int,
arrtime int,
crsarrtime int,
uniquecarrier string,
flightnum bigint,
tailnum string,
actualelapsedtime int,
crselapsedtime int,
airtime int,
arrdelay int,
depdelay int,
origin string,
dest string,
distance int,
taxiin int,
taxiout int,
cancelled int,
diverted string,
carrierdelay int,
weatherdelay int,
nasdelay int,
securitydelay int,
lateaircraftdelay int,
year int);
```
Insert new data for 2022

```sql
INSERT INTO TABLE airlines.flights VALUES (
7,
22,
3,
1606,
1300,
8,
2131,
'NW',
842,
'N540US',
302,
331,
286,
157,
186,
'ANC',
'MSP',
2518,
3,
13,
0,
0,
NULL,
NULL,
NULL,
NULL,
NULL,
2021);
```

Using Impala with Iceberg Tables<br>
https://impala.apache.org/docs/build/html/topics/impala_iceberg.html

Hive<br>
https://iceberg.apache.org/docs/latest/hive/
