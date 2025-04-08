# Hyundai Automotive Analysis
*by Joohee Yoon*  


## Overview
1) Market Share Trends  
2) Profitability Outlook  
3) Vehicle Sales Predictions  
4) Individual Vehicle Sales Prediction  
5) Source Codes  

### Dataset Source
The dataset for this analysis was scraped from [hyundainews.com](https://www.hyundainews.com) using a dedicated script (available at the end). One figure, however, was sourced from the web, and the link is provided in the figure's footer.

## BLUF (Bottom Line Up Front)
- **Increased Sales Expected**: Hyundai-Kia Motor Group is projected to sell more vehicles in the coming year, although operating margins are likely to decline.  
- **Sales Volume Growth**: The majority of Hyundai’s vehicle lineup is showing an upward trend in sales volume.  
- **Profitability Decline**: Profitability is expected to decrease compared to last year, as the unusually high profit margins from the previous year were driven by a favorable sales-to-inventory ratio.

## Market Share Trends
- **US Automotive Market Value**: The US automotive market is valued at $1.62 trillion in 2024.  
- **Consistent Market Share**: Hyundai-Kia Auto Group has maintained a market share above 10% in recent years.  
- **Fluctuating Trend**: Market share has shown some fluctuations, with minor increases and decreases, but overall, it remains relatively stable in the past few years.

## Profitability
- **Unusually High Profits Post-COVID**: Automakers experienced higher-than-usual profitability during the COVID-19 pandemic, driven by supply chain disruptions and low vehicle inventory, which resulted in favorable sales-to-inventory ratios.  
- **Supply Chain Impact**: Limited vehicle supply led to increased demand, allowing automakers to command higher prices and achieve greater margins.  
- **Return to Pre-COVID Norms**: As supply chains stabilize and inventory levels return to pre-pandemic values, sales-to-inventory ratios are normalizing.  
- **Expect Profitability to Normalize**: Given the return to typical inventory levels, profit margins are expected to return to more sustainable, pre-COVID levels in the coming years.

### Figure: US Auto Industry Inventory to Sales Ratio
*(Include the figure here, or link to it if hosted online)*

## Choice of Linear Regression Model
The linear regression model was selected due to its simplicity and ease of interpretation. While more complex models could potentially offer higher accuracy, the primary focus of this presentation is on demonstrating my ability to implement and showcase a fundamental model, rather than maximizing prediction accuracy or exploring more advanced techniques.

## Vehicle Sales Prediction
- **Sales Prediction for May 2025**: Linear regression suggests that 74,691 vehicles will be sold in May 2025.  
- **Standard Deviation**: The standard deviation of historical data is 9,034.39, indicating the potentially large variability in sales numbers.  
- **Regression Line Slope**: The regression line slope is positive (0.00016392), suggesting a trend of vehicle sales growth over time.

### Figure: Total Number of Vehicles Sold by Month
*(Include the figure here, or link to it if hosted online)*

## Individual Vehicle Sales Prediction for May 2025
- **Elantra**: 12,434 vehicles  
- **Kona**: 6,997 vehicles  
- **Palisade**: 9,755 vehicles  
- **Santa Cruz**: 2,438 vehicles  
- **Santa Fe**: 10,903 vehicles  
- **Sonata**: 5,913 vehicles  
- **Tucson**: 19,385 vehicles  
- **Venue**: 1,965 vehicles  
- **Ioniq 5**: 4,185 vehicles  
- **Ioniq 6**: 906 vehicles

### Figure: Total Number of Vehicles Sold by Month
*(Include the figure here, or link to it if hosted online)*

## Source Codes Overview

### DataScraping.py:
- **Purpose**: Scrapes sales-related data from the Hyundai News website and saves it into a dataset for further analysis.  
- **Key Functions**: Web scraping, data extraction, data storage.

#### DataAnalysis.py:
- **Purpose**: Handles data preprocessing, cleaning, modeling, and prediction.  
- **Key Functions**: Data cleaning, feature engineering, statistical analysis, machine learning model training, sales predictions.

### DataAnalysis.py Code:
python
import pandas as pd
import seaborn as sns
import matplotlib.ticker as ticker
from matplotlib import pyplot as plt
import numpy as np
from sklearn.linear_model import LinearRegression
from datetime import datetime
import math

total_sales_df = pd.read_csv("data/total_sales_df.csv")
total_vehicle_sales_df = pd.read_csv("data/total_vehicle_sales_df.csv")

y = total_sales_df['total sales']
x = total_sales_df['timestamp']

# Some confidence interval
ci = 3 * np.std(y)  # 3 sigma

print(np.std(y))

fig, ax = plt.subplots()

# Train model
x_dt = x.apply(lambda q: datetime.strptime(q, "%Y-%m-%d"))
x_np = x_dt.apply(lambda q: q.timestamp()).to_numpy().reshape(-1, 1)
reg = LinearRegression().fit(x_np, y)

#DataScraping.py Code: 
import requests
from bs4 import BeautifulSoup
import pandas as pd

# Step 1: Set a User-Agent to mimic a browser request
headers = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36'
}

# Step 2: Initialize a session
session = requests.Session()
session.headers.update(headers)

# Step 3: Fetch the main page containing article links
base_url = "https://www.hyundainews.com"
query_result_url = base_url + "/en-us/releases?category_ids=382&categories=sales+releases&page_title=sales_releases"
response = session.get(query_result_url)

# Step 4: Parse the main page with BeautifulSoup
soup = BeautifulSoup(response.text, 'html.parser')

# Find all the article links on the page (assuming they are <a> tags)
article_links = []
