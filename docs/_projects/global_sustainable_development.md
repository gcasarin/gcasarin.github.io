---
layout: project
title: "Global Trends: Economic, Energy & Migration Insights"
date: 2026-06-10
excerpt: "A multi-dataset PostgreSQL exploratory and comparative analysis examining global economic, demographic, environmental, and migration trends to support sustainable development policies."
image: https://raw.githubusercontent.com/gcasarin/SQLProject-Global-sustainable-development/main/Charts/global_trend_cover.jpg
link: https://github.com/gcasarin/SQLProject-Global-sustainable-development
badges:
  - text: "Data Analysis"
    bg: "#f20266"
    color: "#ffffff"
  - text: "SQL"
    bg: "#0270c7"
    color: "#ffffff"
  - text: "Excel"
    bg: "#02a149"
    color: "#ffffff"
---

## Overview
This project simulates a data analysis engagement for a non-profit organization dedicated to promoting global sustainable development. By integrating and querying multiple international datasets in **PostgreSQL**, the analysis uncovers cross-country patterns across economic, demographic, environmental, and humanitarian indicators to support data-driven policy recommendations.

The primary objectives of this study are:
1. **Database Architecture & ETL:** Clean raw multi-source datasets using Excel Power Query and structure a normalized relational schema in PostgreSQL with primary/foreign key constraints.
2. **Exploratory Data Analysis (EDA):** Leverage advanced SQL techniques (CTEs, Window Functions, Views, and Joins) to answer 13 targeted analytical questions covering CO₂ emissions, energy transition, demographic aging, healthcare access, and global migration routes.

🔗 **[View Full SQL Script on GitHub](https://github.com/gcasarin/SQLProject-Global-sustainable-development/blob/main/SQL_Project-Global_Sustainable_Developement.sql)**
{: .blue-infobox}

---

## Tools & SQL Techniques

| Category | Technology / Method |
| :--- | :--- |
| **Database Engine** | PostgreSQL |
| **ETL & Data Cleaning** | Excel - Power Query, SQL |
| **Schema Design** | Normalized Relational Tables, Primary & Foreign Keys |
| **Advanced SQL** | Common Table Expressions (CTEs), Window Functions, Views, Subqueries, Multi-table Joins |

---

## Data Sources & Database Modeling

The analysis integrates three distinct datasets covering 2023 snapshots alongside historical time series (2000–2023):
- **Global Country Information Dataset (2023):** Split into 5 normalized macro-thematic tables (`general_info_2023`, `healthcare_2023`, `employment_2023`, `economy_2023`, `demographics_2023`) linked via *`country`* foreign keys.
- **Global Data on Sustainable Energy (2000–2020):** Structured across 3 tables (`energy_general_info_00_20`, `energy_sources_00_20`, `energy_renewable_00_20`) tracking electricity generation mix, energy intensity, and GDP growth.
- **Global Missing Migrants Dataset (2014–2023):** Stored in `migration_incidents_14_23`, detailing casualties, demographic breakdowns, and incident locations.

```sql
-- Example of relational schema definition & constraint enforcement
CREATE TABLE healthcare_2023 (
    country VARCHAR(255) PRIMARY KEY,
    infant_mortality DECIMAL(5, 2),
    life_expectancy DECIMAL(5, 2),
    maternal_mortality_ratio DECIMAL(10, 2),
    out_of_pocket_health_expenditure DECIMAL(5, 2),
    physicians_per_thousand DECIMAL(5, 2),
    CONSTRAINT fk_healthcare FOREIGN KEY (country) REFERENCES general_info_2023(country)
);
```

---

## Exploratory Data Analysis & Key Queries

The SQL script tackles 13 core investigative questions categorized into three main areas (summarized below), with each query engineered to extract meaningful insights on global sustainability.

<div style="max-width: 800px; margin: 25px auto; overflow-x: auto;">
  <table class="table table-bordered table-striped" style="font-size: 0.95rem;">
    <thead>
      <tr>
        <th>Main Area</th>
        <th>Focus Topics</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td><strong>Environment & Energy</strong></td>
        <td>CO₂ emissions share, agricultural land impact, fossil vs. clean energy ratio, GDP vs. energy growth (Q1–Q7)</td>
      </tr>
      <tr>
        <td><strong>Economy & Demographics</strong></td>
        <td>Generational replacement rates, population aging, income tiers vs. education & healthcare access (Q8–Q12)</td>
      </tr>
      <tr>
        <td><strong>Humanitarian & Migration</strong></td>
        <td>Casualties by region, demographic breakdown, seasonal peaks, cumulative rolling totals (Q13.1–Q13.5)</td>
      </tr>
    </tbody>
  </table>
</div>

The most insightful queries from each category are detailed below. For a deeper dive, please refer to the [Full SQL Script](https://github.com/gcasarin/SQLProject-Global-sustainable-development/blob/main/SQL_Project-Global_Sustainable_Developement.sql).

### 1.  Environment & Energy


- **Top 10 Global CO₂ Emitters in 2023**
  
  To identify the primary contributors to global emissions, a Common Table Expression (CTE) was combined with a subquery to calculate each country's percentage share of total        worldwide CO₂ emissions in 2023:   

  ```sql
  WITH co2 (country, co2_emissions, tot_co2) AS (
    SELECT country, 
           co2_emissions,
           (SELECT SUM(co2_emissions) FROM general_info_2023)
    FROM general_info_2023
  )
  SELECT country, 
       co2_emissions,
       ROUND((co2_emissions / tot_co2) * 100, 2) AS world_co2_percent
  FROM co2
  WHERE (co2_emissions / tot_co2) * 100 IS NOT NULL
  ORDER BY world_co2_percent DESC
  LIMIT 10;
  ```
  <p align="center">
    <img src="https://raw.githubusercontent.com/gcasarin/SQLProject-Global-sustainable-development/main/Charts/Q1.png" width="95%" style="border-radius: 4px;" alt="Grafico Q1" />
  </p>

  Key Results: China, US, India, Russia, and Japan emerged as the top five emitters in 2023, collectively accounting for over 60% of global CO₂ emissions.
  {: .blue-infobox}   

- **Fossil vs. Clean Energy Impact on CO₂ Emissions**
  
  To assess how national energy mixes influenced emissions over a 10-year window (2010–2020), a CTE was created with Window Ranking Functions (*ROW_NUMBER*) to compute a custom      Clean-to-Fossil Energy Ratio:   

  ```sql
  WITH avg_energy_co2 AS (
    SELECT country, 
           ROUND(AVG(electricity_from_fossil_fuels_twh), 5) AS avg_electricity_from_fuels_twh,
           ROUND(AVG(electricity_from_renewables_twh + electricity_from_nuclear_twh), 5) AS avg_clean_electricity_twh,
           ROUND(AVG(co2_emissions_kt), 5) AS avg_co2_emiss_kt
    FROM energy_sources_00_20
    WHERE year BETWEEN 2010 AND 2020
    GROUP BY country
    HAVING AVG(co2_emissions_kt) IS NOT NULL
  )
  SELECT *,
       CASE WHEN avg_electricity_from_fuels_twh = 0 THEN NULL
            ELSE ROUND(avg_clean_electricity_twh / avg_electricity_from_fuels_twh, 2)
       END AS clean_fuel_energy_ratio,
       ROW_NUMBER() OVER(ORDER BY avg_electricity_from_fuels_twh DESC) AS rank_fuel_electr,
       ROW_NUMBER() OVER(ORDER BY avg_clean_electricity_twh DESC) AS rank_clean_electr,
       ROW_NUMBER() OVER(ORDER BY avg_co2_emiss_kt DESC) AS rank_co2_emiss
  FROM avg_energy_co2
  WHERE avg_co2_emiss_kt IS NOT NULL AND avg_electricity_from_fuels_twh IS NOT NULL
  ORDER BY rank_co2_emiss
  LIMIT 15;
  ```

  <p align="center">
    <img src="https://raw.githubusercontent.com/gcasarin/SQLProject-Global-sustainable-development/main/Charts/Q3.png" width="95%" style="border-radius: 4px;" alt="Grafico Q3" />
  </p>
  
  Key Results: Among the top 15 CO₂-emitting nations, 10 also rank in the global top 15 for fossil-fuel electricity generation, confirming a strong correlation between fossil        energy reliance and emissions. The *clean_fuel_energy_ratio* metric effectively isolates the most virtuous countries transitioning toward nuclear and renewable sources.
  {: .blue-infobox}


### 2. Economy & Demographics

- **Economic Tiers vs. Education & Minimum Wage**

  To analyze how education levels and wages vary across income categories, a dedicated view (`economic_status`) classifies countries by per-capita GDP before joining employment      indicators:

  ```sql
  CREATE VIEW economic_status (country, economy) AS (
      SELECT ec.country,
             CASE 
                 WHEN gdp / population > 30000 THEN 'High-Income Country'                
                 WHEN gdp / population > 10000 AND gdp / population <= 30000 THEN 'Middle-Income Country'
                 ELSE 'Low-Income Country'
             END  
      FROM economy_2023 ec
      JOIN demographics_2023 d ON ec.country = d.country
  );

  SELECT economy,
         ROUND(AVG(gross_tertiary_education_enrollment)) AS "avg_tertiary_education_enrollment %", 
         ROUND(AVG(minimum_wage), 2) AS "avg_minimum wage $/h"
  FROM economic_status es
  JOIN employment_2023 em ON es.country = em.country
  GROUP BY economy
  ORDER BY economy;
  ```

  <p align="center">
    <img src="https://raw.githubusercontent.com/gcasarin/SQLProject-Global-sustainable-development/main/Charts/Q11.png" width="95%" style="border-radius: 4px;" alt="Grafico Q11" />
  </p>
  
  **Key Finding:** Higher-income countries exhibit significantly higher levels of tertiary education enrollment and higher average hourly minimum wages.
  {: .blue-infobox}

- **Economic Status vs. Healthcare & Life Expectancy**

  Reusing the previously created economic_status view, this query joins healthcare indicators to examine how income tiers influence life expectancy, physician density, and out-of-   pocket medical expenses:

  ```sql
  SELECT es.economy,
       ROUND(AVG(life_expectancy), 2) AS avg_life_expectancy,
       ROUND(AVG(out_of_pocket_health_expenditure), 2) AS avg_out_of_pocket_health_expenditure,
       ROUND(AVG(physicians_per_thousand), 2) AS avg_physician_per_thousand
  FROM economic_status es
  JOIN healthcare_2023 h ON es.country = h.country
  WHERE physicians_per_thousand IS NOT NULL
  GROUP BY economy
  ORDER BY economy;
  ```

  **Key Finding:** More economically developed countries exhibit higher average life expectancy, driven by greater healthcare coverage (public/private) that reduces out-of-pocket        expenses and a higher density of physicians per thousand inhabitants.
  {: .blue-infobox}

  <p align="center">
    <img src="https://raw.githubusercontent.com/gcasarin/SQLProject-Global-sustainable-development/main/Charts/Q12.png" width="95%" style="border-radius: 4px;" alt="Grafico Q12"       />
  </p>

### 3. Global Migration Trends & Rolling Totals (Q13.4 & Q13.5)
Using window functions (`SUM() OVER` and `RANK() OVER`), annual cumulative casualties and peak seasonal months were isolated across the 2014–2023 period:

```sql
WITH monthly_totals AS (
    SELECT incident_year, month_number, SUM(number_of_dead) AS monthly_deaths
    FROM migration_incidents_14_23
    GROUP BY incident_year, month_number
)
SELECT incident_year, 
       month_number AS incident_month,
       monthly_deaths,
       SUM(monthly_deaths) OVER (PARTITION BY incident_year ORDER BY month_number) AS rolling_total_deaths
FROM monthly_totals
ORDER BY incident_year, month_number;
```

---

## Conclusions

This comparative SQL investigation demonstrates how structured relational modeling and advanced analytical querying can transform disparate global datasets into clear policy intelligence. On the environmental front, **fossil fuel reliance and agricultural land scale** remain the primary drivers of national CO₂ emissions; while major economies lead in absolute clean energy output, their transition pace has yet to offset fossil-driven emissions.

Socio-economically, higher GDP per capita correlates strongly with **longer life expectancy, lower out-of-pocket healthcare burdens, higher tertiary education enrollment, and higher minimum wages**, yet it is accompanied by acute **demographic aging** below the 2.1 fertility replacement threshold. Finally, humanitarian data underscores persistent migration risks concentrated heavily in the **Mediterranean Sea**, with fatalities averaging ~3,000 per year and peaking seasonally between **April and September**.

---

## Links & Resources

> <span style="font-size: 0.98rem; line-height: 1.8;">
> ▶️ **GitHub Repository:** [Project Repo](https://github.com/gcasarin/SQL-Project-Global-sustainable-development)<br>
> 📄 **SQL Script:** [Direct Link to PostgreSQL Code](https://github.com/gcasarin/SQL-Project-Global-sustainable-development/blob/main/SQL_Project-Global_Sustainable_Developement.sql)<br>
> 📌 **Output Results:** [Query Charts & Tables Folder](https://github.com/gcasarin/SQL-Project-Global-sustainable-development)
> </span>
