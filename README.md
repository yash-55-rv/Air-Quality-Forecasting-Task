
# AI-ML Recruitment Task Submission

## 1. Candidate Details
* **Name:** Venkat Yashwanth Rachamalla
* **Year:** Second Year, B.Tech
* **Branch:** Computer Science and Engineering (CSE Core)
* **Registration Number:** RA2511003011463

---

## 2. Tasks Completed
* **Task 1:** Air Quality Forecasting

---

## 3. Problem Statement
The goal of this project is to analyze historical air quality data from the UCI dataset and build a machine learning model to predict future air quality. Specifically, I focused on predicting **Benzene (C6H6(GT))** concentrations, which is helpful for tracking and forecasting pollution levels in urban areas over time.

---

## 4. Approach
* **Data Cleaning:** First, I removed the completely empty rows and columns from the raw CSV file. The dataset used a value of `-200` to represent missing sensor readings, so I replaced all of them with proper `NaN` values and used a forward-fill (`ffill()`) method to replace the blanks with the most recent valid reading. I also combined the Date and Time columns into a single datetime index.
* **EDA (Exploratory Data Analysis):** I plotted charts to look at the distribution of the pollutants and created a bar plot showing the average pollution levels by hour of the day. This clearly showed spikes matching morning and evening rush hours. I also generated a heatmap to check the correlation between different sensors.
* **Feature Engineering:** To help the model predict better, I created time features (`Hour` and `DayOfWeek`) from the timestamp. I also created lag features (`Lag_1h` and `Lag_2h`) and a 3-hour rolling average so the model could understand recent historical trends.
* **Modeling:** Since this is a time-series problem, I avoided a randomized train/test split because that would cause data leakage. Instead, I did a chronological split (the first 80% of the data for training, and the remaining 20% for testing) and trained a Random Forest Regressor on it.

---

## 5. Technologies Used
* **Language:** Python
* **Libraries:** Pandas, NumPy, Matplotlib, Seaborn, Scikit-Learn

---

## 6. Results
I evaluated the Random Forest model on the unseen test data using standard regression metrics:

* **R² Score:** 0.7973 (This means the model explains about 80% of the variance in the data)
* **Mean Absolute Error (MAE):** 1.8150
* **Root Mean Squared Error (RMSE):** 2.8934

### Feature Importance:
When looking at what features the model relied on the most:
* `Lag_1h`: **77.05%** (What the pollution was 1 hour ago is by far the biggest indicator)
* `Hour`: **8.50%** (The time of day, capturing the daily rush hour traffic cycles)
* `Lag_2h`: **6.99%**
* `Rolling_Mean_3h`: **4.92%**
* `DayOfWeek`: **2.54%**

---

## 7. Key Learnings
1. **Handling Real-World Data Flags:** I learned that real datasets often hide missing values under specific numbers like `-200`, rather than leaving them blank, which messes up statistics if you don't clean them first.
2. **Time-Series Splitting Rules:** I realized why you cannot use regular randomized train-test splits on temporal data. Splitting data randomly mixes the past and the future, which makes the model look artificially accurate during training but fail in real life.
3. **The Importance of Lags:** I noticed how heavily a time-series model relies on the immediate past (`Lag_1h`). Air quality doesn't change instantly; it carries over from the hour before.

---

## 8. Challenges & Solutions
* **The Challenge:** While setting up the script, I ran into an error where the notebook couldn't find the `RandomForestRegressor` and gave a `NameError`. I also had some trouble initially setting up the data loading block because the file URL was hitting a timeout error.
* **How I Solved It:** I fixed the data loading problem by manually adjusting the download logic to pull directly from the verified UCI archive link inside a fresh code cell to clear the cache. I fixed the code error by ensuring all necessary scikit-learn imports (`sklearn.ensemble` and `sklearn.metrics`) were declared explicitly right at the top of the cell before initializing the model.
