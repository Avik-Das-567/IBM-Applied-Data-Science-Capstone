# SpaceX Falcon 9 Landing Prediction | IBM Applied Data Science Capstone

## 📌 Project Overview

This capstone project applies a full data science pipeline to a real-world business problem: **predicting whether the SpaceX Falcon 9 first stage will land successfully after launch.**

SpaceX advertises Falcon 9 rocket launches at approximately **$62 million**, compared to other providers who charge upward of **$165 million**. The key to this cost advantage is first-stage reusability. By accurately predicting whether the first stage will land, we can estimate the true cost of a launch — information that is extremely valuable for any company looking to compete with or bid against SpaceX.

---

## 🗂️ Repository Structure

```
IBM-Applied-Data-Science-Capstone/
│
├── Module 1 - Introduction/
│   ├── 1. Data Collection API Lab/
│   │   └── data-collection-api-lab.ipynb
│   ├── 2. Data Collection with Web Scraping lab/
│   │   └── data-collection-with-web-scraping.ipynb
│   └── 3. Data Wrangling/
│       └── data-wrangling.ipynb
│
├── Module 2 - Exploratory Data Analysis/
│   ├── 1. EDA with SQL/
│   │   └── eda-with-sql.ipynb
│   └── 2. EDA with Visualization Lab/
│       └── eda-with-visualization.ipynb
│
├── Module 3 - Interactive Visual Analytics & Dashboards/
│   ├── 1. Interactive Visual Analytics with Folium lab/
│   │   └── interactive-visual-analytics-with-folium.ipynb
│   └── 2. Interactive Dashboard with Plotly Dash/
│       └── spacex_dash_app.py
│
├── Module 4 - Predictive Analysis (Classification)/
│   └── SpaceX-Machine-Learning-Prediction.ipynb
│
└── Final_Presentation_Avik.pdf
```

---

## 🧰 Technologies & Libraries Used

| Category | Tools / Libraries |
|---|---|
| Language | Python 3 |
| Data Collection | `requests`, `BeautifulSoup` |
| Data Manipulation | `pandas`, `numpy` |
| Data Visualization | `matplotlib`, `seaborn` |
| SQL / Database | `sqlite3`, `ipython-sql` |
| Interactive Maps | `folium`, `MarkerCluster`, `MousePosition` |
| Dashboard | `Plotly Dash` |
| Machine Learning | `scikit-learn` |
| ML Models | Logistic Regression, SVM, Decision Tree, KNN |
| Hyperparameter Tuning | `GridSearchCV` |
| Notebook Environment | Jupyter Notebook |

---

## 📦 Modules & Methodology

### Module 1 — Data Collection & Wrangling

#### 🔹 Data Collection via SpaceX REST API
Historical Falcon 9 launch data was collected using the SpaceX public API:
- A `GET` request was made to the SpaceX API endpoint to retrieve historical launch records.
- The JSON response was parsed and converted into a Pandas DataFrame.
- Key features were selected; data was filtered to launches on or before **November 13, 2020**.
- Additional API calls using IDs from rockets, payloads, launchpads, and cores enriched the dataset with booster names, payload masses, orbit types, and core landing outcomes.
- Missing `PayloadMass` values were imputed with the column mean.
- The processed dataset was exported as `dataset_part_1.csv`.

#### 🔹 Data Collection via Web Scraping
Falcon 9 historical launch records were also scraped from a **Wikipedia page** using BeautifulSoup:
- An HTTP GET request retrieved the raw HTML.
- The third HTML table containing launch records was identified and parsed.
- Column names, dates, payloads, booster versions, and landing outcomes were extracted row by row.
- The resulting DataFrame was exported as `spacex_web_scraped.csv`.

#### 🔹 Data Wrangling
- The dataset was inspected for missing values and data type consistency.
- Mission outcomes were converted into a binary **training label (`Class`)**: `1` for successful landing, `0` for unsuccessful.
- "Bad outcomes" such as `False ASDS`, `None None`, `False Ocean`, `False RTLS`, and `None ASDS` were identified and labeled as failures.
- The overall landing success rate was calculated.
- The wrangled dataset was exported as `dataset_part_2.csv`.

---

### Module 2 — Exploratory Data Analysis (EDA)

#### 🔹 EDA with Data Visualization
Using `pandas`, `matplotlib`, and `seaborn`, the following analyses were performed:

| Visualization | Key Insight |
|---|---|
| Flight Number vs. Launch Site | Higher flight numbers (later launches) trend toward more successes; experience improves reliability. |
| Payload Mass vs. Launch Site | VAFB SLC-4E did not handle payloads above ~10,000 kg; heavier payloads don't always mean failure. |
| Success Rate by Orbit Type | ES-L1, GEO, HEO, and SSO show 100% success; GTO shows the lowest rate (~51.9%). |
| Flight Number vs. Orbit Type | LEO success improves with flight number; GTO shows no such relationship. |
| Payload Mass vs. Orbit Type | Polar, LEO, and ISS orbits handle heavy payloads with good success; GTO is less predictable. |
| Launch Success Yearly Trend | Success rate grew from ~0% (2010–2013) to ~84% by 2020, with a notable leap during SpaceX's reusability breakthroughs (2015–2017). |

#### 🔹 EDA with SQL
The SpaceX dataset was loaded into an SQLite database and queried using SQL magic (`%sql`). Key queries included:

- **Task 1:** Retrieved all 4 unique launch sites — `CCAFS LC-40`, `VAFB SLC-4E`, `KSC LC-39A`, `CCAFS SLC-40`.
- **Task 2:** Fetched 5 records for launch sites beginning with `'CCA'`.
- **Task 3:** Total payload mass for NASA (CRS) customers — **48,213 kg**.
- **Task 4:** Average payload mass for `F9 v1.1` booster — **~2,534.67 kg**.
- **Task 5:** First successful ground pad landing date — **2015-12-22**.
- **Task 6:** Listed booster versions with successful drone ship landings and payload mass between 4,000–6,000 kg (`F9 FT B1022`, `F9 FT B1026`, `F9 FT B1021.2`, `F9 FT B1031.2`).
- **Task 7:** Counted mission outcomes — **98 successes**, 1 failure (in flight).
- **Task 8:** Identified booster versions carrying maximum payload mass (multiple `F9 B5` variants).
- **Task 9:** Filtered drone ship failure outcomes in 2015 — months `01` and `04`, both at `CCAFS LC-40`.
- **Task 10:** Ranked landing outcome counts between 2010-06-04 and 2017-03-20 in descending order — "No attempt" (10) topped the list.

---

### Module 3 — Interactive Visual Analytics & Dashboard

#### 🔹 Interactive Map with Folium
Three interactive Folium maps were built to analyze launch site geography:

**Map 1 — Launch Sites Overview**
All SpaceX launch sites were plotted on an interactive map centered at NASA Johnson Space Center. Each site was marked with a `folium.Circle` (1000m radius) and a labeled `folium.Marker`.

**Map 2 — Launch Success/Failure Visualization**
Each individual launch record was represented by a color-coded `folium.Marker`:
- 🟢 **Green** → Successful landing (Class = 1)
- 🔴 **Red** → Failed landing (Class = 0)

A `MarkerCluster` was used to manage overlapping markers at the same coordinates, allowing intuitive site-level success rate assessment.

**Map 3 — Proximity & Distance Analysis**
Distances from each launch site to the nearest **coastline, railway, highway, and city** were calculated using a custom `calculate_distance` function. `folium.PolyLine` objects were drawn between the launch site and each proximity point, with distance labels (in km) displayed on the map.

#### 🔹 Interactive Dashboard with Plotly Dash
A fully interactive web dashboard was built (`spacex_dash_app.py`) featuring:

| Component | Description |
|---|---|
| **Launch Site Dropdown** | Filters the dashboard by "All Sites" or a specific launch site |
| **Pie Chart** | Displays total launch success counts by site (or success/failure ratio for a selected site); KSC LC-39A had the highest success ratio at **76.9%** |
| **Payload Range Slider** | Filters scatter plot data by payload mass (0–10,000 kg) |
| **Scatter Plot** | Plots Payload Mass vs. Launch Outcome, color-coded by Booster Version Category — reveals correlations between payload mass and mission success |

---

### Module 4 — Predictive Analysis (Classification)

A machine learning pipeline was built to classify whether the Falcon 9 first stage would land successfully.

#### 🔹 Data Preparation
- Target variable `Y` extracted from the `Class` column.
- Features `X` standardized using `StandardScaler()`.
- Data split: **80% training / 20% test**.

#### 🔹 Models Evaluated

| Model | Best Hyperparameters | Validation Accuracy | Test Accuracy |
|---|---|---|---|
| Logistic Regression | `C=0.01`, `penalty='l2'`, `solver='lbfgs'` | 0.8464 | 0.8333 |
| Support Vector Machine (SVM) | `C=1.0`, `gamma≈0.032`, `kernel='sigmoid'` | 0.8482 | 0.8333 |
| Decision Tree Classifier | `criterion='entropy'`, `max_depth=10`, `max_features='sqrt'` | **0.8625** | 0.8333 |
| K-Nearest Neighbors (KNN) | `algorithm='auto'`, `n_neighbors=10`, `p=1` | 0.8482 | 0.8333 |

All models used **`GridSearchCV` with 10-fold cross-validation** for hyperparameter tuning.

#### 🔹 Best Model: Decision Tree Classifier
The Decision Tree Classifier achieved the highest **validation accuracy of 0.8625**. All four models converged to the same **test set accuracy of 0.8333**.

The confusion matrix for the best model revealed:
- **True Positives (correctly predicted: landed):** 12
- **True Negatives (correctly predicted: did not land):** 3
- **False Positives:** 3
- **False Negatives:** 0

---

## 📊 Key Findings & Conclusions

1. **Launch success has improved significantly over time.** The overall success rate grew from nearly 0% in 2010–2013 to approximately 84% by 2020, with a major leap coinciding with SpaceX's reusability breakthroughs between 2015–2017.

2. **Higher flight numbers correlate with higher success rates.** More experienced launches, especially at CCAFS SLC-40, show a clear improvement in landing outcomes.

3. **KSC LC-39A is the highest-performing launch site**, with a success ratio of ~76.9% among the four sites.

4. **Orbit type strongly influences landing success.** ES-L1, GEO, HEO, and SSO achieved 100% success rates in the dataset, while GTO had the lowest at ~51.9%.

5. **VAFB SLC-4E does not support heavy payloads**, with no recorded launches exceeding ~10,000 kg.

6. **The Decision Tree Classifier was the best-performing model** with a validation accuracy of 0.8625 and a test accuracy of 0.8333, shared across all four evaluated models.

7. **All SpaceX launch sites share key geographical traits** — proximity to coastlines and equatorial latitudes — which helps optimize trajectory efficiency and booster recovery.

---

## 🔗 Notebook Links

| Lab | Link |
|---|---|
| Data Collection — SpaceX API | [Notebook](https://github.com/Avik-Das-567/IBM-Applied-Data-Science-Capstone/blob/main/Module%201%20-%20Introduction/1.%20Data%20Collection%20API%20Lab/data-collection-api-lab.ipynb) |
| Data Collection — Web Scraping | [Notebook](https://github.com/Avik-Das-567/IBM-Applied-Data-Science-Capstone/blob/main/Module%201%20-%20Introduction/2.%20Data%20Collection%20with%20Web%20Scraping%20lab/data-collection-with-web-scraping.ipynb) |
| Data Wrangling | [Notebook](https://github.com/Avik-Das-567/IBM-Applied-Data-Science-Capstone/blob/main/Module%201%20-%20Introduction/3.%20Data%20Wrangling/data-wrangling.ipynb) |
| EDA with SQL | [Notebook](https://github.com/Avik-Das-567/IBM-Applied-Data-Science-Capstone/blob/main/Module%202%20-%20Exploratory%20Data%20Analysis/1.%20EDA%20with%20SQL/eda-with-sql.ipynb) |
| EDA with Visualization | [Notebook](https://github.com/Avik-Das-567/IBM-Applied-Data-Science-Capstone/blob/main/Module%202%20-%20Exploratory%20Data%20Analysis/2.%20EDA%20with%20Visualization%20Lab/eda-with-visualization.ipynb) |
| Interactive Map with Folium | [Notebook](https://github.com/Avik-Das-567/IBM-Applied-Data-Science-Capstone/blob/main/Module%203%20-%20Interactive%20Visual%20Analytics%20%26%20Dashboards/1.%20Interactive%20Visual%20Analytics%20with%20Folium%20lab/interactive-visual-analytics-with-folium.ipynb) |
| Plotly Dash Dashboard | [Script](https://github.com/Avik-Das-567/IBM-Applied-Data-Science-Capstone/blob/main/Module%203%20-%20Interactive%20Visual%20Analytics%20%26%20Dashboards/2.%20Interactive%20Dashboard%20with%20Plotly%20Dash/spacex_dash_app.py) |
| Machine Learning Prediction | [Notebook](https://github.com/Avik-Das-567/IBM-Applied-Data-Science-Capstone/blob/main/Module%204%20-%20Predictive%20Analysis%20(Classification)/SpaceX-Machine-Learning-Prediction.ipynb) |

---

## 📄 Final Presentation

The full project presentation (`Final_Presentation_Avik.pdf`) is included in this repository and covers the executive summary, methodology, EDA insights, interactive analytics demos, and predictive analysis results end-to-end.

---

*This project was completed as part of the **IBM Data Science Professional Certificate** on Coursera.*
