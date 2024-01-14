# Investigating Crime in Los Angeles
![alt text](https://github.com/NelsonAbolaji/LA-Crime-Analysis/blob/main/Los%20Angeles%20Sunset.jfif)

### Insights

- As at 23rd of October 2023, 825,212 crimes have been reported to the LAPD, with 212,018 cases recorded each year on average and 592 cases per day.

- Vehicle theft is the most committed crime and it accounts for 10.71% of the total crime record. Battery-Simple Assault and Theft of identity are the 2nd 
  and 3rd most committed crimes respectively.
  
- Crime occur more on weekdays than weekends. Mondays have the highest crime rate and the intensity slowly reduces till the days near the weekend where there's a significant reduction in crime rate.
  
- Noon (12 p.m.) is the time of the day with the highest crime rate (6.78%).
  
- 12 p.m. and 4 p.m. to 8 p.m. are the hours where criminal activities are most intense while 2 a.m. to 6 a.m. have the lowest criminal activities.
  
- People aged 30-34, closely followed by those aged 25-29 are more likely than other age groups to be victims of crime, especially assault with deadly weapons.
  
- Battery-Simple Assault is the highest crime men are victim of and it accounts for 10.6% of the total crime involving men, while Simple Assault by Intimate Partner is the highest crime women are victim of and it accounts for 10.9% of the total crime involving women.
  
- People of Hispanic/Latin/Mexican, White and Black descent are the top 3 victims of crime. The top crime Hispanic/Latin/Mexican and Black people are victim
  of is Violent crime, Battery-Simple Assault to be precise while for the White demographic, it is Property crime, especially Burglary From Vehicle.

- Central is the area with the highest crime rate, with 77th Street and Pacific having the 2nd and 3rd highest crime rate respectively.
 
- 28.8% of the crimes were reported to have happened in the street and 19.3% in Single Family Dwelling. This makes them the top two premises where crime occur.
  
- Strong arm (Hands, Fist, Feet Or Bodily Force) is the top weapon used by assailants and it was reported as the used weapon in 53.6% of crimes where the weapon used is known. It is mostly used against females and it is commonly used in Battery-Simple Assault cases (Weapon types commonly used may inform the budget for weapon procurement).
  
- LAPD solves an average of 21.6% of the crime they record per year. Since 2020, the Police Department have closed 19.96% (164,724) cases, leaving 80.04% of the total crimes under investigation. This analysis aims to prevent crime occurences and assist surveillance efforts.

### Policy Recommendations
The result of the analysis has revealed trends in the way crime occur in LA and the implementation of these policies may help police LA better:
1. Patrol policing has been observed to moderately prevent crime and effectively reduce its intensity in [high crime areas](https://www.tandfonline.com/doi/abs/10.1080/07418829500096221). The deployment of human resources responsible for patrolling should thus be done with the consideration of the day, time and flow of the population. More patrol officers should be deployed during the weekdays, and their number should be proportional to the intensity of crime in each area and the average rate of crime on those days. On weekdays, patrol officers should be deployed more in the afternoon while on weekends, an higher number of the officers deployed should be on-duty at night.
2. The ethnicities that make up an area's population is informative of the rate and type of crime that is prone to happen in it. Areas with a considerable population of Hispanic/Latin/Mexican people should receive more coverage from patrol officers as this demographic is more probable to be a victim of crime, especially Violent crimes. Areas with a population of White people are more susceptible to Property crimes and patrolling may help deter criminals or promptly apprehend them.
3. Surveillance systems have been [proven](https://www.ojp.gov/ncjrs/virtual-library/abstracts/police-surveillance-and-emergence-cctv-1960s) to deter crime, especially property crimes. An investment of the LAPD budget to improving surveillance coverage will be useful in preventing and investigating crimes that happen in public places. While this will reduce the cost of labour involved in investigating Property crimes, human personnel still have a role to play.


### SQL Queries
This project was done using PostgreSQL and Tableau. To go through the SQL statements and queries used to perform the analysis, click the link below:
- [Table Creation Statements](https://github.com/NelsonAbolaji/LA-Crime-Analysis/blob/main/LA%20Crime%20Table%20Creation%20Statement.md)
- [Exploratory and Descriptive Analysis Queries](https://github.com/NelsonAbolaji/LA-Crime-Analysis/blob/main/LA%20Crime%20Descriptive%20Data%20Analysis.md)

### Los Angeles Crime Report

*This report uses data collected from the 1st of January, 2020 to 23rd of October, 2023. An initial capital letter convention is used in the report to lay emphasis on important phrases.*

*The 138 crime descriptions the LAPD employs to summarize offenses reported by victims have been grouped into 8 categories to reduce granularity. The Crime against Children category contains crimes committed against children with no detail of the particular offense. Other crimes children were victims of have been placed in the appropriate category.*

*To check the category each crime description is grouped into, click here:* [LA Crime Description Grouped](https://github.com/NelsonAbolaji/LA-Crime-Analysis/blob/main/LA%20Crime%20Category.csv)

LAPD has received a report of 825,212 crime cases as at 23rd of October, 2023. The lowest cases in a year so far (192,703) was recorded in 2020 (excluding 2023 which is still incomplete) and the low crime rate may have been due to the pandemic. The highest amount of cases in a year was recorded in 2022 (235,069 cases).

##### Property offense is the most committed crime

**Property crimes** (e.g. theft, burglary) **make up 60.9% of total reported crime** while Violent crimes (e.g. assault, manslaughter) account for 30.1%. Vehicle-Stolen is the top Property crime reported by LA inhabitants while Battery-Simple Assault is the top Violent crime. Fig. 1 shows that since 2020, 88,355 vehicles have been stolen making **Vehicle-Stolen the most committed crime** (10.7% of total crime), followed by Battery-Simple Assault (8.0%) and Theft of Identity (6.3%). An average of 63 cases of stolen vehicles, 47 cases of battery-simple assault, 36 cases of buglary from vehicles and 36 cases of regular buglaries are reported to the LAPD daily. Strong arm (hands, fist, feet or bodily force) is the most common weapon (53.6%), followed by Verbal Threat (7.3%) and Hand Gun (5.9%).

![An image showing Crime by Crime Category](https://github.com/NelsonAbolaji/LA-Crime-Analysis/blob/main/Markdown%20Images/Crime%20by%20Crime%20Category.png)
Fig. 1: Crime reportage by crime category

Central, 77th Street and Pacific are the areas with the highest daily crime rates. In Fig. 2, areas with an average daily crime rate above the median are coloured red while areas with values below the median are yellow. This shows where crime is most and least intense daily. The occurence of crime of each category is also displayed in the RHS of Fig. 2 below.

![An image showing Crime by Areapng](https://github.com/NelsonAbolaji/LA-Crime-Analysis/blob/main/Markdown%20Images/Crime%20by%20Area.png)
Fig. 2: Daily crime occurence in each Area

Crimes leading to death (Criminal Homicide, Negligient Manslaugther, Lynching) - which have been tagged **Fatal crimes** for this analysis - make up approximately 0.2% of the total crimes committed. On average, one person is **killed** in LA each day. Hispanic/Latino/Mexican and Black people around the age of 20-40 are more likely to be a victim of a fatal crime, precisely Criminal Homicide. The age group of White people who are victims of fatal crime isn't distributed like the others that peak in the middle, as can be inferred from Fig. 3 and Fig. 4 below. White people aged 35-39 are the highest victims of fatal crime (19 cases), followed by those aged 45-49 and 55-59 who recorded 12 cases each.

![Fatal Crimes Dashboard](https://github.com/NelsonAbolaji/LA-Crime-Analysis/blob/main/Markdown%20Images/Fatal%20Crimes%20Dashboard.png)
Fig 3: Fatal crime by victim's age and descent

![Fatal Crimes Dashboard White Descent](https://github.com/NelsonAbolaji/LA-Crime-Analysis/blob/main/Markdown%20Images/Fatal%20Crimes%20Dashboard%20(White%20Descent).png)
Fig. 4: Fatal crime involving victims of White descent

The intensity of fatal crimes does not always match the general criminal activity in each area. The most extreme example is highlighted in Fig. 5, where Hollenbeck has the **2nd lowest** crime rate by area but it is **3rd** in the rank of top areas where fatal crimes occur. Other metrics crime rate and occurence is dependent on such as time and descent are explored further in the report.

![Crime Rate by Fatal Crime Rate](https://github.com/NelsonAbolaji/LA-Crime-Analysis/blob/main/Markdown%20Images/Crime%20Rate%20by%20Fatal%20Crime%20Rate.png)
Fig. 5: A comparison of rank of all crimes with the rank of fatal crimes

##### Crime occurence is dependent on the time and day
There is no clear trend in the way criminal activity occurs by month, but by day, more crimes occur on weekdays than weekends. This isn't an observation influenced by the ratio of days present in the two groups. Crime rate during the week is highest on Monday (15.7%), and the rate gradually reduces until Saturday (12.9%), where the decline in crime rate is marked. In Table 1, average crime occurence on each day of the week can be seen.

Table 1: Average crime occurence on each day of the week

|     |     |     |     |     |     |     |     |
| --- | --- | --- | --- | --- | --- | --- | --- |
|     | Monday | Tuesday | Wednesday | Thursday | Friday | Saturday | Sunday |
| Avg Crime Rate | 652 | 625 | 614 | 603 | 593 | 534 | 526 |

In Fig. 6 below, an increase in the occurence of Violent crime can be seen on Saturday and Sunday. The crimes most responsible for this increase are Intimate Partner-Simple Assault, Intimate Partner-Aggravated Assault and Assault with Deadly Weapon-Aggravated Assault. The rise in occurence of Intimate Partner related cases on the weekend may be because that is when partners are free from work and are both present at home for an extended time.

![Crime Rate on Days of the Week Crime Category](https://github.com/NelsonAbolaji/LA-Crime-Analysis/blob/main/Markdown%20Images/Crime%20Rate%20on%20Days%20of%20the%20Week%20(Crime%20Category).png)
Fig 6: Crime type occurence on each day of the week

##### Crime rate is highest in the afternoon
12p.m. and 4p.m.- 8p.m. respectively are the hours where criminal offenses are most committed. The rate of crime occurence at each time of the day can be seen in the chart on the LHS of Fig. 7. The rate of occurence of crime of each category by time can also be seen in the chart on the RHS of Fig. 7. The hours of the day were grouped into periods. Afternoon (12p.m.- 5p.m.) recorded the highest crime rate (32.4%), while night (9p.m. -4a.m.) recorded 28.1% of total crime.

![Crime Activity by Time](https://github.com/NelsonAbolaji/LA-Crime-Analysis/blob/main/Markdown%20Images/Crime%20Activity%20by%20Time.png)
Fig 7: Crime activity by time and type

Generally, criminal offenses are more likely to occur in the afternoon, but on weekends, the crime rate is highest at night (9p.m. - 4a.m.) and 31.8% of crime that occur on weekends happen then. In Fig. 8, it can be observed that on weekends, Violent crimes see an increase in occurence at night which is significantly different from the intensity during the same period on weekdays. The rate of Sex-Related offenses committed is highest at night on weekends as compared to weekdays where it is highest in the afternoon.

![Weekday vs Weekend Violent and Sex Related Offenses Comparison](https://github.com/NelsonAbolaji/Crime-Occurence-Pattern-in-LA/blob/main/Markdown%20Images/Weekday%20vs%20Weekend%20(Violent%20and%20Sex-Related).png)
Fig. 8: Difference in intensity between criminal activity on weekdays and weekends

##### Crime by Sex
Males have been victims of 41.2% of total reported crime while females have been victims of 36.8% of total reported crime. The rest of the victim's sex values are either missing or not provided. Battery-Simple Assault (10.6%) and Aggravated Assault (10.4%) are the top 2 crime males are victim of while Intimate Partner-Simple Assault (10.9%) and Battery-Simple Assault (10.7%) are the top crimes females are victim of. There are crime types that plague a gender considerably more than the other and should be further looked into, such as sex-related offenses. A summary table of the total crime and the type of crime committed against each gender can be seen below.

Table 2: Number of crime in each category each gender is a victim of

|     |     |     |     |     |     |     |     |     |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Victim's Gender | Property | Violent | Other | Sex-Related Offenses | Disturbance | White Collar | Crime against Children | Organized |
| Male | 191,726 | 126,599 | 9,089 | 4,132 | 5,408 | 3,430 | 273 | 7   |
| Female | 149,381 | 111,089 | 17,460 | 16,874 | 3,999 | 3,727 | 1,262 | 86  |

##### Adults are the highest victims of crime
The tendency of being a victim of a criminal occurence is largely dependent on how young or old one is. People who are very old (elderlies aged 95-99 years have reported 0.1% of total cases) or very young (children aged 0-4 years have reported 0.3% of total cases) are least likely to be victims of crime. As the groups proceed to the age of 30-34, so does the tendecy of being a victim of crime. The crime rate sharply rises from 2.9% for people aged 15-19 to 7.9% for people 20-24 years old. People aged 30-34 (10.6% of total cases) are the highest demographic who are victims of crime and the crime rate drops as it moves towards groups of older ages. Crime type is also dependent on age group.

![Crime Rate and Type by Age Group](https://github.com/NelsonAbolaji/LA-Crime-Analysis/blob/main/Markdown%20Images/Age%20Dashboard.png)
Fig. 9: Crime type experienced by victims of each age group

##### Crime occurence by Victim's Descent
Hispanic/Latino/Mexican are the highest victims of crime, followed by LA's White and Black inhabitants. People of Hispanic/Latino/Mexican descent have been victims of 30.7% (253,152) of total crimes, with White victims behind at 20.4% (168,138) while Black people account for 14.2% (117,571) of total crime. These three descents are altogether victims of 65.3% of total criminal activities. With 13.2% of the records having a null descent value and 9.6% an unknown value, the rest of the descents are victims of 11.9% crimes.

![Victim's Dashboard](https://github.com/NelsonAbolaji/LA-Crime-Analysis/blob/main/Markdown%20Images/Victim's%20Descent%20Dashboard.png)
Fig. 10: Crime activity and intensity experienced by each descent

Hispanic/Latino/Mexican females reported 0.2% crime more than the males. It is also worth reporting they are victims of 44.4% of the total Sex-related offenses. White people are the highest victims (111,993 cases and 33.4% of total Property crime cases) of Property crime cases, with the most common Propety crime being Burglary from Vehicle.
