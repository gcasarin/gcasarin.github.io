---
layout: project
title: "Sales Performance and Customer Analytics Dashboards"
date: 2026-06-05
excerpt: "An interactive Business Intelligence solution built in Tableau, featuring dual-perspective dashboards for tracking year-over-year sales metrics and deep-dive customer behaviors."
image: https://raw.githubusercontent.com/GabryGit/Tableau_Project-Sales-and-Customer-Dashboard/main/Charts/Sales_Dash.png
link: https://public.tableau.com/app/profile/gabriele.casarin/viz/Sales_Dashboard_Project_17510126295020/SalesDash
badges:
  - text: "Data Visualization"
    bg: "#00cec9"
    color: "#ffffff"
  - text: "Business Intelligence"
    bg: "#0984e3"
    color: "#ffffff"
---

## Project Overview
This project delivers a comprehensive Business Intelligence solution designed to empower stakeholders, marketing teams, and management with actionable insights. Developed in Tableau, the solution splits analysis across two interconnected dashboards:

1. **Sales Dashboard:** Offers a high-level macro view of financial metrics, tracking year-over-year performance, seasonal trends, and product-level profitability.
2. **Customer Dashboard:** Focuses on micro-level consumer behavior, analyzing customer retention, order distributions, and identifying top-tier profitable relationships.

🔗 **[Explore Dashboards Live on Tableau Public](https://public.tableau.com/app/profile/gabriele.casarin/viz/Sales_Dashboard_Project_17510126295020/SalesDash)**
{: .blue-infobox}

---

## Tools & Techniques

| Category | Technology / Method |
| :--- | :--- |
| **BI Tool** | Tableau |
| **Data Modeling** | Relational Schemas, Joins & Relationships |
| **Core Techniques** | Calculated Fields, Parameters, Dynamic Filters, Action Filters |

---

## Data Sources and Modeling

The dashboards are powered by structured retail transaction datasets. To ensure optimal performance and accurate relationship handling across tables, a relational data model schema was designed directly within Tableau.

<p align="center">
  <img src="https://raw.githubusercontent.com/GabryGit/Tableau_Project-Sales-and-Customer-Dashboard/blob/main/Charts/Datamodel-Schema.png" width="90%" style="border-radius: 4px;" alt="Data Model Schema" />
</p>

---

## 1. Sales Performance Dashboard

This dashboard provides a macro-level financial overview, allowing users to monitor key metrics, evaluate seasonal trends, and audit product subcategory performance.

### Key Visual Components:
- **Big Numbers (BANs):** High-level overview at the top displaying current vs. previous year comparisons for total sales, profit, and quantity.
- **Monthly Trend Sparklines:** Compact charts visualizing monthly trajectories to easily spot seasonal peaks and troughs.
- **Product Subcategory Analysis:** Comparative bar charts assessing sales performance against profitability across categories.
- **Weekly Performance Matrix:** Detailed tracking of short-term performance against weekly averages to highlight high- and low-performing periods.

<p align="center">
  <img src="https://raw.githubusercontent.com/gcasarin/Tableau_Project-Sales-and-Customer-Dashboard/blob/main/Charts/Sales_Dash.png" width="95%" style="border-radius: 4px;" alt="Sales Dashboard View" />
</p>

---

## 2. Customer Analytics Dashboard

Designed specifically for marketing and management teams, this view shifts focus to behavioral patterns, customer segmentation, and loyalty drivers.

### Key Visual Components:
- **Customer KPI Overview:** Instant metrics tracking total active customers, sales per customer, and order volumes year-over-year.
- **Customer Activity Sparklines:** Month-by-month tracking of engagement frequency and monetary value.
- **Loyalty Distribution Plot:** Visual representation grouping customers by number of orders to quantify one-time buyers versus loyal repeat purchasers.
- **Top Customers Ranking Table:** A detailed tabular view highlighting top 10 customers by total profit, detailing order counts and recency of last purchase.

<p align="center">
  <img src="https://raw.githubusercontent.com/gcasarin/Tableau_Project-Sales-and-Customer-Dashboard/blob/main/Charts/Customer_Dash.png" width="95%" style="border-radius: 4px;" alt="Customer Dashboard View" />
</p>

---

## Dashboard Interactivity & User Experience

Both dashboards are engineered for high interactivity, enabling stakeholders to customize views dynamically without modifying underlying data structures:
- **Cross-Filtering & Action Filters:** Clicking any graphical element instantly cross-filters the entire dashboard layout for seamless exploration.
- **Granular Parameter Controls:** Users can slice metrics by product hierarchies (Category, Subcategory) and geographical regions (Region, State, City).
- **Historical Navigation:** Instant switching between temporal windows allows for robust retrospective analysis and strategic planning.

<p align="center">
  <img src="https://raw.githubusercontent.com/gcasarin/Tableau_Project-Sales-and-Customer-Dashboard/blob/main/Charts/Filters_Panel.png" width="85%" style="border-radius: 4px;" alt="Filters and Interactivity Panel" />
</p>

---

## Conclusions

This Tableau project demonstrates how robust data modeling and intuitive visual design can bridge the gap between raw transactional data and strategic decision-making. By pairing a macroeconomic financial overview with micro-level customer behavioral analytics, the solution provides a versatile framework for retail monitoring, inventory planning, and targeted marketing campaigns.

---

## Links & Resources

> <span style="font-size: 0.98rem; line-height: 1.8;">
> ▶️ **Tableau Public Dashboard:** [View Live Viz](https://public.tableau.com/app/profile/gabriele.casarin/viz/Sales_Dashboard_Project_17510126295020/SalesDash)<br>
> 📌 **GitHub Repository:** [Project Source & Datasets](https://github.com/GabryGit/Tableau_Project-Sales-and-Customer-Dashboard)<br>
> </span>
