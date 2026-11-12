---
layout: project
title: "Data Analysis and Predictive Modeling in FDM 3D Printing"
date: 2026-06-01
excerpt: "An in-depth analysis investigating the relationships between FDM printing parameters and mechanical properties (PLA & ABS), featuring predictive ML models."
image: https://raw.githubusercontent.com/gcasarin/Python_Project-Data-Analysis-and-Predictive-Modeling-of-Mechanical-Properties-in-FDM-3D-Printing/main/Project_charts/3d-printing-project.jpg
link: https://github.com/gcasarin/Python_Project-Data-Analysis-and-Predictive-Modeling-of-Mechanical-Properties-in-FDM-3D-Printing

badges:
  - text: "Data Science"
    bg: "#00cec9"
    color: "#ffffff"
  - text: "Machine Learning"
    bg: "#fdcb6e"
    color: "#2d3436"
---

## Project Overview
This study uses the **"3D Printer Dataset for Mechanical Engineers"** (from Kaggle) to investigate relationships between FDM printing parameters and the mechanical properties of PLA and ABS printed specimens. 

The main goals of this project are:
1. Conduct exploratory data analysis (EDA) and correlation assessment to determine key factors influencing surface roughness and tensile strength.
2. Develop predictive Machine Learning models to estimate mechanical properties based on process parameters.

👉 **[View Full Python Notebook on GitHub](https://github.com/gcasarin/Python_Project-Data-Analysis-and-Predictive-Modeling-of-Mechanical-Properties-in-FDM-3D-Printing/blob/main/Data-Analysis-and-Predictive-Modeling-of-Mechanical-Properties-in-FDM-3D-Printing.ipynb)**

---

## Tools & Libraries 

| Categoria | Tecnologie / Librerie |
| :--- | :--- |
| **Core Language** | Python |
| **Data Processing** | Pandas, NumPy |
| **Machine Learning** | Scikit-Learn |
| **Visualization** | Matplotlib, Seaborn |

---

## Exploratory Data Analysis (EDA)

The data analysis was conducted on a dataset of 50 FDM-printed specimens (25 ABS and 25 PLA samples) produced with an Ultimaker S5 printer.

### Key Insights:
- **Material Performance:** PLA achieved higher tensile strength and approximately 30% lower surface roughness compared to ABS.
- **Infill Pattern Impact:** The choice of infill pattern (*grid* vs. *honeycomb*) had a limited effect across most mechanical responses, though PLA was slightly more sensitive to pattern selection regarding surface finish.

<p align="center">
  <img src="https://raw.githubusercontent.com/gcasarin/Python_Project-Data-Analysis-and-Predictive-Modeling-of-Mechanical-Properties-in-FDM-3D-Printing/main/Project_charts/4.png" width="80%" />
</p>

---

## Correlation Assessment

Correlation analysis revealed several significant relationships:
- **Layer Height vs. Surface Roughness:** Strong positive correlation ($r \approx 0.8$), confirming that larger layer heights degrade surface quality.
- **Structural Reinforcement:** Tensile strength increased with both wall thickness ($r \approx 0.4$) and infill density ($r \approx 0.36$).

<p align="center">
  <img src="https://raw.githubusercontent.com/gcasarin/Python_Project-Data-Analysis-and-Predictive-Modeling-of-Mechanical-Properties-in-FDM-3D-Printing/main/Project_charts/8.png" width="80%">
</p>

- **Material Sensitivity:** PLA tensile strength was more sensitive to infill density, whereas ABS strength was influenced more by wall thickness.

<p align="center">
  <img src="https://raw.githubusercontent.com/gcasarin/Python_Project-Data-Analysis-and-Predictive-Modeling-of-Mechanical-Properties-in-FDM-3D-Printing/main/Project_charts/6.png" width="80%">
</p>

---

## Predictive Modeling & Evaluation

To evaluate model performances efficiently, here is a summary of the implemented machine learning approaches:

<div style="max-width: 800px; margin: 0 auto; overflow-x: auto;">
  <table class="table table-bordered table-striped" style="font-size: 0.95rem;">
    <thead>
      <tr>
        <th>Model</th>
        <th>Target Variable</th>
        <th>Evaluation metric</th>
        <th>Results</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td><strong>Linear Regression</strong></td>
        <td>Surface Roughness</td>
        <td>R<sup>2</sup>=0.81<br><small class="text-muted">(Single fold)</small></td>
        <td>Overfitting risk due to small dataset size</td>
      </tr>
      <tr>
        <td><strong>Random Forest Regressor</strong></td>
        <td>Tensile Strength</td>
        <td>R<sup>2</sup> = 0.55<br>MAE ~ 5 MPa</small></td>
        <td>Good model stability across k-fold tests</td>
      </tr>
    </tbody>
  </table>
</div>


### 1. Surface Roughness Prediction (Linear Regression)
An easy-to-use **Linear Regression** model was built using key features (*layer height, nozzle temperature, wall thickness, and material type*).

<p align="center">
  <img src="https://raw.githubusercontent.com/gcasarin/Python_Project-Data-Analysis-and-Predictive-Modeling-of-Mechanical-Properties-in-FDM-3D-Printing/main/Project_charts/10a.png" width="70%">
</p>

{: .red-infobox}
Sebbene il modello iniziale abbia registrato un ottimo $R^2 = 0.81$, la validazione incrociata ($k$-fold) ha evidenziato una limitata capacità di generalizzazione causata dal numero ridotto di osservazioni (50 campioni).

<p align="center">
  <img src="https://raw.githubusercontent.com/gcasarin/Python_Project-Data-Analysis-and-Predictive-Modeling-of-Mechanical-Properties-in-FDM-3D-Printing/main/Project_charts/10.png" width="90%">
</p>

### 2. Tensile Strength Prediction (Random Forest Regressor)
A **Random Forest Regressor** was implemented to capture non-linear relationships and feature interactions.

<p align="center">
  <img src="https://raw.githubusercontent.com/gcasarin/Python_Project-Data-Analysis-and-Predictive-Modeling-of-Mechanical-Properties-in-FDM-3D-Printing/main/Project_charts/11a.png" width="80%">
</p>

<div class="alert alert-success" role="alert">
  ✅ <strong>Risultato chiave:</strong> Nonostante la quantità limitata di dati, i test ripetuti in $k$-fold hanno confermato una buona stabilità del modello, fornendo una solida base di partenza per future ottimizzazioni con dataset più estesi.
</div>

---

## 🔗 Links & Resources
* 🐙 **GitHub Repository:** [Project Repo](https://github.com/gcasarin/Python_Project-Data-Analysis-and-Predictive-Modeling-of-Mechanical-Properties-in-FDM-3D-Printing)
* 📓 **Jupyter Notebook:** [Direct Link to Code](https://github.com/gcasarin/Python_Project-Data-Analysis-and-Predictive-Modeling-of-Mechanical-Properties-in-FDM-3D-Printing/blob/main/Data-Analysis-and-Predictive-Modeling-of-Mechanical-Properties-in-FDM-3D-Printing.ipynb)
* 📊 **Dataset:** [3D Printer Dataset on Kaggle](https://www.kaggle.com/datasets/afumetto/3dprinter/data?select=data.csv)
