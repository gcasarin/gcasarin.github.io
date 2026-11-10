---
layout: project
title: "Data Analysis and Predictive Modeling in FDM 3D Printing"
date: 2026-06-01
excerpt: "An in-depth analysis investigating the relationships between FDM printing parameters and mechanical properties (PLA & ABS), featuring predictive ML models."
image: https://raw.githubusercontent.com/gcasarin/Python_Project-Data-Analysis-and-Predictive-Modeling-of-Mechanical-Properties-in-FDM-3D-Printing/main/Project_charts/1.png
link: https://github.com/gcasarin/Python_Project-Data-Analysis-and-Predictive-Modeling-of-Mechanical-Properties-in-FDM-3D-Printing
---

## 📌 Project Overview
This study uses the **"3D Printer Dataset for Mechanical Engineers"** (from Kaggle) to investigate relationships between FDM printing parameters and the mechanical properties of PLA and ABS printed specimens. 

The main goals of this project are:
1. Conduct exploratory data analysis (EDA) and correlation assessment to determine key factors influencing surface roughness and tensile strength.
2. Develop predictive Machine Learning models to estimate mechanical properties based on process parameters.

👉 **[View Full Python Notebook on GitHub](https://github.com/gcasarin/Python_Project-Data-Analysis-and-Predictive-Modeling-of-Mechanical-Properties-in-FDM-3D-Printing/blob/main/Data-Analysis-and-Predictive-Modeling-of-Mechanical-Properties-in-FDM-3D-Printing.ipynb)**

---

## 🛠️ Tools & Libraries Used
* **Language:** Python
* **Libraries:** 
  * `Pandas` & `Numpy` (Data Cleaning & Manipulation)
  * `Matplotlib` & `Seaborn` (Exploratory Data Visualization)
  * `Scikit-Learn` (Predictive Modeling: Linear Regression & Random Forest)

---

## 📊 Exploratory Data Analysis (EDA)

The data analysis was conducted on a dataset of 50 FDM-printed specimens (25 ABS and 25 PLA samples) produced with an Ultimaker S5 printer.

<p align="center">
  <img src="https://raw.githubusercontent.com/gcasarin/Python_Project-Data-Analysis-and-Predictive-Modeling-of-Mechanical-Properties-in-FDM-3D-Printing/main/Project_charts/2.png" width="45%" />
  <img src="https://raw.githubusercontent.com/gcasarin/Python_Project-Data-Analysis-and-Predictive-Modeling-of-Mechanical-Properties-in-FDM-3D-Printing/main/Project_charts/3.png" width="45%" />
</p>

### Key Insights:
* **Material Performance:** PLA achieved higher tensile strength and approximately 30% lower surface roughness compared to ABS.
* **Infill Pattern Impact:** The choice of infill pattern (*grid* vs. *honeycomb*) had a limited effect across most mechanical responses, though PLA was slightly more sensitive to pattern selection regarding surface finish.

<p align="center">
  <img src="https://raw.githubusercontent.com/gcasarin/Python_Project-Data-Analysis-and-Predictive-Modeling-of-Mechanical-Properties-in-FDM-3D-Printing/main/Project_charts/4.png" width="45%" />
  <img src="https://raw.githubusercontent.com/gcasarin/Python_Project-Data-Analysis-and-Predictive-Modeling-of-Mechanical-Properties-in-FDM-3D-Printing/main/Project_charts/5.png" width="45%" />
</p>

---

## 📈 Correlation Assessment

Correlation analysis revealed several significant relationships:
* **Layer Height vs. Surface Roughness:** Strong positive correlation ($r \approx 0.8$), confirming that larger layer heights degrade surface quality.
* **Structural Reinforcement:** Tensile strength increased with both wall thickness ($r \approx 0.4$) and infill density ($r \approx 0.36$).

<p align="center">
  <img src="https://raw.githubusercontent.com/gcasarin/Python_Project-Data-Analysis-and-Predictive-Modeling-of-Mechanical-Properties-in-FDM-3D-Printing/main/Project_charts/8.png" width="80%">
</p>

* **Material Sensitivity:** PLA tensile strength was more sensitive to infill density, whereas ABS strength was influenced more by wall thickness.

<p align="center">
  <img src="https://raw.githubusercontent.com/gcasarin/Python_Project-Data-Analysis-and-Predictive-Modeling-of-Mechanical-Properties-in-FDM-3D-Printing/main/Project_charts/6.png" width="80%">
</p>

---

## 🤖 Predictive Modeling

### 1. Surface Roughness Prediction (Linear Regression)
An easy-to-use **Linear Regression** model was built using key features (*layer height, nozzle temperature, wall thickness, and material type*).

<p align="center">
  <img src="https://raw.githubusercontent.com/gcasarin/Python_Project-Data-Analysis-and-Predictive-Modeling-of-Mechanical-Properties-in-FDM-3D-Printing/main/Project_charts/10a.png" width="70%">
</p>

* **Performance:** $R^2 = 0.81$ on single fold. 
* **Validation Note:** $k$-fold cross-validation indicated limited generalization capability due to the small sample size (50 observations), highlighting the need for more training data.

<p align="center">
  <img src="https://raw.githubusercontent.com/gcasarin/Python_Project-Data-Analysis-and-Predictive-Modeling-of-Mechanical-Properties-in-FDM-3D-Printing/main/Project_charts/10.png" width="90%">
</p>

---

### 2. Tensile Strength Prediction (Random Forest Regressor)
A **Random Forest Regressor** was implemented to capture non-linear relationships and feature interactions.

<p align="center">
  <img src="https://raw.githubusercontent.com/gcasarin/Python_Project-Data-Analysis-and-Predictive-Modeling-of-Mechanical-Properties-in-FDM-3D-Printing/main/Project_charts/11a.png" width="80%">
</p>

* **Performance:** $R^2 = 0.55$ and $\text{MAE} \approx 5\text{ MPa}$, showing moderate predictive power.
* **Stability:** Repeated $k$-fold cross-validation confirmed high model stability, providing a strong foundation for future optimization with larger datasets.

---

## 🔗 Links & Resources
* 🐙 **GitHub Repository:** [Project Repo](https://github.com/gcasarin/Python_Project-Data-Analysis-and-Predictive-Modeling-of-Mechanical-Properties-in-FDM-3D-Printing)
* 📓 **Jupyter Notebook:** [Direct Link to Code](https://github.com/gcasarin/Python_Project-Data-Analysis-and-Predictive-Modeling-of-Mechanical-Properties-in-FDM-3D-Printing/blob/main/Data-Analysis-and-Predictive-Modeling-of-Mechanical-Properties-in-FDM-3D-Printing.ipynb)
* 📊 **Dataset:** [3D Printer Dataset on Kaggle](https://www.kaggle.com/datasets/afumetto/3dprinter/data?select=data.csv)
