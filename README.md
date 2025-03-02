
# **COVID-19 Data Analysis**

## **Project Overview**
This project focuses on analyzing **global COVID-19 data**, identifying trends, and automating data visualization for confirmed, active, recovered, and death cases. The dataset includes global and country-level information sourced from **Worldometer and WHO**. The analysis is performed using **Python, Pandas, Matplotlib, Seaborn, and Plotly**.

## **Dataset**
### **1. Full COVID-19 Dataset**
- **Source:** `full_grouped.csv`
- **Features:**
  - `Date`: The date of data recording.
  - `Country/Region`: Country name.
  - `Confirmed`: Total confirmed cases.
  - `Deaths`: Total deaths reported.
  - `Recovered`: Total recoveries.
  - `Active`: Active COVID-19 cases.
  - `New cases`, `New deaths`, `New recovered`: Daily changes.
  - `WHO Region`: The WHO-defined region.

### **2. Worldometer COVID-19 Dataset**
- **Source:** `worldometer_data.csv`
- **Features:**
  - `Country/Region`, `Continent`, `Population`
  - `TotalCases`, `NewCases`, `TotalDeaths`, `NewDeaths`, `TotalRecovered`, `NewRecovered`
  - `ActiveCases`, `Serious/Critical`
  - `TotalTests`, `Tests/1M pop`

## **Methodology**
### **1. Importing Data**
```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import plotly.graph_objects as go
import plotly.express as px

# Load datasets
data_global = pd.read_csv('full_grouped.csv')
data_worldometer = pd.read_csv('worldometer_data.csv')
```

### **2. Data Analysis & Exploration**
#### **2.1 Global Trends Analysis**
- Visualized the **global trend of confirmed, deaths, recovered, and active cases** over time.
```python
fig = px.line(data_global, x="Date", y=["Confirmed","Deaths","Recovered","Active"],
              title="COVID-19 Trends Over Time", template="plotly_dark")
fig.show()
```

#### **2.2 Top 20 Most Affected Countries**
- Identified the **20 worst-hit countries** using bar charts.
```python
fig = px.bar(data_worldometer.iloc[0:20], x='Country/Region',
             y=['TotalDeaths','TotalRecovered','ActiveCases','TotalCases'],
             template="plotly_dark")
fig.update_layout(title="Coronavirus: 20 Most Affected Countries")
fig.show()
```

#### **2.3 Top 10 Countries with Most Cases**
- Identified countries with the highest confirmed cases.
```python
fig = px.bar(data_worldometer.sort_values(by='TotalCases', ascending=False)[0:10],
             x='TotalCases', y='Country/Region', color='TotalCases', text="TotalCases")
fig.update_layout(title="Top 10 Countries with Total Confirmed Cases", template="plotly_dark")
fig.show()
```

#### **2.4 Top 10 Countries with Highest Deaths**
- Visualized the **countries with the most reported deaths**.
```python
fig = px.bar(data_worldometer.sort_values(by='TotalDeaths', ascending=False)[0:10],
             x='TotalDeaths', y='Country/Region', color='TotalDeaths', text="TotalDeaths")
fig.update_layout(title="Top 10 Countries with Highest Deaths", template="plotly_dark")
fig.show()
```

#### **2.5 Top 10 Countries with Highest Recoveries**
- Identified countries with the **most recovered cases**.
```python
fig = px.bar(data_worldometer.sort_values(by='TotalRecovered', ascending=False)[0:10],
             x='TotalRecovered', y='Country/Region', color='TotalRecovered', text="TotalRecovered")
fig.update_layout(title="Top 10 Countries with Highest Recoveries", template="plotly_dark")
fig.show()
```

### **3. Ratio Analysis**
#### **3.1 Deaths to Confirmed Ratio**
```python
deaths_to_confirmed = data_worldometer['TotalDeaths'] / data_worldometer['TotalCases']
fig = px.bar(data_worldometer, x='Country/Region', y=deaths_to_confirmed)
fig.update_layout(title="Deaths to Confirmed Cases Ratio", template="plotly_dark")
fig.show()
```

#### **3.2 Serious to Deaths Ratio**
```python
serious_to_death = data_worldometer['Serious,Critical'] / data_worldometer['TotalDeaths']
fig = px.bar(data_worldometer, x='Country/Region', y=serious_to_death)
fig.update_layout(title="Serious to Death Ratio", template="plotly_dark")
fig.show()
```

### **4. Automated Country-Specific Visualization**
#### **Function to Generate COVID-19 Trend for Any Country**
```python
from plotly.subplots import make_subplots
import plotly.graph_objects as go

def country_visualization(data, country):
    df = data[data['Country/Region'] == country]
    df = df[['Date', 'Confirmed', 'Deaths', 'Recovered', 'Active']]
    
    fig = make_subplots(rows=1, cols=4, subplot_titles=("Confirmed", "Active", "Recovered", "Deaths"))
    
    fig.add_trace(go.Scatter(name="Confirmed", x=df['Date'], y=df['Confirmed']), row=1, col=1)
    fig.add_trace(go.Scatter(name="Active", x=df['Date'], y=df['Active']), row=1, col=2)
    fig.add_trace(go.Scatter(name="Recovered", x=df['Date'], y=df['Recovered']), row=1, col=3)
    fig.add_trace(go.Scatter(name="Deaths", x=df['Date'], y=df['Deaths']), row=1, col=4)
    
    fig.update_layout(height=600, width=1000, title=f"COVID-19 Cases in {country}", template="plotly_dark")
    fig.show()

# Example usage
country_visualization(data_global, 'USA')
```

## **Findings & Insights**
- **USA, Brazil, and India** were among the top three most affected countries.
- **The death-to-confirmed ratio varied by country**, indicating differences in healthcare quality and pandemic response.
- **Countries with higher testing rates** had better reported recovery rates.
- **Automating country-specific analysis** allows for better monitoring of COVID-19 trends in individual regions.

## **Challenges & Limitations**
- **Data Inconsistencies:** Some country reports had missing data.
- **Unreported Cases:** COVID-19 figures depended on the accuracy of testing and reporting by countries.
- **Lack of External Factors:** Economic impact and healthcare response effectiveness were not included in this dataset.

## **Future Improvements**
- **Incorporate Vaccination Data:** Analyze the impact of vaccinations on COVID-19 trends.
- **Sentiment Analysis:** Use social media sentiment analysis to study public response.
- **Predictive Modeling:** Apply time-series forecasting models to predict future COVID-19 trends.

## **Required Libraries**
To run this project, install the necessary Python libraries:
```bash
pip install numpy pandas matplotlib seaborn plotly
```

## **References**
- [Worldometer COVID-19 Data](https://www.worldometers.info/coronavirus/)
- [WHO Coronavirus Dashboard](https://covid19.who.int/)

