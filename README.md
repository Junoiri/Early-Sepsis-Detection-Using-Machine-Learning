
# Early Sepsis Detection Using Machine Learning
AI-based early warning system for sepsis detection, predicting onset up to 6 hours before diagnosis using ICU time-series data.

### Group Members
- Pola Polańska  
- Marta Sadłowska  
- Łukasz Stecyk  

---

### Learn Model Canvas

You can view our full project canvas here:

[**Miro Board – Early Sepsis Detection Project**](https://miro.com/app/board/uXjVJ9P2hXQ=/?share_link_id=876634889655)

---

### 1. Motivation

Sepsis is a life-threatening condition that occurs when the body overreacts to an infection.

It can quickly lead to organ failure or death if not recognized and treated early.

The main problem is **time** -every hour of delay in diagnosis lowers the patient’s chances of survival.

Doctors in intensive care units (ICUs) monitor many parameters, but the signs of sepsis often appear gradually and can be missed.

This is why we want to create a system that can help detect sepsis automatically, **before it becomes critical**.

---

### 2. Goal of the Project

Our goal is to build a **Python-based machine learning tool** that can **predict sepsis several hours before it is officially diagnosed**.

The idea is to give doctors and nurses an early warning, so they can react faster and start treatment sooner.

In simple terms:

> We want an AI system that says, “Hey, this patient might develop sepsis soon -check them now.”

---

### 3. Dataset and What It Contains

We will use a large dataset collected from ICU patients (PhysioNet / Kaggle Sepsis Prediction Challenge 2019).

For each patient, there are hourly records of around **40 medical parameters**, including:

- **Vital signs:** heart rate, temperature, blood pressure, oxygen level, and breathing rate.
- **Laboratory results:** pH, glucose, creatinine, lactate, and other blood chemistry values.
- **Demographic data:** age, gender, and ICU unit.

Each patient has a unique ID, and their data forms a **time series**, showing how their condition changes hour by hour.

The dataset also includes a special column called **“SepsisLabel”**, which indicates when sepsis was clinically confirmed.

This lets us train our model to recognize the patterns that typically appear **hours before** sepsis is detected by doctors.

---

### 4. Why a 6-Hour Prediction Window?

We decided to focus on predicting sepsis **6 hours before** it is officially diagnosed.

This time window is often used in clinical research because it strikes the right balance between **accuracy** and **impact**.

Predicting too early (12–24 hours) reduces reliability, while predicting too late leaves no time for action.

A **6-hour window** gives doctors a realistic amount of time to confirm the diagnosis, start antibiotics or fluids, and prevent further organ damage -while still keeping the model’s predictions accurate and trustworthy.

In short, it’s a **sweet spot** between **model performance** and **clinical usefulness**.

---

### 5. How the Model Will Work

We will train a machine learning model that looks at a few hours of patient data at a time and predicts whether sepsis will develop soon.

Technically, it will try to detect **sepsis up to 6 hours before** it’s clinically confirmed.

If the model sees warning signs -for example, a drop in blood pressure combined with rising lactate levels -it will calculate a **risk probability** between 0 and 1.

This probability represents how likely the patient is to develop sepsis in the next few hours.

If the probability passes a chosen **threshold** (for example, 0.7), the system will generate an alert.

The probability will be obtained directly from the model’s output:

- For logistic regression and XGBoost: via the **sigmoid/logistic function**, which converts the model’s score into a probability.
- For LSTM or Transformer models: from the **final output neuron** after a sigmoid activation, updated each hour as new data arrives.

In both cases, the number reflects how confident the model is that the patient is progressing toward sepsis.

We will test different ML models:

#### Simpler Baselines

- **Logistic Regression / Random Forest** – fast, interpretable, and good for verifying the setup.

#### Gradient Boosting (XGBoost / LightGBM)

- Excellent for tabular clinical data.
- Learns complex feature interactions (e.g., *low MAP + high lactate*).
- Performs well with aggregated time-window features (last value, slope, mean).

#### Recurrent Neural Networks (LSTM / GRU)

- Ideal for **sequential hourly data**, capturing how vitals evolve over time.
- Detects early patterns that appear before sepsis diagnosis.

#### Transformer-based Models (mini-Transformer / Temporal Fusion Transformer)

- Can attend to the **most relevant hours and features**.
- Offer better interpretability through attention weights but are more complex to train.

---

### 6. Comparison to Existing ML Approaches

Several ML approaches have already been tested for sepsis prediction, for example:

- **Random Forests and Gradient Boosting models** from PhysioNet Challenge (2019), which used feature-engineered vitals and labs.
- **Deep LSTMs** that model temporal sequences directly, used in research projects by MIT Critical Data (2020–2022).
- **CNN + LSTM hybrids** that process both short-term and long-term patterns.
- **Autoencoder-based anomaly detection** models that flag patients whose vitals deviate from normal trends.

However, most of these models:

- Focus mainly on **competition-style metrics (AUROC)**, not clinical interpretability or usability.
- Are trained directly on Kaggle splits, often leading to **patient-level data leakage**.
- Are published as **standalone notebooks**, not deployable hospital tools.

Our project is **different** because:

1. We prioritize **clinical usefulness and explainability**, not only accuracy.
2. The system is designed as a **plug-in hospital add-on**, not a research-only model.
3. We use **time-series analysis** for continuous probability updates, creating dynamic risk tracking over time.
4. We evaluate **false alarms, alert frequency, and early detection time**, making our results directly applicable to real hospital conditions.

This combination makes our approach **implementation-ready and clinically meaningful**, unlike most existing research models.

---

### 7. Integration Idea -“AI Hospital Add-On”

In the future, our tool could work as an **add-on to hospital systems**, like an “AI-based alert module.”

It would automatically analyze patient data in real time and send a notification to hospital staff through the internal system (for example, an IHR platform).

If something abnormal is detected, a doctor or nurse would immediately receive an alert saying that the patient might be developing sepsis soon.

This kind of early warning could help staff take action sooner -for instance, start antibiotics or fluids earlier -and potentially **save lives**.

---

### 8. Expected Benefits

- **Faster Diagnosis:** helps detect sepsis before it becomes severe.
- **Higher Survival Rate:** patients get treatment sooner, improving outcomes.
- **Reduced Workload:** fewer missed cases, less manual monitoring.
- **Smarter Alerts:** tuned thresholds minimize false positives and alert fatigue.
- **Continuous Tracking:** probability updates every hour give a clearer view of patient condition.

---

### 9. Why This Project Matters

AI is slowly becoming part of clinical decision-support systems, but **early sepsis prediction using time-series machine learning** remains rare -especially in European hospitals.

By focusing on **real-time probability tracking** and **seamless integration with existing systems**, our project stands out from previous Kaggle-style solutions.

If successful, this framework could be adapted for detecting other critical conditions -like septic shock, respiratory failure, or cardiac arrest.

---

### 10. Target Users / Stakeholders (Who It’s For)

The main users would be doctors and nurses in intensive care units, who rely on real-time monitoring.

Hospital IT departments could integrate and maintain the system.

Later, the solution could extend to smaller clinics or telemedicine centers that track high-risk patients remotely.

---

### 11. Evaluation Plan / How We’ll Measure Success

We will evaluate the model using both **technical** and **clinical** metrics:

- **Technical:** AUROC, AUPRC, sensitivity, specificity, calibration.
- **Clinical:** alerts per patient-day, false-positive rate, and average time gained before diagnosis.

We’ll select the model that offers the **best trade-off between early detection and reliability**, not just the highest raw accuracy.

---

### 12. Ethical & Regulatory Awareness

Because the tool deals with clinical data, all patient records will be **de-identified and handled in line with GDPR**.

In the long term, the system could qualify as **Software as a Medical Device (SaMD)** under **EU MDR Rule 11**, meaning it supports clinicians without replacing their judgment.
