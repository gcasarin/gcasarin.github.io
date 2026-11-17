---
layout: project
title: "Global Sustainable Development: Comparative SQL Analysis"
date: 2026-06-10
excerpt: "A multi-dataset PostgreSQL exploratory and comparative analysis examining global economic, demographic, environmental, and migration trends to support sustainable development policies."
image: https://raw.githubusercontent.com/gcasarin/SQL-Project-Global-sustainable-development/main/cover-image.jpg
link: https://github.com/gcasarin/SQL-Project-Global-sustainable-development
badges:
  - text: "PostgreSQL"
    bg: "#336791"
    color: "#ffffff"
  - text: "Data Analysis"
    bg: "#00cec9"
    color: "#ffffff"
  - text: "Excel Power Query"
    bg: "#217346"
    color: "#ffffff"
---

## Project Overview
This project simulates a data analysis engagement for a non-profit organization dedicated to promoting global sustainable development. By integrating and querying multiple international datasets in **PostgreSQL**, the analysis uncovers cross-country patterns across economic, demographic, environmental, and humanitarian indicators to support data-driven policy recommendations.

The primary objectives of this study are:
1. **Database Architecture & ETL:** Clean raw multi-source datasets using Excel Power Query and structure a normalized relational schema in PostgreSQL with primary/foreign key constraints.
2. **Comparative Exploratory Analysis:** Leverage advanced SQL techniques (CTEs, Window Functions, Views, and Joins) to answer 13 targeted analytical questions covering CO₂ emissions, energy transition, demographic aging, healthcare access, and global migration routes.

🔗 **[View Full SQL Script on GitHub](https://github.com/gcasarin/SQL-Project-Global-sustainable-development/blob/main/SQL_Project-Global_Sustainable_Developement.sql)**
{: .blue-infobox}

---

## Tools & SQL Techniques

| Category | Technology / Method |
| :--- | :--- |
| **Database Engine** | PostgreSQL |
| **ETL & Data Cleaning** | Microsoft Excel (Power Query), SQL `UPDATE` & Standardization |
| **Schema Design** | Normalized Relational Tables, Primary & Foreign Keys |
| **Advanced SQL** | Common Table Expressions (CTEs), Window Functions (`ROW_NUMBER`, `RANK`, `LAG`, Rolling `SUM`), Views, Subqueries, Multi-table Joins |

---

## Data Sources & Database Modeling

The analysis integrates three distinct datasets covering 2023 snapshots alongside historical time series (2000–2023):
- **Global Country Information Dataset (2023):** Split into 5 normalized macro-thematic tables (`general_info_2023`, `healthcare_2023`, `employment_2023`, `economy_2023`, `demographics_2023`) linked via `country` foreign keys.
- **Global Data on Sustainable Energy (2000–2020):** Structured across 3 longitudinal tables (`energy_general_info_00_20`, `energy_sources_00_20`, `energy_renewable_00_20`) tracking electricity generation mix, energy intensity, and GDP growth.
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

The SQL script addresses 13 core investigative questions grouped into three analytical pillars. Below is a summary of the key methodologies and query implementations:

<div style="max-width: 800px; margin: 25px auto; overflow-x: auto;">
  <table class="table table-bordered table-striped" style="font-size: 0.95rem;">
    <thead>
      <tr>
        <th>Analytical Pillar</th>
        <th>Focus Area (Questions)</th>
        <th>Primary SQL Techniques</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td><strong>Environment & Energy</strong></td>
        <td>CO₂ emissions share, agricultural land impact, fossil vs. clean energy ratio, GDP vs. energy growth (Q1–Q7)</td>
        <td>CTEs, <code>ROW_NUMBER()</code>, <code>LAG()</code>, Views, Left/Inner Joins</td>
      </tr>
      <tr>
        <td><strong>Economy & Demographics</strong></td>
        <td>Generational replacement rates, population aging, income tiers vs. education & healthcare access (Q8–Q12)</td>
        <td>Custom Views, <code>CASE WHEN</code> segmentation, <code>PARTITION BY</code></td>
      </tr>
      <tr>
        <td><strong>Humanitarian & Migration</strong></td>
        <td>Casualties by region, demographic breakdown, seasonal peaks, cumulative rolling totals (Q13.1–Q13.5)</td>
        <td>Rolling <code>SUM() OVER</code>, <code>RANK()</code>, Date transformation</td>
      </tr>
    </tbody>
  </table>
</div>

### 1. Environmental Impact: Clean vs. Fossil Energy (Q3)
To evaluate how energy generation mixes affect emissions over a 10-year window (2010–2020), a CTE was combined with window ranking functions to compute a custom **Clean-to-Fossil Energy Ratio**:

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
       ROW_NUMBER() OVER(ORDER BY avg_co2_emiss_kt DESC) AS rank_co2_emiss
FROM avg_energy_co2
WHERE avg_electricity_from_fuels_twh IS NOT NULL
ORDER BY rank_co2_emiss
LIMIT 15;
```

**Key Finding:** China, the USA, India, Russia, and Japan accounted for **over 60% of global CO₂ emissions** in 2023. Furthermore, 10 of the top 15 CO₂ emitters also rank in the top 15 for both fossil fuel electricity generation and agricultural land extension.
{: .blue-infobox}

### 2. Demographic Aging: Developed vs. Developing Nations (Q10)
By creating reusable database views (`nations_fertility_overview`), countries were segmented by GDP per capita ($15,000 threshold) and evaluated against the demographic replacement fertility rate (2.1 children per woman):

```sql
CREATE VIEW nations_fertility_overview AS (
    WITH fertility_gdp AS (
        SELECT d.country, d.fertility_rate, e.gdp / d.population AS gdp_pro_capite,
               CASE WHEN fertility_rate > 2.1 THEN 'Generational Replacement Guaranteed'
                    ELSE 'Aging Population'
               END AS demographic_situation,
               CASE WHEN e.gdp / d.population > 15000 THEN 'Developed Country'
                    ELSE 'Developing Country'
               END AS nation_status
        FROM demographics_2023 d
        JOIN economy_2023 e ON e.country = d.country
    )
    SELECT nation_status, demographic_situation, COUNT(demographic_situation) AS nation_count
    FROM fertility_gdp
    GROUP BY demographic_situation, nation_status
);

SELECT *, ROUND(nation_count / SUM(nation_count) OVER(PARTITION BY nation_status), 2) AS nation_percentage
FROM nations_fertility_overview;
```

**Key Finding:** A stark demographic inversion emerged: **over 85% of developed nations** (49 out of 56) face an aging population below the replacement threshold, whereas **nearly 70% of developing nations** maintain generational replacement.
{: .blue-infobox}

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

## Limitations & Data Considerations
- **Missing Longitudinal Values:** Several records in the *Global Data on Sustainable Energy* dataset contained `NULL` entries for specific country-year combinations, requiring targeted filtering (`IS NOT NULL`) and outlier exclusion (e.g., Equatorial Guinea's 70% anomaly in per-capita energy consumption growth).
- **Temporal Alignment:** Cross-dataset estimations (such as 2020 total energy consumption in Q6) relied on 2023 population figures (`demographics_2023`), introducing a minor temporal offset.

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