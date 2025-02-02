### Quiz Questions

Complete the Quiz shown below. It’s a set of 6 multiple-choice questions to test your understanding of workflow orchestration, Kestra and ETL pipelines for data lakes and warehouses.

1) Within the execution for `Yellow` Taxi data for the year `2020` and month `12`: what is the uncompressed file size (i.e. the output file `yellow_tripdata_2020-12.csv` of the `extract` task)?
- 128.3 MB

To get file size from extract step, disable task that purges download file at the end. 

![image](https://github.com/user-attachments/assets/73438b17-2896-4711-bbdc-5dbee08b380d)

2) What is the rendered value of the variable `file` when the inputs `taxi` is set to `green`, `year` is set to `2020`, and `month` is set to `04` during execution?
- `green_tripdata_2020-04.csv`


3) How many rows are there for the `Yellow` Taxi data for all CSV files in the year 2020?
- 24,648,499

SELECT COUNT(*)
FROM yellow_tripdata 
WHERE DATE(tpep_pickup_datetime) < DATE('2021-01-01')

4) How many rows are there for the `Green` Taxi data for all CSV files in the year 2020?
- 1,734,051

SELECT COUNT(*)
FROM green_tripdata 
WHERE DATE(lpep_pickup_datetime) < DATE('2021-01-01')

5) How many rows are there for the `Yellow` Taxi data for the March 2021 CSV file?
- 1,925,152

SELECT COUNT(*)
FROM yellow_tripdata 
WHERE date_trunc('month', tpep_pickup_datetime)  = '2021-03-01'

6) How would you configure the timezone to New York in a Schedule trigger?
- Add a `timezone` property set to `America/New_York` in the `Schedule` trigger configuration

![image](https://github.com/user-attachments/assets/762652d2-4425-46f1-8cbe-41f4fc3a2ca0)

Source: 
https://kestra.io/docs/workflow-components/triggers/schedule-trigger


## Submitting the solutions

* Form for submitting: https://courses.datatalks.club/de-zoomcamp-2025/homework/hw2
* Check the link above to see the due date

## Solution

Will be added after the due date
