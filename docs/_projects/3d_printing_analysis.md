---
layout: project
title: "3D Printing Data Analysis and Predictive Modeling"
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
This study uses the **"3D Printer Dataset for Mechanical Engineers"** (Kaggle) to investigate relationships between FDM printing parameters and the mechanical properties of PLA and ABS printed specimens. 

The main goals of this project are:
1. Conduct exploratory data analysis (EDA) and correlation assessment to determine key factors influencing surface roughness and tensile strength.
2. Develop predictive Machine Learning models to estimate mechanical properties based on process parameters.

🔗 **[View Full Python Notebook on GitHub](https://github.com/gcasarin/Python_Project-Data-Analysis-and-Predictive-Modeling-of-Mechanical-Properties-in-FDM-3D-Printing/blob/main/Data-Analysis-and-Predictive-Modeling-of-Mechanical-Properties-in-FDM-3D-Printing.ipynb)**
{: .blue-infobox}

---

## Tools & Libraries 

| Category | Language / Library |
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

<div style="display: flex; flex-wrap: wrap; justify-content: center; gap: 20px; margin: 30px 0;">
  <div style="flex: 1; min-width: 320px; max-width: 48%;">
    <img src="https://raw.githubusercontent.com/gcasarin/Python_Project-Data-Analysis-and-Predictive-Modeling-of-Mechanical-Properties-in-FDM-3D-Printing/main/Project_charts/4.png" width="100%" style="border-radius: 4px;" alt="Chart 4" />
  </div>
  <div style="flex: 1; min-width: 320px; max-width: 48%;">
    <img src="https://raw.githubusercontent.com/gcasarin/Python_Project-Data-Analysis-and-Predictive-Modeling-of-Mechanical-Properties-in-FDM-3D-Printing/main/Project_charts/5.png" width="100%" style="border-radius: 4px;" alt="Chart 5" />
  </div>
</div>
---

## Correlation Assessment

Correlation analysis revealed several significant relationships:
- **Layer Height vs. Surface Roughness:** Strong positive correlation (*r* ≈ 0.8), confirming that larger layer heights degrade surface quality.

<p align="center">
  <img src="https://raw.githubusercontent.com/gcasarin/Python_Project-Data-Analysis-and-Predictive-Modeling-of-Mechanical-Properties-in-FDM-3D-Printing/main/Project_charts/8.png" width="80%">
</p>

- **Structural Reinforcement:** Tensile strength increased with both wall thickness (*r* ≈ 0.4) and infill density (*r* ≈ 0.36).
- **Material Sensitivity:** PLA tensile strength was more sensitive to infill density, whereas ABS strength was influenced more by wall thickness.

<p align="center">
  <img src="https://raw.githubusercontent.com/gcasarin/Python_Project-Data-Analysis-and-Predictive-Modeling-of-Mechanical-Properties-in-FDM-3D-Printing/main/Project_charts/6.png" width="80%">
</p>

---

## Predictive Modeling & Evaluation

Predictive modeling was performed to estimate surface roughness and tensile strength from FDM printing parameters.
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

```python
from sklearn.linear_model import LinearRegression

X = df[['layer_height', 'nozzle_temperature', 'wall_thickness', 'material']]  
y = df['roughness']   

numerical_features = ['layer_height', 'nozzle_temperature', 'wall_thickness']  
categorical_features = ['material']   

preprocessor = ColumnTransformer(
    transformers=[
        ('numerical', StandardScaler(), numerical_features),    
        ('categorical', OneHotEncoder(drop='first', handle_unknown='ignore'), categorical_features) 
         ])                                                                            

linear_model_pipeline = Pipeline(
    steps=[
        ('preprocessor', preprocessor),
        ('regressor', LinearRegression())]
)

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=15)

linear_model_pipeline.fit(X_train, y_train)
```

<p align="center">
  <img src="https://raw.githubusercontent.com/gcasarin/Python_Project-Data-Analysis-and-Predictive-Modeling-of-Mechanical-Properties-in-FDM-3D-Printing/main/Project_charts/10a.png" width="95%">
</p>

**Key Results:** Although the initial model achieved an excellent R<sup>2</sup> = 0.81, **<i>k</i>-fold cross-validation highlighted limited generalization** due to the small sample size (50 observations).
{: .blue-infobox}

<p align="center">
  <img src="https://raw.githubusercontent.com/gcasarin/Python_Project-Data-Analysis-and-Predictive-Modeling-of-Mechanical-Properties-in-FDM-3D-Printing/main/Project_charts/10.png" width="95%">
</p>

### 2. Tensile Strength Prediction (Random Forest Regressor)
A **Random Forest Regressor** was implemented to capture non-linear relationships and feature interactions.

```python
from sklearn.ensemble import RandomForestRegressor

X2 = df.drop(columns=['roughness','elongation','tension_strenght','print_speed', 'fan_speed','bed_temperature'])
y2 = df['tension_strenght']

numerical_features2 = ['layer_height', 'wall_thickness','infill_density','nozzle_temperature'] 
categorical_features2 = ['infill_pattern', 'material']  

preprocessor2 = ColumnTransformer(
    transformers=[
        ('numerical', StandardScaler(), numerical_features2),   
        ('categorical', OneHotEncoder(handle_unknown='ignore'), categorical_features2)  
         ])                                                                                          


random_forest_pipeline = Pipeline(steps=[
    ('preprocessor', preprocessor2), # Il preprocessor rimane lo stesso di prima
    ('regressor', RandomForestRegressor(n_estimators=50, max_depth=4, random_state=10))
])

X2_train, X2_test, y2_train, y2_test = train_test_split(X2, y2, test_size=0.2, random_state=15)

random_forest_pipeline.fit(X2_train, y2_train)
```

<p align="center">
  <img src="https://raw.githubusercontent.com/gcasarin/Python_Project-Data-Analysis-and-Predictive-Modeling-of-Mechanical-Properties-in-FDM-3D-Printing/main/Project_charts/11a.png" width="95%">
</p>

**Key Results:** Despite the limited dataset size, repeated <i>k</i>-fold cross-validation confirmed **robust model stability**, establishing a solid baseline for future optimization with larger datasets.
{: .blue-infobox}

---

## Conclusions

Experimental and statistical analyses demonstrated that **PLA outperforms ABS** in FDM 3D printing, delivering on average **25% higher tensile strength** and **30% lower surface roughness**. While the infill pattern (Grid vs. Honeycomb) showed negligible impact, mechanical behavior was heavily driven by process parameters: **layer height** strongly dictated surface roughness, whereas **wall thickness** and **infill density** (especially in PLA) boosted tensile strength.

From a predictive modeling perspective, **Linear Regression** identified layer height and nozzle temperature as the primary drivers of roughness (R<sup>2</sup> = 0.81), though *k*-fold cross-validation (*k* = 3) exposed generalization limits due to the small sample size (50 records). Conversely, the **Random Forest Regressor** captured non-linear parameter interactions to predict tensile strength (R<sup>2</sup> = 0.55, MAE ≈ 5 MPa), proving stable under repeated *k*-fold validation and establishing a solid baseline for future optimization with larger datasets.

---

## Links & Resources

> <span style="font-size: 0.98rem; line-height: 1.8;">
> ▶️ **GitHub Repository:** [Project Repo](https://github.com/gcasarin/Python_Project-Data-Analysis-and-Predictive-Modeling-of-Mechanical-Properties-in-FDM-3D-Printing)<br>
> 📄 **Jupyter Notebook:** [Direct Link to Code](https://github.com/gcasarin/Python_Project-Data-Analysis-and-Predictive-Modeling-of-Mechanical-Properties-in-FDM-3D-Printing/blob/main/Data-Analysis-and-Predictive-Modeling-of-Mechanical-Properties-in-FDM-3D-Printing.ipynb)<br>
> 📌 **Dataset:** [3D Printer Dataset on Kaggle](https://www.kaggle.com/datasets/afumetto/3dprinter/data?select=data.csv)
> </span>
