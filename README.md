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

ALTER TABLE airlines.flights CHANGE COLUMN flightnum flightnum bigint;

### Deleting column

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
flightnum int,
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
