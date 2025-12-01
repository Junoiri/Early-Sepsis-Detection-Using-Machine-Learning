
# Early Sepsis Detection Using Machine Learning
AI-based early warning system for sepsis detection, predicting onset up to 6 hours before diagnosis using ICU time-series data.

### Group Members
- Pola Polańska  
- Marta Sadłowska  
- Łukasz Stecyk  

---
### 1.Background
Sepsis is a severe clinical syndrome caused by a systemic response to infection. It is common in hospitals and responsible for a large portion of mortality. According to O’Brien et al. (2007), sepsis affects hundreds of thousands of patients every year and is associated with high mortality, especially once organ failure or septic shock develops. The clinical signs often appear gradually and are non-specific, which makes early recognition difficult for medical staff.

### 2.Goal
This project builds a machine learning system that predicts sepsis several hours before clinical diagnosis. The aim is to assist ICU staff by providing an early warning when a patient begins to show patterns associated with the development of sepsis. The model outputs a probability between 0 and 1, representing the risk that the patient will progress toward sepsis in the near future. If the probability crosses a defined threshold, the system can issue an alert.

### 3.Data and prediction window
The dataset comes from the PhysioNet/ Kaggle Sepsis Prediction Challenge 2019. It contains hourly measurements of vital signs, laboratory values, and demographic information for ICU patients. Each record includes a time series and a SepsisLabel marking the hour when sepsis was clinically confirmed.
We focus on predicting sepsis up to six hours before diagnosis. This window reflects the clinical need to intervene early, as supported by evidence showing that timely treatment improves survival.

### 4. Modelling approach
The workflow includes data preprocessing, exploratory analysis, feature engineering, and model training.
We build two types of models:
Classical machine learning models based on engineered features, including logistic regression, random forests, and gradient boosting (XGBoost and LightGBM).
Sequence models trained directly on hourly time-series windows, including LSTM and GRU networks.
Each model outputs a probability of sepsis within the next six hours.

### 5. Intended use
The system is designed as an early warning component that could be integrated into hospital decision-support infrastructure. It updates the risk estimate as new measurements appear, allowing clinicians to monitor changes over time. The tool is not intended to replace clinical judgment but to provide an additional layer of support in environments where early detection is critical.
