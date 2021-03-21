<p align="center"> <img src="materiale/loghi.png" width="315" height="100" /></p>

# WORK IN PROGRESS

#### Lezione 5
## SQL AVANZATO

Autore: Ferdinando Urbano  

---

1. DATE, TIMESTAMP, EXTRACT, TIMEZONE
2. UNION
2. JOIN di tabelle
3. LEFT JOIN
4. GROUP BY
5. HAVING
6. COALESCE


he `COALESCE` function returns the first of its arguments that is not null. Null is returned only if all arguments are null. It is often used to substitute a default value for null values when data is retrieved for display.

```sql
SELECT 
  gps_data_animals_id,
  animals_id,
  acquisition_time,
  coalesce(longitude, 0) AS longitude_with_0,
  coalesce(latitude, 0) AS latitude_with_0
FROM
  main.gps_data_animals
LIMIT 100;
```

7. CASE

The SQL `CASE` expression is a generic conditional expression, similar to if/else statements in other programming languages. The syntax is `CASE WHEN criteria THEN value END`. Here an example

```sql
SELECT
  gps_data_animals_id,
  animals_id,
  acquisition_time,
  longitude,
  latitude,
  roads_dist,
  CASE WHEN roads_dist < 1000 THEN 'close' ELSE 'far' END AS distance
FROM
  main.gps_data_animals
WHERE
  animals_id = 1 AND
  roads_dist IS NOT NULL
LIMIT 100;
```
8. Creare una tabella
9. Creare una VIEW
