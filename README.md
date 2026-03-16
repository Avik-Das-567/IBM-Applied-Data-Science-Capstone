# SpaceX Falcon 9 Landing Prediction | IBM Applied Data Science Capstone
Predicting whether the **SpaceX Falcon 9 first-stage booster will land successfully** using data science and machine learning techniques. This project demonstrates an end-to-end **data science pipeline**, including data collection, data wrangling, exploratory analysis, interactive visualization, dashboard development, and predictive modeling.

The project is part of the **IBM Applied Data Science Capstone**, where the objective is to generate data-driven insights that can help estimate rocket launch costs and support competitive launch bidding strategies.

## Project Background
SpaceX advertises Falcon 9 rocket launches for approximately **$62 million**, significantly cheaper than other providers that charge **$165 million** or more. The primary reason for this cost advantage is **first-stage booster reusability**.

Predicting whether the **first-stage booster will land successfully** is critical because reusable boosters drastically reduce launch costs.

This project aims to:
- Analyze historical Falcon 9 launch data
- Identify factors influencing landing success
- Build machine learning models to predict landing outcomes
- Provide insights into mission success patterns

## Project Workflow
The project follows a complete data science lifecycle:
```
Data Collection → Data Wrangling → Exploratory Data Analysis
→ Interactive Visual Analytics → Dashboard Development
→ Machine Learning Modeling → Insights & Conclusions
```
The methodology used throughout the project includes:
- API data collection
- Web scraping
- Data preprocessing and wrangling
- SQL analysis
- Data visualization
- Interactive dashboards
- Classification machine learning models

## Project Architecture
```
Raw Data Sources
│
├── SpaceX REST API
├── Wikipedia Web Scraping
│
↓
Data Processing (Pandas)
│
↓
Exploratory Data Analysis
│
├── SQL Analysis
├── Matplotlib / Seaborn Visualizations
│
↓
Interactive Analytics
│
├── Folium Geospatial Map
├── Plotly Dash Dashboard
│
↓
Machine Learning Models
│
├── Logistic Regression
├── SVM
├── Decision Tree
└── KNN
│
↓
Landing Success Prediction
```

## Data Collection

### SpaceX API Data Collection
Launch data was collected using the **SpaceX REST API**.

#### Steps:
1. Sent a **GET request** to retrieve historical launch data.
2. Converted **JSON responses** to **Pandas DataFrames**.
3. Selected relevant features from launch records.
4. Extracted additional information using IDs for:
    - Rocket
    - Launchpad
    - Payload
    - Booster core
5. Filtered dataset to include **Falcon 9 launches** only.
6. Replaced missing payload masses with the **column mean**.
7. Exported cleaned data as CSV.

The final dataset was saved as:
```
dataset_part_1.csv
```
### Web Scraping Launch Records

Additional launch data was collected by scraping the **Falcon 9 launch history table on Wikipedia**.

#### Tools Used:
- `requests`
- `BeautifulSoup`
- `pandas`

#### Process:
- Sent HTTP request to the Wikipedia page.
- Parsed HTML using BeautifulSoup.
- Extracted the **third HTML table containing launch records**.
- Extracted column headers and launch data.
- Constructed a dictionary to store extracted values.
- Converted dictionary into a Pandas DataFrame.
- Exported the dataset as:
  ```
  spacex_web_scraped.csv
  ```

## Data Wrangling
Data preprocessing was performed to prepare the dataset for analysis and modeling.

#### Steps:
1. Loaded the dataset and inspected data types and missing values.
2. Analyzed launch sites, orbit types, and mission outcomes.
3. Converted mission outcomes into a **binary classification variable**:
   ```
   Class = 1 → Successful Landing
   Class = 0 → Unsuccessful Landing
   ```
   **Bad outcomes included:** False ASDS, False Ocean, False RTLS, None ASDS, None None

4. Calculated overall success rate.
5. Exported cleaned dataset as:
    ```
    dataset_part_2.csv
    ```

## Exploratory Data Analysis (EDA)
EDA was conducted using:
- Pandas
- Matplotlib
- Seaborn

The objective was to identify relationships between **launch characteristics** and **landing success**.

### Key Visualizations

#### Flight Number vs Launch Site
Later launches show **higher landing success rates**, indicating improvements over time.

#### Payload Mass vs Launch Site
- Different launch sites handle varying payload ranges. 
- Some sites successfully land boosters with **very heavy payloads**.

#### Success Rate vs Orbit Type
Certain orbits such as **LEO** and **ISS** show higher success rates compared to others like **GTO**.

#### Flight Number vs Orbit Type
Success trends differ by orbit type.
- LEO shows improvement with time.
- GTO has mixed results across launches.

#### Payload Mass vs Orbit Type
Some orbit types successfully carry **large payloads while maintaining landing success**.

#### Launch Success Trend Over Time
Landing success improved dramatically between **2013** and **2020**, reaching close to **100% reliability** in later years.

## SQL-Based Analysis
SQL queries were used to perform additional analysis on launch data.

### Example Queries
- Retrieve unique launch sites
- Identify launch sites starting with **CCA**
- Calculate total payload mass for **NASA CRS missions**
- Compute average payload mass for **F9 v1.1**
- Identify the first successful ground landing date
- Determine boosters with successful drone ship landings
- Count mission success vs failure outcomes
- Identify boosters carrying maximum payload mass

#### Example query:
```
SELECT DISTINCT Launch_Site
FROM SPACEXTABLE;
```

## Interactive Visual Analytics

### Geospatial Analysis with Folium
Interactive maps were built using **Folium**.

Features included:
- Launch site markers
- Clustered launch outcomes
- Color-coded markers:
  - Green → Successful landing
  - Red → Failure
- Proximity analysis to:
  - Coastlines
  - Railways
  - Highways
  - Cities

Distance lines were drawn using **Folium PolyLine** objects to analyze geographic factors affecting launch site placement.

## Interactive Dashboard
An interactive dashboard was developed using **Plotly Dash**.

### Dashboard Components

#### Launch Site Dropdown
Allows selection of:
- All sites
- Individual launch sites

#### Pie Chart
Displays launch success counts per site.

#### Payload Range Slider
Filters launches by payload mass.

#### Scatter Plot
- Shows relationship between: **Payload Mass vs Landing Outcome**
- Colored by booster version category.

This dashboard enables interactive exploration of **launch performance data**.

## Machine Learning: Predictive Analysis

The final stage of the project involved building **classification models** to predict landing success.

### Data Preparation
- Target variable:
  ```
  Y = Class
  ```
- Feature scaling using:
  ```
  StandardScaler()
  ```
- Train/Test split:
  ```
  80% training
  20% testing
  ```

## Machine Learning Models
The following algorithms were evaluated:
- Logistic Regression
- Support Vector Machine (SVM)
- Decision Tree
- K-Nearest Neighbors (KNN)

### Hyperparameter Optimization
Grid search with **10-fold cross-validation** was used:
```
GridSearchCV(cv=10)
```

## Model Performance
- All models achieved similar accuracy: `Test Accuracy ≈ 0.8333 (83%)`
- Confusion matrices were used to analyze classification performance.

### Best Model

#### Logistic Regression
Accuracy: `0.8333`

## Key Insights
From exploratory analysis and modeling:
- Falcon 9 landing success rates improved significantly over time.
- Launch site experience contributes to higher success rates.
- Orbit type impacts landing probability.
- Payload mass influences mission outcomes but does not necessarily cause failure.
- Machine learning models can reasonably predict landing success using historical data.

## Technologies Used

- Programming Language: Python
- Libraries: Pandas, NumPy, Matplotlib, Seaborn, Scikit-learn, Plotly Dash, Folium, BeautifulSoup, Requests
- Tools: Jupyter Notebook, SQL

## Project Structure
```
IBM-Applied-Data-Science-Capstone

Module 1 – Introduction
   Data Collection API
   Web Scraping
   Data Wrangling

Module 2 – Exploratory Data Analysis
   EDA with SQL
   EDA with Visualization

Module 3 – Interactive Visual Analytics
   Folium Map
   Plotly Dash Dashboard

Module 4 – Machine Learning Prediction
   SpaceX ML Prediction Notebook

Final Presentation
```

## Future Improvements
Potential enhancements include:
- Incorporating additional launch features
- Using advanced ML models (Random Forest, XGBoost)
- Deploying the model as a web application
- Real-time launch data integration
- Expanding dataset with more recent launches

---
