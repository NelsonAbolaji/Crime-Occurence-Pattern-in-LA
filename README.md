# Investigating Crime in Los Angeles
![alt text](https://github.com/NelsonAbolaji/LA-Crime-Analysis/blob/main/Los%20Angeles%20Sunset.jfif)

## Insights

- As at 23rd of October 2023, 825,212 crimes have been reported to the LAPD, with 212,018 cases recorded each year on average and 592 cases per day.
- Crime occurs more on weekdays than weekends. Mondays have the highest crime rate and the intensity slowly reduces till the days near the weekend, where there's a significant reduction in crime rate.
- 6.78% of the total crime committed occured at noon (12 p.m.). This makes it the time with the highest crime rate.
- 12 p.m. and 4 p.m. to 8 p.m. are the hours where criminal activities are most intense while 2 a.m. to 6 a.m. have the lowest criminal activities.
- Vehicle theft is the most committed crime and it accounts for 10.71% of the total crime records. Battery-Simple Assault and Theft of identity are the 2nd 
  and 3rd most committed crimes respectively.
- Central is the area with the highest crime rate, with 77th Street and Pacific having the 2nd and 3rd highest crime rate respectively.
- People aged 30-34, closely followed by those aged 25-29 are more likely than other age groups to be victims of crime, especially assault with deadly weapons
  (*There are 203,879 records with missing or incorrect age values which may affect the result concerning crime committed to each age group, especially for 'Vehicle-Stolen' which has the most implausible records -89,250 records with the age 0).
  
- Battery involving Simple Assault is the highest crime men are victims of and it accounts for 10.6% of the total crime involving men, while Simple Assault by Intimate Partner is the highest crime women are victims of and it accounts for 10.9% of the total crime involving women.
  
- People of Hispanic/Latin/Mexican, White and Black descent are the top 3 victims of crime. The top crime type Hispanic/Latin/Mexican and Black people are victim
  of is Violent crime, Battery-Simple Assault to be precise while for the White demographic, it is Property crime, especially Burglary From Vehicle.
  
- 28.8% of the crimes were reported to have happened in the street and 19.3% in Single Family Dwelling. This makes them the top two premises where crime occur.
  
- Strong arm (Hands, Fist, Feet Or Bodily Force) is the top weapon used by assailants and it was reported as the used weapon in 57.3% of crimes where the weapon used is known. It is mostly used against females and it is commonly used in Battery-Simple Assault cases.
  
- LAPD solves an average of 21.9% of the crime they record per year. Since 2020, the Police Department have closed 20.0% (164,724) cases, leaving 80.0% of the total crimes under investigation.


### Los Angeles Crime Report

*This report uses data collected from the 1st of January, 2020 to 23rd of October, 2023.*

LAPD has received a report of 825,212 crime cases as at 23rd of October, 2023. The lowest cases in a year so far (192,703) was recorded in 2020 (excluding 2023 which is still incomplete) and the low rate may have been due to the pandemic. The highest crime rate was recorded in 2022 (235,069 cases).

**At 60.9% of the total crime recorded, Property crimes are the most committed offenses, followed by Violent crimes which makes up 30.1% of the total crime recorded**

There are 138 distinct crime descriptions that the LAPD employs to briefly explain reports made by victims. These 138 crime descriptions have been grouped into 8 crime categories, 4 categories according to criminologists (Violent, Property, White-Collar, Organized) while the remaining groups were created to accomodate crime descriptions that do not fit the aforemetioned categories. Some of the crime descriptions were grouped by discretion and it may be necessary to check the category certain crime descriptions were grouped into.

Property crime (theft, burglary) make up 60.9% of the total reported crimes while Violent crime account for 30.1%. Vehicle-Stolen is the top Property crime reported by LA inhabitants while Battery-Simple Assault is the top Violent crime. 88,355 vehicles have been stolen since 2020, making Vehicle-Stolen the most committed crime (10.7% of total crime), followed by Battery-Simple Assault (8.0%) and Theft of Identity (6.3%).
  
- An average of 63 cases of stolen vehicles, 47 cases of Battery-Simple Assault, 36 buglaries from vehicles and 36 regular buglaries are reported to the LAPD each day.
  
- Fatal crimes (Criminal Homicide, Negligient Manslaugther, Lynching) make up 0.2% of the total crimes committed. On average, 1 person is killed each day in LA.
  
- Hollenbeck has the 2nd lowest crime rate but it is 3rd in the rank of top areas where fatal crimes occur.
  
- The highest victims of Criminal Homicide are Hispanic/Latino/Mexican and Black people.

  **Crime occurence is time-dependent**

There is no clear trend in the way crime occurs by month, but by day, crimes are more likely to occur on weekdays (74.4%) than on weekends (25.6%). The probability of crime occuring during the week is highest on Monday, and this probability gradually reduces until Saturday, where the decline in crime rate is marked. The trend in crime rate by day is dependent on the flow of the population during those days. 203,349 (24.6% of total cases) cases occured in the afternoon (12p.m. - 5p.m.) on weekdays, and this accounts for highest crime rate (33.1%) on weekdays. The rate at which property crimes are committed also increases during these hours. On weekends however, the crime rate is highest at night(9p.m. - 4a.m.) and this time period accounts for 31.8% of crime that occurs on weekends (67,055 of total crime). While Property crimes are the highest crimes that occur on weekends, Violent crimes and Sex-Related crimes see an increase in occurence that is significantly different from the intensity during other hours.

Table 1: Crime rate at each period of the day

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
| period_of_the_day | weekday_crime_count | weekday_percent_of_total | weekend_crime_count | weekend_percent_of_total |
| morning | 142,451 | 17.30% | 43,601 | 5.30% |
| afternoon | 203,349 | 24.60% | 64,285 | 7.80% |
| evening | 103,914 | 12.60% | 36,137 | 4.40% |
| night | 164,420 | 19.90% | 67,055 | 8.10% |

**Crime by Sex**

Males have been victims of crime(41.2%) while Females have been victims of 303,878 crimes (36.8%). The rest of the victim's sex values are either missing or it wasn't supplied. Battery-Simple Assault (10.6%) and Aggravated Assault (10.4%) are the top 2 crimes males are victims of while Intimate Partner-Simple Assault(10.9%) and Battery-Simple Assault (10.7%) are the top crimes females are victims of. There are certain crimes that plaque a gender considerably more than the other, such as sex-related offenses. The crime descriptions have been grouped in an effort to reduce granularity and the crime categories can be seen in the table below.

Table 2: Percentage of crime each gender is a victims of (This uses data with recorded gender)

|     |     |     |     |     |     |     |     |     |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Victim's<br> Gender | Property | Violent | Other | Sex-Related Offenses | Disturbance | White Collar | Crime against Children | Organized |
| M   | 29.7% | 19.6% | 1.4% | 0.6% | 0.8% | 0.5% | 0.0% | 0.0% |
| F   | 23.2% | 17.2% | 2.7% | 2.6% | 0.6% | 0.6% | 0.2% | 0.0% |

There is a need to present details about the 'Other' category as the crime descriptions that make it up cannot be guessed. The top 3 crime descriptions that make it up are Violation Of Restraining Order, Letters, Lewd - Telephone Calls, Lewd and Violation Of Court Order. The total of the crime Females were victims of in this section is 21,632 while for Males, the value is 11,252. Females are the top victims of the top three crimes and the percentage sum of these three crimes totals to 2.5% of the total crime in the Other category.

**Adults are the highest victims of crimes**

The tendency of being a victim of a criminal occurence is largely dependent on how young or old one is. People who are very old (elderlies aged 95-99 years have reported 594 cases) or very young (children aged 0-4 years have 1,829 reported cases) are least likely to be victims of crime. As the age groups proceed to the age of 30-34, so does the tendecy of being a victim of crime. The crime rate sharply rises from 2.9% (23,894 cases) for people aged 15-19 to 7.9% (65,148 cases) for people 20-24 years old. People aged 30-34 (87,629 cases and 10.6% of total cases) are the highest demographic who are victims of crime and the crime rate drops as it moves towards groups of older ages.

*insert two images, age distribution and crime distribution by age*

**Crime occurence by Victim's Descent**

Hispanic/Latino/Mexican are the highest victims of crime, followed by LA's White and Black inhabitants. People of Hispanic/Latino/Mexican descent have been victims of 30.7% (253,152) of total crimes, with White victims behind at 20.4% (168,138) while Black people account for 14.2% (117,571) of total crime. These three descents are altogether victims of 65.3% of total criminal activities. With 13.2% of the records having a null descent value and 9.6% an unknown value, the rest of the descents are victims of 11.9% crimes.

Hispanic/Latino/Mexican females reported 0.2% crimes more than the males. It is also worth reporting they are victims of 44.4% of the total Sex-related offenses. Hispanic/Latino/Mexican have seen 162 people falsely imprisoned, followed by Black people with 57 people falsely imprisoned and White people with 51 people falsely imprisoned. White people are the highest victims (111,993 cases and 33.4% of total Property crime cases) of Property crime cases, with the most common Propety crime being Burglary from Vehicle.

