# Analyzing Effect of Weight on Blood Pressure 
This repo contains code for the report: [Analyzing Effect of Weight on Blood Pressure](https://drive.google.com/file/d/1_HovWcbrhRbZbi9o4iB4M_nV3ofFlBCr/view). 

All code in can be found under: report-and-code-ols-blood-pressure.Rmd
All data used in the project can be found under the src folder. 

## Overview
This project investigates the relationship between body weight and blood pressure levels, with an additional focus on the impact of household smoking. Hypertension, affecting nearly one-third of adults globally, is a significant risk factor for heart disease and stroke. Understanding the factors contributing to hypertension, such as body weight and smoking, can inform public health strategies and personalized interventions.

## Research Question
**What are the differences in blood pressure levels among individuals with varying body weights?**

## Data Source
Data was sourced from the **National Health and Nutrition Examination Survey (NHANES)** for the years 2013-2014. The analysis utilizes two datasets:
- **Physical Examination Dataset**: Contains blood pressure and body weight measurements.
- **Questionnaire Dataset**: Includes household smoking data.

The datasets were joined on `SEQN`, a unique identifier for each respondent.

## Methodology
### Data Preparation
1. **Blood Pressure Calculation**:
   - Variables used: `BPXDI1-BPXDI4` (diastolic) and `BPXSY1-BPXSY4` (systolic).
   - Zero and NA values were removed.
   - Average blood pressure was calculated for each respondent.
   - **Mean Arterial Pressure (MAP)** was computed using the formula:
     \[
     MAP = DP + \frac{1}{3}(SP - DP)
     \]
     where \(DP\) is diastolic pressure and \(SP\) is systolic pressure.

2. **Body Weight**:
   - Variable used: `BMXWT`.
   - NA values were removed.

3. **Household Smoking**:
   - Variable used: `SMD460` (number of household members who smoke).
   - Responses were categorized as:
     - **Smoking Household**: Any value > 0.
     - **Non-Smoking Household**: Value = 0.
   - Invalid responses (> 777) were removed.

### Dataset
- Initial dataset: 9,813 rows.
- After filtering: 7,350 rows.
- Split into:
  - **Training Data**: 2,215 rows.
  - **Test Data**: 5,135 rows.

### Models
Two Ordinary Least Squares (OLS) regression models were evaluated:
1. **Simple Model**:
   \[
   MAP = \beta_0 + \beta_1 \times \text{weight}
   \]
2. **Indicator Variable Model**:
   \[
   MAP = \beta_0 + \beta_1 \times \text{weight} + \beta_2 \times \text{smoking\_household}
   \]

### Assumptions
- **Independence**: Each respondent's data is independent.
- **Identically Distributed**: Data is representative of the U.S. population.
- **No Collinearity**: No significant correlation between weight and household smoking.

### Evaluation
- **T-tests**: Assessed the significance of coefficients.
- **F-test**: Compared the two models to determine if adding the smoking variable improved the model.

## Results
- **Weight**: Statistically significant (\(p < 2.2e-16\)). A 1 lb increase in weight leads to a 0.095 mmHg increase in MAP.
- **Smoking Household**: Not statistically significant (\(p = 0.239\)). The addition of this variable did not improve the model.

### Final Model
The **Simple Model** was selected as the final model due to its interpretability and the lack of significant improvement from the indicator variable model.

## Discussion
- **Household Smoking**: The broad categorization of smoking may have limited the variable's effectiveness. A more targeted approach focusing on the respondent's smoking behavior could yield better insights.
- **Body Weight Transformation**: Considering a log transformation of body weight could stabilize the variable and improve model performance.
- **Data Staleness**: The dataset is from 2013-2014, which may not fully represent the current U.S. population.

## Conclusion
This study confirms a positive relationship between body weight and blood pressure, highlighting the importance of maintaining a healthy weight for cardiovascular health. While household smoking did not significantly impact the model, further research with more precise smoking data could provide additional insights.

## References
- Centers for Disease Control and Prevention. (2014). [2013-2014 NHANES Questionnaire Data](https://wwwn.cdc.gov/Nchs/Nhanes/Search/DataPage.aspx?Component=Questionnaire&Cycle=2013-2014).
- DeMers, D., & Wachs, D. (2023). [Physiology, Mean Arterial Pressure](https://www.ncbi.nlm.nih.gov/books/NBK538226/).
