## LA Crime Table Creation Statement

#### LA Crime Table (Primary Table)

- LA Crime table creation statement

```sql
CREATE TABLE la_crime (
	dr_no INT PRIMARY KEY NOT NULL,
	date_rptd TEXT,
	date_occ TEXT,
	time_occ TEXT,
	area SMALLINT,
	area_name VARCHAR, 
	rpt_dist_no SMALLINT,
	part_1_or_2 SMALLINT, 
	crm_cd SMALLINT,
	crm_cd_desc TEXT,
	mocodes TEXT,
	vict_age SMALLINT,
	vict_sex CHAR(1),
	vict_descent CHAR(1),
	premis_cd SMALLINT,
	premis_desc TEXT,
	weapon_used_cd SMALLINT, 
	weapon_desc TEXT,
	status TEXT,
	status_desc VARCHAR,
	crm_cd_1 SMALLINT,
	crm_cd_2 SMALLINT,
	crm_cd_3 SMALLINT,
	crm_cd_4 SMALLINT,
	location VARCHAR,
	cross_street TEXT,
	lat NUMERIC,
	lon NUMERIC
	);

```

- Data importation from local storage
  
  ```sql
  COPY la_crime
  FROM 'C:\Users\lenovo\Desktop\PostgreSQL_Files\LA Crime Analysis\Crime_Data_from_2020_to_Present_20231028.csv'
  DELIMITER ','
  CSV HEADER ;
  ```

- Formatting columns to their appropriate datatypes
  
  ```sql
  ALTER TABLE la_crime
  /*The date columns are being formatted from timestamp to date 
  because they contain default time values and not actual time stamps */
  ALTER COLUMN date_rptd TYPE date
  USING date_rptd::date,
  ALTER COLUMN date_occ TYPE date
  USING date_rptd::date,
  ALTER COLUMN time_occ TYPE time
  USING time_occ::time;
  ```

- Changing case of string columns in uppercase to initial capital 
  
  ```sql
  ALTER TABLE la_crime
  ALTER COLUMN crm_cd_desc TYPE TEXT
  USING INITCAP(crm_cd_desc),
  ALTER COLUMN premis_desc TYPE TEXT
  USING INITCAP(premis_desc),
  ALTER COLUMN weapon_desc TYPE TEXT
  USING INITCAP(weapon_desc),
  ALTER COLUMN location TYPE TEXT
  USING INITCAP(location),
  ALTER COLUMN cross_street TYPE TEXT
  USING INITCAP(cross_street);
  ```



#### Victim's Descent Table

- Victim's descent table creation

```sql
CREATE TABLE victim_ethnicity (
	descent_cd CHAR(1),
	descent_desc VARCHAR
	)
	
INSERT INTO victim_ethnicity(
	descent_cd, descent_desc)
	VALUES ('A', 'Other Asian'),
			('B', 'Black'),
			('C', 'Chinese'),
			('D', 'Cambodian'),
			('F', 'Filipino'),
			('G', 'Guamanian'),
			('H', 'Hispanic/Latin/Mexican'),
			('I', 'American Indian/Alaskan Native'),
			('J', 'Japanese'),
			('K', 'Korean'),
			('L', 'Laotian'),
			('O', 'Other'),
			('P', 'Pacific Islander'),
			('S', 'Samoan'),
			('U', 'Hawaiian'),
			('V', 'Vietnamese'),
			('W', 'White'),
			('X', 'Unknown'),
			('Z', 'Asian Indian');
```

#### Crime Category Table

- Crime category table creation

```sql
CREATE TABLE crime_group (
		crm_cd SMALLINT PRIMARY KEY NOT NULL,
		crm_cd_desc	TEXT,
		crime_grouped TEXT
	);

```

- Data importation

```sql
COPY crime_group
FROM 'C:\Users\lenovo\Desktop\PostgreSQL_Files\LA Crime Analysis\LA Crime Grouping.csv'
HEADER DELIMITER ',' CSV;

```

- Altering crime description column
  
  ```sql
  ALTER TABLE crime_group
  ALTER COLUMN crm_cd_desc TYPE TEXT
  USING INITCAP(crm_cd_desc);
  ```

- Updating crime category
  
  ```sql
  UPDATE crime_group
  SET crime_grouped='Sex-Related Offenses'
  WHERE crm_cd_desc='Oral Copulation'
  	OR crm_cd_desc='Sodomy/Sexual Contact B/W Penis Of One Pers To Anus Oth'
  	OR crm_cd_desc='Indecent Exposure'
  	OR crm_cd_desc='Letters, Lewd  -  Telephone Calls, Lewd';
  	
  UPDATE crime_group
  SET crime_grouped='Sex-Related Offenses'
  WHERE crm_cd_desc='Incest (Sexual Acts Between Blood Relatives)';
  
  
  UPDATE crime_group
  SET crime_grouped='Disturbance'
  WHERE crm_cd_desc='Disrupt School';
  
  UPDATE crime_group
  SET crime_grouped='Other'
  WHERE crm_cd_desc='False Imprisonment';
  
  ```

- Column name change in crime_group table
  
  ```sql
  ALTER TABLE crime_group
  RENAME COLUMN crime_grouped TO crime_category;
  ```


