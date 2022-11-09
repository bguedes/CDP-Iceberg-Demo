# CDP-Iceberg-Demo
Example of Iceberg usage on CDP using CDW

## Impala

```sql
drop database if exists airlines cascade;

create database airlines;

create table airlines.unique_tickets
STORED AS ICEBERG AS
select * from airlines_csv.unique_tickets_csv;

create table airlines.airlines
STORED AS ICEBERG AS
select * from airlines_csv.airlines_csv;

create table airlines.airports
STORED AS ICEBERG AS
select * from airlines_csv.airports_csv;

create table airlines.planes
STORED AS ICEBERG AS
select * from airlines_csv.planes_csv;

create table airlines.flights
STORED AS ICEBERG AS
select * from airlines_csv.flights_csv;

```