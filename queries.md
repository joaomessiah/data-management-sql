## SQL Queries

1. **Sites in the Late Roman Period**

  This query retrieves all sites that date to the Late Roman Period, including their start and end periods, and adds the corresponding period explanations from the reference table.
  
```sql
SELECT 
    Sites.*, 
    Periods2.Name AS Start_period_name, 
    Periods2.Date_range AS Start_period_date_range, 
    Periods1.Name AS End_period_name, 
    Periods1.Date_range AS End_period_date_range
FROM 
    (Sites 
    LEFT JOIN Periods AS Periods1 
    ON Sites.End_period = Periods1.Period_ID) 
    LEFT JOIN Periods AS Periods2 
    ON Sites.Start_period = Periods2.Period_ID
WHERE 
    (
        (Sites.Start_period = "ROML" OR 
         Sites.Start_period = "ROMLA" OR 
         Sites.Start_period = "ROMLB")
    ) 
    OR 
    (
        Sites.End_period = "ROML" OR 
        Sites.End_period = "ROMLA" OR 
        Sites.End_period = "ROMLB"
    );
```

2. **Settlement Count by Site Type and Place**

  This query calculates the number of settlements grouped by site type and place, providing insights into the distribution of settlement types across different locations.
  
```sql
SELECT 
    Sites.Place, 
    Sites.Site_Type, 
    Count("Site_ID") AS Number_of_Settlements
FROM 
    Sites
GROUP BY 
    Sites.Place, 
    Sites.Site_Type
ORDER BY 
    Sites.Place, 
    Sites.Site_Type;
```

3. **Literature References Between 1980-1989 for Each Site**

  This query returns the number of literature references published between 1980 and 1989 for each archaeological site, linking sites to their corresponding literature references.
  
```sql
SELECT 
    Sites.Site_ID, 
    Sites.Place, 
    Sites.Start_period, 
    Sites.End_period, 
    Count(Sites_Literatures.Site_ID) AS Number_of_Literatures_between_1980_1989
FROM 
    (Sites_Literatures 
    INNER JOIN Literatures 
    ON Sites_Literatures.literature_ID = Literatures.literature_ID) 
    INNER JOIN Sites 
    ON Sites_Literatures.Site_ID = Sites.Site_ID
WHERE 
    Literatures.Pub_year BETWEEN 1980 AND 1989
GROUP BY 
    Sites.Site_ID, 
    Sites.Place, 
    Sites.Start_period, 
    Sites.End_period
ORDER BY 
    Sites.Site_ID;
```
