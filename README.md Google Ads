# :bar_chart:**Google Ads Analytics Project**:bar_chart:

_Python Data Processing + Looker Studio Visualization_

This project demonstrates my ability to clean, transform, and prepare marketing performance data using Python, then visualize insights using Looker Studio

:pencil2:**1.PROJECT DESCRIPTION**  
This repository contains the Python codes used to process raw Google Ads data before sending it into Looker Studio.
The main goals:  
-Data ingestion from csv to pandas  
-Identification data problems  
-Convert inconsistent & wrong data format  
-Standardize campaign name  
-Identification & remove outlier  
-Calculate performance metric  
-Export cleaning data for dashboard process

:file_folder:**2. REPOSITORY STRUCTURE**  
/data  
├─ GoogleAds_DataAnalytics_Sales_Uncleaned.csv  
└─ Googleads_clean.csv  
/notebooks  
└─ Google Ads Analytics.ipynb  
README.md

:books:**3. PYTHON WORKFLOW OVERVIEW**  
:red_circle:**Import & Inspecting Data**  
import pandas as pd  
import numpy as np  
import matplotlib.pyplot as plt  
import seaborn as sns  

```Python
path = r"C:\Users\Nur Aini Mahfud\Downloads\GoogleAds_DataAnalytics_Sales_Uncleaned.csv"  
df = pd.read_csv(path)  
df

df.shape

df.info()

df_clean = df.copy()  
df_clean
```
:red_circle:**Cleaning Data Format**  
**Change data type for Cost & Sale_Amount column**  
```Python
for col in ['Cost', 'Sale_Amount']:
    df_clean[col] = (
        df_clean[col]
        .replace(r'[\$,USD,\s]', '', regex=True)
        .replace('', pd.NA).astype(float
)  

df_clean['Ad_Date'] = pd.to_datetime(df_clean['Ad_Date'], errors='coerce')  
```

:red_circle:**Identification data duplicates**  
```Python
df_clean.duplicated().any()
```  

:red_circle:**Identification Problems by Unique Values & Fixing It**  
```Python
print('Campaign_Name unique:', df_clean['Campaign_Name'].unique())  
print('Location unique:', df_clean['Location'].unique())  
print('Keyword unique:', df_clean['Keyword'].unique())  
print('Ad_Date unique:', df_clean['Ad_Date'].unique())  

campaign_mapping = {
    'DataAnalyticsCourse': 'Data Analytics Course',
    'Data Anlytics Corse': 'Data Analytics Course',
    'Data Analytcis Course': 'Data Analytics Course',
    'Data Analytics Corse': 'Data Analytics Course',
}  

df_clean['Campaign_Name'] = df_clean['Campaign_Name'].replace(campaign_mapping)  

campaign_mapping2 = {
    'hyderabad': 'hyderabad',
    'HYDERABAD': 'hyderabad',
    'Hyderbad': 'hyderabad', 
    'hydrebad': 'hyderabad',
}  

df_clean['Location'] = df_clean['Location'].replace(campaign_mapping2)  

df_clean['Device'] = df_clean['Device'].str.lower()  
```

:red_circle:**Handling Missing Values**  
_handling missing values for 'Ad_Date' column with randome date_  
```Python
df_clean['Ad_Date'] = pd.to_datetime(df_clean['Ad_Date'], errors='coerce')
all_dates = pd.date_range(start='2024-01-01', end='2024-12-31')
missing_idx = df_clean[df_clean['Ad_Date'].isna()].index
random_dates = np.random.choice(all_dates, size=len(missing_idx), replace=True)
df_clean.loc[missing_idx, 'Ad_Date'] = random_dates
df_clean['Ad_Date'] = df_clean['Ad_Date'].dt.strftime('%d-%m-%Y')
df_clean  
```

_handling missing values for 'Clicks, Leads, Conversions, Sale_Amount, Impressions, & Costs' with median values_  
```Python
cols = ['Clicks', 'Leads', 'Conversions', 'Sale_Amount', 'Impressions', 'Cost']

for col in cols:  
````if col in df_clean.columns:
''''''''median_val = df_clean[col].median()
        df_clean[col] = df_clean[col].fillna(median_val)  
```

_handling missing values for 'Conversion Rate' column with following formula_  
```Python
mask = df_clean['Conversion Rate'].isna()
df_clean.loc[mask, 'Conversion Rate'] = np.where(
    df_clean.loc[mask, 'Clicks'] > 0,
    df_clean.loc[mask, 'Conversions'] / df_clean.loc[mask, 'Clicks'],
    np.nan
)  
```

:red_circle:**Detection Outlier with Boxplot for Numeric Column**  
```Python
numeric_df = df_clean.select_dtypes(include=['float','int'])
plt.figure(figsize=(10, 5))
sns.boxplot(data=numeric_df)
plt.title("Outlier detection_all numeric columns")
plt.show()  
```

:red_circle:**Adjust Format Date for Looker**
```Python
df_clean['Ad_Date'] = pd.to_datetime(df_clean['Ad_Date'], format='%d-%m-%Y')  
```

:red_circle:**Save Cleaned Data**  
```Python
df_clean.to_csv('googleads_clean.csv', index=False)  
```

**:mag_right:DASHBOARD OVERVIEW** 
<img width="987" height="768" alt="GoogleAdsDashboard3" src="https://github.com/user-attachments/assets/31a94120-3763-4481-8d07-c3a876c526ca" />

(open live dashboard) [https://lookerstudio.google.com/reporting/effc0022-b699-4ee5-806d-3813185cb4bf]  

:chart_with_upwards_trend: **4.INSIGHT DASHBOARD**  
_for December 2024 period_  
:one:**Click dropped(20.6K,-85.5%)**, **Impressions dropped(664.6K, -85.6%)**, **CTR Improved(3.10%, +0.7%)** :arrow_right: ads reached fewer users but with higher relevance, resulting in more efficient and engaged traffic.  
:two:**Conversions dropped (1,005, -84.8%)**, **CVR Increased (4.88%, +4.7%)**, **Cost per conversions dropped ($32.21, -2.5%)** :arrow_right: Conversions decreased, but higher CVR and lower cost per conversion show that fewer users came in, yet they were more qualified and converted more efficiently.  
:three:**Cost decreased ($32.4k, -85.2%)**, **Avg.CPC increased ($1.57, +2.1%)**, **Avg.CPM($48.70, +2.8%)**, :arrow_right: Total costs dropped significantly, meaning advertising budgets were significantly reduced. However, CPC and CPM rose slightly, indicating increased advertising competition, making the remaining traffic slightly more expensive.  
:four:**The line chart of clicks & CTR shows a fluctuating trend and CTR increases at the end of the month, indicating that the traffic quality is getting better.**  
:five:**Conversions were stable, but cost per conversion dropped at the end of the month, indicating increased efficiency.**  
:six:**Costs decreased at the end but the CPC was quite stable, indicating that spending was quite controlled.**  
:seven:**For the top campaign, all relevant ads had stable CTR values, ranging from 5-6%. The average CPS for all campaigns was below 2%. All ads performed well, with no outliers.**  
:eight:**While most traffic comes from mobile, desktop has a higher conversion rate. Consider allocating a slightly larger budget to desktop for optimal results..**  

:toolbox:**5.TECH STACK**  
	:ballot_box_with_check:1. Python (Pandas, Matplotlib, Numpy, Seaborn)  
  	:ballot_box_with_check:2. Jupiter Notebook  
    	:ballot_box_with_check:3. Looker Studio  
      	:ballot_box_with_check:4. Github (Documentation)  

:key:**6.KEY LEARNING**   
:heavy_check_mark: Learn to clean and prepare data.  
:heavy_check_mark:Analyze patterns and performance across devices.  
:heavy_check_mark:Read key metrics like CTR, CVR, and cost.  
:heavy_check_mark:Create visualizations to understand data.  
:heavy_check_mark:Construct insights in a concise and structured manner.



















