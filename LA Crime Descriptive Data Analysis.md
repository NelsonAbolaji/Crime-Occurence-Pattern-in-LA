### LA Crime Descriptive Data Analysis

This markdown document contains queries that were written to pull results from the table and further understand the problem statement.



### Queries

It takes a relatively long time to query the table. An index may help reduce query time

```sql
--Index on the primary key
CREATE INDEX CONCURRENTLY primary_key_index
ON la_crime(dr_no, date_rptd);
```

- What is the span of the data?
  
  ```sql
  SELECT CONCAT(MIN(date_occ),' ', ' -',' ', MAX(date_occ)) AS date_span
  FROM la_crime;
  --The record runs from 01-01-2020 to 23-10-2023.
  ```



###### Crime Analysis by Time

- How many crimes took place each year(The year 2023 hasn't ended so its data is incomplete)?

```sql
SELECT EXTRACT(year FROM date_occ) AS year,
        to_char(COUNT(*), '999,999,999') AS no_of_crime
FROM la_crime
GROUP BY EXTRACT(year FROM date_occ)
ORDER BY year;
```

*2020 had a slightly lower crime rate than the rest of the years. This may be due to the COVID-19 pandemic.*

- Average crime count per year
  
  ```sql
  WITH t1 AS(
  SELECT EXTRACT(year FROM date_occ) AS year,
          CASE WHEN COUNT(DISTINCT DATE_TRUNC('month', date_occ)) =12 THEN 1
              ELSE 0 END AS no_of_complete_years
      FROM la_crime
      GROUP BY EXTRACT(year FROM date_occ)
      ),
  
  t2 AS (
  SELECT EXTRACT(year FROM date_occ) AS year,
      (CASE WHEN COUNT(DISTINCT DATE_TRUNC('month', date_occ)) =12 THEN COUNT(*) 
              ELSE NULL END) AS total_cases_per_year
  FROM la_crime
  GROUP BY EXTRACT(year FROM date_occ)
      )
  
  SELECT to_char(ROUND(SUM(total_cases_per_year)::numeric/SUM(no_of_complete_years)::numeric),
                 '999,999,999') AS avg_cases_per_year
  FROM t1
  INNER JOIN t2
  USING (year);
  ```

- Average crime count per month
  
  ```sql
  SELECT to_char(COUNT(*)/COUNT(DISTINCT DATE_TRUNC('month', date_occ)), '999,999,999') AS avg_crime_count_per_month
  FROM la_crime;
  ```

- Average crime count per day

```sql
SELECT COUNT(*)/COUNT(DISTINCT DATE_TRUNC('day', date_occ)) AS avg_crime_count_per_day
FROM la_crime;
```

- Average crime rate for each day of the week
  
  ```sql
  WITH t1 AS(
      SELECT to_char(date_occ, 'Day') AS day,
          COUNT(*) AS total_days
      FROM la_crime
      GROUP BY to_char(date_occ, 'Day')
      ),
  
  t2 AS (
      SELECT to_char(date_occ, 'Day') AS day,
              COUNT(DISTINCT date_occ) AS distinct_days
      FROM la_crime
      GROUP BY to_char(date_occ, 'Day')
      )
  
  SELECT t1.day, total_days/distinct_days AS avg_crime
  FROM t1
  INNER JOIN t2
  USING (day)
  ORDER BY avg_crime DESC;
  ```

- Crime count by distinct month
  
  ```sql
  SELECT to_char(date_occ, 'Mon') AS month,
          COUNT(*) AS crime_count
  FROM la_crime
  WHERE date_occ NOT BETWEEN '01-01-2023' AND '12-31-2023'
  GROUP BY month
  ORDER BY crime_count DESC;
  ```

- Is there a noticeable trend in the way crimes are committed each month over the years?
  
  ```sql
  SELECT RANK() OVER (PARTITION BY DATE_TRUNC('year', date) ORDER BY no_of_crime DESC) AS rank,
          date,
          no_of_crime
  FROM (
  SELECT DATE_TRUNC('month', date_occ)::date AS date,
          to_char(COUNT(*), '999,999,999') AS no_of_crime
  FROM la_crime
  GROUP BY date_trunc('month', date_occ)
      ) AS t1
  ORDER BY rank;
  ```

*There doesn't seem to be a noticeable trend in crime rate along continuous months*

- On what days of the week are crimes committed the most?
  
  ```sql
  SELECT RANK() OVER (ORDER BY no_of_crime DESC) AS rank,
          day,
          no_of_crime
  FROM (
  SELECT to_char(date_occ, 'Dy') AS day,
          to_char(COUNT(*), '999,999,999') AS no_of_crime
  FROM la_crime
  GROUP BY to_char(date_occ, 'Dy')
      ) AS t1
  ORDER BY rank;
  ```

*Weekdays recorded the higher crime rates, with the crime rate reducing as the days gets closer to the days in the weekend. This may be because a lot of people are at work on those days. A look into the time these crimes occured on weekdays may help answer the question better.*

- What hours are crimes mostly committed?
  
  ```sql
  SELECT DATE_TRUNC('hour',time_occ) AS hour_of_the_day,
          to_char(COUNT(*), '999,999,999') AS no_of_crime
  FROM la_crime
  GROUP BY hour_of_the_day
  ORDER BY no_of_crime DESC;
  ```

- What hours on weekdays are crimes mostly committed?
  
  ```sql
  SELECT DATE_TRUNC('hour',time_occ) AS hour_of_the_day,
          to_char(COUNT(*), '999,999,999') AS no_of_crime
  FROM la_crime
  WHERE to_char(date_occ, 'Dy') NOT IN ('Sat','Sun')
  GROUP BY hour_of_the_day
  ORDER BY no_of_crime DESC;
  --Daylight hours is when crimes are mostly committed. Binning the hours will make this clearer
  
  --What period of the day on weekdays are crimes most committed?
  SELECT CASE WHEN hour_of_the_day BETWEEN '5:00:00' AND '11:00:00' THEN 'morning'
                  WHEN hour_of_the_day BETWEEN '12:00:00'AND '17:00:00' THEN 'afternoon'
                  WHEN hour_of_the_day BETWEEN '18:00:00' AND '20:00:00' THEN 'evening'
                  ELSE 'night' END AS period_of_the_day,
                  to_char(SUM(no_of_crime), '999,999,999') AS no_of_crime
  FROM(
  SELECT DATE_TRUNC('hour',time_occ) AS hour_of_the_day,
          COUNT(*) AS no_of_crime
  FROM la_crime
  WHERE to_char(date_occ, 'Dy') NOT IN ('Sat','Sun')
  GROUP BY hour_of_the_day
      ) AS t1
  GROUP BY period_of_the_day
  ORDER BY no_of_crime DESC;
  ```

*Crimes that occur on weekdays mostly happen between the hours of 12:00:00 and 17:00:00*

- What period of the day on weekends are crimes most committed?
  
  ```sql
  SELECT CASE WHEN hour_of_the_day BETWEEN '5:00:00' AND '11:00:00' THEN 'morning'
                  WHEN hour_of_the_day BETWEEN '12:00:00'AND '17:00:00' THEN 'afternoon'
                  WHEN hour_of_the_day BETWEEN '18:00:00' AND '20:00:00' THEN 'evening'
                  ELSE 'night' END AS period_of_the_day,
                  to_char(SUM(no_of_crime), '999,999,999')  AS no_of_crime
  FROM(
  SELECT DATE_TRUNC('hour',time_occ) AS hour_of_the_day,
          COUNT(*) AS no_of_crime
  FROM la_crime
  WHERE to_char(date_occ, 'Dy') NOT IN ('Mon','Tue', 'Wed', 'Thu','Fri')
  --Hardcoding the values to filter by instead of writing a WHERE subquery reduces the query time
  GROUP BY hour_of_the_day
      ) AS t1
  GROUP BY period_of_the_day
  ORDER BY no_of_crime DESC;
  ```

*Crimes mostly happens in the night and afternoon on weekends*

- Crime distribution by weekday and weekend (weekday and weekend crime total and percentage  by weekpart )
  
  ```sql
  WITH t1 AS (--weekday table
      SELECT CASE WHEN hour_of_the_day BETWEEN '5:00:00' AND '11:00:00' THEN 'morning'
                  WHEN hour_of_the_day BETWEEN '12:00:00'AND '17:00:00' THEN 'afternoon'
                  WHEN hour_of_the_day BETWEEN '18:00:00' AND '20:00:00' THEN 'evening'
                  ELSE 'night' END AS period_of_the_day,
                  SUM(no_of_crime) AS weekday_crime_count
  FROM(
  SELECT DATE_TRUNC('hour',time_occ) AS hour_of_the_day,
          COUNT(*) AS no_of_crime
  FROM la_crime
  WHERE to_char(date_occ, 'Dy') NOT IN ('Sat','Sun')
  GROUP BY hour_of_the_day
      ) AS t1
  GROUP BY period_of_the_day
      ),
  
  t2 AS (--weekend table
  SELECT CASE WHEN hour_of_the_day BETWEEN '5:00:00' AND '11:00:00' THEN 'morning'
                  WHEN hour_of_the_day BETWEEN '12:00:00'AND '17:00:00' THEN 'afternoon'
                  WHEN hour_of_the_day BETWEEN '18:00:00' AND '20:00:00' THEN 'evening'
                  ELSE 'night' END AS period_of_the_day,
                  SUM(no_of_crime) AS weekend_crime_count
  FROM(
  SELECT DATE_TRUNC('hour',time_occ) AS hour_of_the_day,
          COUNT(*) AS no_of_crime
  FROM la_crime
  WHERE to_char(date_occ, 'Dy') NOT IN ('Mon','Tue', 'Wed', 'Thu','Fri')
  GROUP BY hour_of_the_day
      ) AS t1
  GROUP BY period_of_the_day
          )
  
  SELECT (CASE WHEN period_of_the_day='morning' THEN 1
          WHEN period_of_the_day='afternoon' THEN 2
          WHEN period_of_the_day='evening' THEN 3
          ELSE 4 END) AS sn,
          period_of_the_day, 
          to_char(weekday_crime_count, '999,999,999') AS weekday_crime_count,
          ROUND(weekday_crime_count/(SELECT COUNT(*)
                                      FROM la_crime)*100, 1) || '%' AS weekday_percent_of_total,
          to_char(weekend_crime_count, '999,999,999') AS weekend_crime_count,
          ROUND(weekend_crime_count/(SELECT COUNT(*)
                                      FROM la_crime)*100, 1) || '%' AS weekend_percent_of_total
  FROM t1
  INNER JOIN t2
  USING (period_of_the_day)
  ORDER BY sn;
  ```

- Crime count by crime category and day of the week 
  
  ```sql
  SELECT CASE WHEN hour_of_the_day BETWEEN '5:00:00' AND '11:00:00' THEN 'morning'
                  WHEN hour_of_the_day BETWEEN '12:00:00'AND '17:00:00' THEN 'afternoon'
                  WHEN hour_of_the_day BETWEEN '18:00:00' AND '20:00:00' THEN 'evening'
                  ELSE 'night' END AS period_of_the_day,
                  crime_category,
                  COUNT(*) AS crime_count
  FROM crime_group
  LEFT JOIN(
      SELECT crm_cd,
          DATE_TRUNC('hour',time_occ) AS hour_of_the_day
      FROM la_crime
          ) AS t2
  USING (crm_cd)
  GROUP BY crime_category, period_of_the_day
  ORDER BY crime_count DESC;
  ```

- How long does it take to report a crime?

```sql
SELECT DISTINCT days,
                COUNT(days) AS no_of_reports
FROM(
    SELECT (date_occ - date_rptd) AS days
    FROM la_crime
    ) AS t1
GROUP BY days
ORDER BY days;
```

*All crimes were reported on the day it occurred*



###### Crime rate by Area

- Which geographic area do most of these crimes occur?
  
  ```sql
  SELECT area_name,
          COUNT(*) AS no_of_crime
  FROM la_crime
  GROUP BY area_name
  ORDER BY no_of_crime DESC;
  ```

- On average, what is the crime rate per day in each area?
  
  ```sql
  SELECT ROW_NUMBER() OVER(ORDER BY COUNT(*)/COUNT(DISTINCT DATE_TRUNC('day', date_occ)) DESC) AS sn,
          area_name,
          COUNT(*)/COUNT(DISTINCT DATE_TRUNC('day', date_occ)) AS avg_crime_count_per_day
  FROM la_crime
  GROUP BY area_name
  ORDER BY avg_crime_count_per_day DESC;
  ```

- Top crimes by area name
  
  ```sql
  WITH crm_by_area AS (
      SELECT RANK() OVER(PARTITION BY area_name ORDER BY COUNT(*) DESC) AS rnk,
          area_name,
          crm_cd_desc,
          COUNT(*) AS crime_count
      FROM la_crime
      GROUP BY area_name, crm_cd_desc
      )
  
  SELECT area_name,
          crm_cd_desc,
          crime_count
  FROM crm_by_area
  WHERE rnk = 1
  ORDER BY crime_count DESC;
  ```

- Count of crime by crime description
  
  ```sql
  SELECT ROW_NUMBER() OVER(ORDER BY COUNT(*) DESC) AS sn,
          crm_cd_desc,
          to_char(COUNT(*), '999,999,999') AS count_of_crime
  FROM la_crime
  GROUP BY crm_cd_desc
  ORDER BY count_of_crime DESC;
  ```

- Areas where distinct crimes are most popular
  
  ```sql
  WITH t1 AS (
      SELECT RANK() OVER(PARTITION BY crm_cd_desc ORDER BY COUNT(*) DESC) AS rnk,
          crm_cd_desc,
          area_name,
          COUNT(*) AS crime_count
      FROM la_crime
      GROUP BY crm_cd_desc, area_name
      )
  
  SELECT ROW_NUMBER() OVER(ORDER BY crime_count DESC) AS sn,
          INITCAP(crm_cd_desc) AS crm_cd_desc,
          area_name,
          to_char(crime_count, '999,999,999') AS crime_count
  FROM t1
  WHERE rnk = 1
  ORDER BY crime_count DESC;
  ```



###### Fatal Crime

- Temporary table for fatal crimes (Criminal Homicide, Manslaughter, Negligent, Lynching)
  
  ```sql
  CREATE TEMP TABLE fatal_crime AS (
  SELECT area_name, 
  		COUNT(*) AS count_of_fatal_crimes
  FROM crime_group AS cg
  LEFT JOIN la_crime
  USING (crm_cd)
  WHERE cg.crm_cd_desc='Criminal Homicide' 
  		OR cg.crm_cd_desc='Manslaughter, Negligent' 
  		OR cg.crm_cd_desc='Lynching'
  GROUP BY area_name
  	);
  
  ```

- Count of fatal crime occurence by area
  
  ```sql
  WITH t1 AS (
  SELECT area_name,
  		count_of_fatal_crimes,
  		ROUND((count_of_fatal_crimes/(SELECT SUM(count_of_fatal_crimes)
  									FROM fatal_crime)*100) ,1) || '%' AS percent_of_fatal_crime_by_area 
  FROM fatal_crime
  	),
  
  t2 AS (
  	SELECT area_name, COUNT(*) AS total_crime_by_area,
  			ROUND((COUNT(*)::numeric/(SELECT COUNT(*)::numeric AS x
  							   FROM la_crime))*100, 1) || '%' AS percent_of_total_crime
  	FROM la_crime
  	GROUP BY area_name
  	)
  	
  SELECT area_name, count_of_fatal_crimes, 
  		RANK() OVER(ORDER BY count_of_fatal_crimes DESC) || '(' || percent_of_fatal_crime_by_area || ')' AS fatal_crime_rank_by_area,
  		RANK() OVER(ORDER BY total_crime_by_area DESC) || '(' || percent_of_total_crime || ')' AS crime_rank_by_area
  FROM t1
  LEFT JOIN t2
  USING (area_name)
  ORDER BY count_of_fatal_crimes DESC; 
  ```
  
  
  
  ###### Victim's age analysis

- Temporary table for Victim's age analysis

```sql
CREATE TEMP TABLE vict_age_table AS(
    SELECT crm_cd_desc, vict_age
    FROM la_crime
    WHERE vict_age =0 AND crm_cd_desc LIKE '%CHILD%' OR crm_cd_desc LIKE '%MINOR%' 
            OR crm_cd_desc LIKE 'KID%' OR crm_cd_desc LIKE 'RAPE%' OR crm_cd_desc = 'SEXUAL PENETRATION W/FOREIGN OBJECT'


    UNION ALL

    SELECT crm_cd_desc, vict_age
    FROM la_crime
    WHERE vict_age > 0
        );
```

- Crime distribution by age
  
  ```sql
  SELECT vict_age, COUNT(*) AS crime_count
  FROM vict_age_table
  GROUP BY vict_age
  ORDER BY vict_age;
  ```

*The rate of crime occurence is dependent on victim's age*

- Crime count by 5 year age bin

```sql
SELECT (vict_age/5)*5 AS five_yr_bin,
        COUNT(*) AS crime_count
FROM vict_age_table
GROUP BY five_yr_bin
ORDER BY five_yr_bin;
```

- Crime description by age bin

```sql
WITH bin_table AS(
    SELECT DISTINCT (vict_age/5)*5 AS five_yr_bin, vict_age
    FROM vict_age_table
        ),

vict_age_table AS (
    SELECT crm_cd_desc, vict_age
    FROM vict_age_table
    )

SELECT  RANK() OVER(PARTITION BY five_yr_bin ORDER BY COUNT(*) DESC) AS rnk,
        five_yr_bin,
        crm_cd_desc,
        to_char(COUNT(*), '999,999,999') AS crime_count
FROM vict_age_table
INNER JOIN bin_table 
ON bin_table.vict_age=vict_age_table.vict_age
GROUP BY five_yr_bin, crm_cd_desc
ORDER BY five_yr_bin;
```

*This result is largely missing information about the age group(s) whose vehicles got stolen from them*



###### Victim's sex analysis

- Temporary table for victim's sex analysis

```sql
CREATE TEMP TABLE vict_sex_table AS (
	SELECT crm_cd_desc, vict_sex
	FROM la_crime
	WHERE vict_sex IS NOT NULL
	);

UPDATE vict_sex_table
SET vict_sex = 'X'
WHERE vict_sex='H' OR vict_sex='-';
```

- Crime count by victim's sex
  
  ```sql
  SELECT CASE WHEN vict_sex='M' THEN 'Male'
  		WHEN vict_sex='F' THEN 'Female'
  		WHEN vict_sex='X' THEN 'Unknown'
  		END AS vict_sex,
  		to_char(COUNT(*), '999,999,999') AS crime_count
  FROM vict_sex_table
  GROUP BY vict_sex
  ORDER BY crime_count DESC;
  ```
  
  *Males are victims of more crimes than females*

- Crime count by sex and crime category
  
  ```sql
  SELECT crime_category,
  		vict_sex,
  		COUNT(*) AS crime_count,
  		ROUND((COUNT(*)*1.0/(SELECT COUNT(*)
  				 			FROM vict_sex_table
  							WHERE vict_sex <> 'X')*1.0)*100, 1) || '%' AS percent_of_total
  FROM vict_sex_table
  LEFT JOIN crime_group
  USING (crm_cd_desc)
  WHERE vict_sex <> 'X'
  GROUP BY crime_category, vict_sex
  ORDER BY crime_count DESC;
  
  ```

- Crime count by sex and crime description
  
  ```sql
  SELECT (CASE WHEN vict_sex='M' THEN 'Male'
  		WHEN vict_sex='F' THEN 'Female'
  		WHEN vict_sex='X' THEN 'Unknown'
  		END) AS vict_sex,
  		crm_cd_desc,
  		to_char(COUNT(*), '999,999,999') AS crime_count
  FROM vict_sex_table
  GROUP BY vict_sex, crm_cd_desc
  ORDER BY crime_count DESC;
  ```

- Top 5 crime description by distinct victim's sex
  
  ```sql
  WITH top_five AS (
  	SELECT RANK() OVER(PARTITION BY vict_sex ORDER BY COUNT(*) DESC) AS rnk,
  		(CASE WHEN vict_sex='M' THEN 'Male'
  		WHEN vict_sex='F' THEN 'Female'
  		WHEN vict_sex='X' THEN 'Unknown'
  		END) AS vict_sex,
  		crm_cd_desc,
  		to_char(COUNT(*), '999,999,999') AS crime_count
  	FROM vict_sex_table
  	GROUP BY vict_sex, crm_cd_desc
  	ORDER BY vict_sex, crime_count DESC
  	)
  
  SELECT *
  FROM top_five
  WHERE rnk BETWEEN 1 AND 5 AND vict_sex <> 'Unknown'
  ORDER BY vict_sex;
  ```

*Assault is the top 1 and 2 crime females are victims of, while it is top 1 for males*



###### Victim's descent analysis

- Temporary table for victim's descent analysis
  
  ```sql
  CREATE TEMP TABLE vict_descent_table AS (
  	SELECT crm_cd_desc, vict_descent, status_desc
  	FROM la_crime
  	WHERE vict_descent IS NOT NULL
  	);
  
  UPDATE vict_descent_table
  SET vict_descent = 'X'
  WHERE vict_descent='-';
  ```

- Crime count by victim's descent
  
  ```sql
  SELECT ROW_NUMBER() OVER(ORDER BY COUNT(*) DESC) AS rn,
  		descent_desc, 
  		to_char(COUNT(*), '999,999,999') AS crime_count
  FROM vict_descent_table
  RIGHT JOIN victim_ethnicity
  ON vict_descent_table.vict_descent=victim_ethnicity.descent_cd
  GROUP BY descent_desc
  ORDER BY crime_count DESC;
  ```

*The top three descents who are victims of crime are 'Hispanic/Latin/Mexican', 'White' and 'Black'*

- Crime category by victim's descent
  
  ```sql
  WITH t1 AS (
  SELECT descent_desc, 
  		crime_grouped, COUNT(*) AS crime_count, 
  		(SELECT COUNT(*)
  		FROM la_crime) AS total_crime
  FROM vict_descent_table
  LEFT JOIN victim_ethnicity
  ON vict_descent_table.vict_descent=victim_ethnicity.descent_cd
  LEFT JOIN crime_group
  USING(crm_cd_desc)
  GROUP BY descent_desc, crime_grouped
  ORDER BY crime_count DESC
  	)
  	
  SELECT descent_desc, 
  		crime_grouped, 
  		crime_count, 
  		ROUND((SUM(crime_count)/SUM(total_crime))*100, 2) || '%' AS percent_of_total
  FROM t1
  WHERE descent_desc <> 'Unknown'
  GROUP BY descent_desc, crime_grouped, crime_count
  ORDER BY crime_count DESC;
  ```

- Top 5 crime by victim's descent

```sql
WITH descent_rank AS(
	SELECT RANK() OVER(PARTITION BY descent_desc ORDER BY COUNT(*) DESC) AS rnk,
			descent_desc, 
			crm_cd_desc,
			to_char(COUNT(*), '999,999,999') AS crime_count
	FROM vict_descent_table
	RIGHT JOIN victim_ethnicity
	ON vict_descent_table.vict_descent=victim_ethnicity.descent_cd
	GROUP BY descent_desc, crm_cd_desc
	)

SELECT *
FROM descent_rank
WHERE rnk BETWEEN 1 AND 5 AND descent_desc <> 'Unknown' 
ORDER BY descent_desc;
```



###### Crime by Premis analysis

- Top premis_desc by crime count

```sql
SELECT premis_desc,
		to_char(COUNT(*),'999,999,999') AS crime_count
FROM la_crime
WHERE premis_desc IS NOT NULL
GROUP BY premis_desc
ORDER BY crime_count DESC;
```



###### Crime by Weapon description analysis

```sql
SELECT weapon_desc,
	COUNT(*) AS crime_count
FROM la_crime
WHERE weapon_desc IS NOT NULL
GROUP BY weapon_desc
ORDER BY crime_count DESC;
```



###### Percentage of cases solved and unsolved

- Percentage of cases solved and unsolved
  
  ```sql
  SELECT ROUND((COUNT(CASE WHEN status_desc='Adult Arrest' OR status_desc='Adult Other' 
  	 	 OR status_desc='Juv Arrest' OR status_desc='Juv Other' THEN 'Closed' END)::numeric/
  				COUNT(*)::numeric *100), 2) || '%' AS closed,
  		ROUND((COUNT(CASE WHEN status_desc='Invest Cont' THEN 'Open' END)::numeric/
  		 		COUNT(*)::numeric *100), 2) || '%' AS open
  FROM la_crime
  WHERE status_desc <> 'UNK';
  ```

*80% of the cases are still open i.e. unsolved*

- Percentage of cases solved and unsolved on average each year

```sql
WITH t1 AS (
SELECT EXTRACT(year FROM date_occ) AS year,
		(CASE WHEN COUNT(DISTINCT DATE_TRUNC('month', date_occ)) =12 THEN 1
			ELSE 0 END) AS no_of_complete_years
FROM la_crime
GROUP BY EXTRACT(year FROM date_occ)
		),

t2 AS ( 
SELECT EXTRACT(year FROM date_occ) AS year,
		COUNT(*) AS total_cases,
	COUNT(CASE WHEN status_desc='Adult Arrest' OR status_desc='Adult Other' 
	 	 OR status_desc='Juv Arrest' OR status_desc='Juv Other' THEN 'Closed' END) AS closed,
	COUNT(CASE WHEN status_desc='Invest Cont' THEN 'Open' END) AS open
FROM la_crime
WHERE status_desc <> 'UNK'
GROUP BY EXTRACT(year FROM date_occ)
		)
		
SELECT ROUND((SUM(closed)/SUM(total_cases))*100, 2)|| '%' AS avg_closed_cases
FROM t2
LEFT JOIN t1
USING (year)
WHERE no_of_complete_years =1;
```


