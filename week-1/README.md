# Module 1 Homework: Docker & SQL

Related Docker, SQL, and Terraform code for Module 1 Homework


## Question 1. Understanding docker first run 

Run docker with the `python:3.12.8` image in an interactive mode, use the entrypoint `bash`.

What's the version of `pip` in the image?

Answer:

- 24.3.1

## Question 2. Understanding Docker networking and docker-compose

Given the following `docker-compose.yaml`, what is the `hostname` and `port` that **pgadmin** should use to connect to the postgres database?

```yaml
services:
  db:
    container_name: postgres
    image: postgres:17-alpine
    environment:
      POSTGRES_USER: 'postgres'
      POSTGRES_PASSWORD: 'postgres'
      POSTGRES_DB: 'ny_taxi'
    ports:
      - '5433:5432'
    volumes:
      - vol-pgdata:/var/lib/postgresql/data

  pgadmin:
    container_name: pgadmin
    image: dpage/pgadmin4:latest
    environment:
      PGADMIN_DEFAULT_EMAIL: "pgadmin@pgadmin.com"
      PGADMIN_DEFAULT_PASSWORD: "pgadmin"
    ports:
      - "8080:80"
    volumes:
      - vol-pgadmin_data:/var/lib/pgadmin  

volumes:
  vol-pgdata:
    name: vol-pgdata
  vol-pgadmin_data:
    name: vol-pgadmin_data
```

Answer:

- db:5433

## Question 3. Trip Segmentation Count

During the period of October 1st 2019 (inclusive) and November 1st 2019 (exclusive), how many trips, **respectively**, happened:
1. Up to 1 mile
2. In between 1 (exclusive) and 3 miles (inclusive),
3. In between 3 (exclusive) and 7 miles (inclusive),
4. In between 7 (exclusive) and 10 miles (inclusive),
5. Over 10 miles 

Answer:

- 104,838;  199,013;  109,645;  27,688;  35,202

``
WITH trip_segment AS (
SELECT 
	trip_distance,
	CASE WHEN trip_distance <= 1 THEN 'segment1'
	     WHEN trip_distance > 1 AND trip_distance <= 3 THEN 'segment2'
	     WHEN trip_distance > 3 AND trip_distance <= 7 THEN 'segment3'
	     WHEN trip_distance > 7 AND trip_distance <= 10 THEN 'segment4'
	     ELSE 'segment5'
	     END AS segment
FROM public.green_october_2019
)
SELECT segment, count(*)
FROM trip_segment
GROUP BY 1
ORDER BY 1
``

## Question 4. Longest trip for each day

Which was the pick up day with the longest trip distance?
Use the pick up time for your calculations.

Tip: For every day, we only care about one single trip with the longest distance. 

Answer:

- 2019-10-31

``
SELECT lpep_pickup_datetime
FROM public.green_october_2019
ORDER BY trip_distance DESC 
LIMIT 1;
``

## Question 5. Three biggest pickup zones

Which were the top pickup locations with over 13,000 in
`total_amount` (across all trips) for 2019-10-18?

Consider only `lpep_pickup_datetime` when filtering by date.
 
Answer:

- East Harlem North, East Harlem South, Morningside Heights

``
SELECT l."Zone" , SUM(total_amount) AS total_amount_by_zone
FROM public.green_october_2019 t
LEFT JOIN public.taxi_zone_lookup l ON t."PULocationID"  = l."LocationID" 
WHERE DATE(lpep_pickup_datetime) = '2019-10-18'
GROUP BY 1 
HAVING SUM(total_amount) > 13000.00
ORDER BY 2 DESC
``

## Question 6. Largest tip

For the passengers picked up in October 2019 in the zone
named "East Harlem North" which was the drop off zone that had
the largest tip?

Note: it's `tip` , not `trip`

We need the name of the zone, not the ID.


Answer: 
- East Harlem North

``
SELECT l."Zone" , SUM(tip_amount) AS total_tip
FROM public.green_october_2019 t
LEFT JOIN public.taxi_zone_lookup l ON t."PULocationID"  = l."LocationID" 
GROUP BY 1
ORDER BY 2 DESC
LIMIT 1;
``

## Question 7. Terraform Workflow

Which of the following sequences, **respectively**, describes the workflow for: 
1. Downloading the provider plugins and setting up backend,
2. Generating proposed changes and auto-executing the plan
3. Remove all resources managed by terraform`

Answer:

- terraform init, terraform apply -auto-approve, terraform destroy

Related code in `terraform` folder