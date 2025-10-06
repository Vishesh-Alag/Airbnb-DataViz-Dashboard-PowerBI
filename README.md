# 🏙️ Airbnb Chicago vs New Orleans — Data Visualization Dashboard (Power BI + Python)

### 🎯 Capstone Project — The DataViz Challenge: **"Transforming EDA Projects into Dashboards"**

This project demonstrates how to transform an exploratory data analysis (EDA) project into a compelling, interactive Power BI dashboard. By leveraging **Python (Google Colab)** for data cleaning and feature engineering, and **Power BI Desktop** for visual analytics, we compare Airbnb activity across two diverse cities — **Chicago** and **New Orleans**.

---

## 📌 Problem Statement

> How can Power BI be leveraged to perform a comprehensive and comparative visual analysis of Airbnb listings across Chicago and New Orleans?

This dashboard uncovers patterns in pricing, availability, host behavior, property types, and user reviews — giving valuable insights for decision-makers, investors, and Airbnb analysts.

---

## 🛠️ Tools & Technologies Used

| Tool                  | Purpose |
|------------------------|---------|
| **Python (Google Colab)** | Data cleaning, transformation, feature engineering |
| **Pandas, NumPy**      | Data manipulation |
| **Power BI Desktop**   | Dashboard design, visuals, DAX measures |
| **Power Query Editor** | Grouping, binning, text cleanup |
| **DAX**                | Custom measures and calculated columns |

---

## 🧹 Data Cleaning & Feature Engineering (Python)

The raw Airbnb listings datasets were cleaned and enhanced in **Google Colab**, using two files from [Inside Airbnb](http://insideairbnb.com/get-the-data/): one for **Chicago** and one for **New Orleans**.

### ✅ Key Cleaning Steps:
- Standardized column names and stripped whitespace
- Converted `last_review` to datetime
- Dropped missing and duplicate records
- Cleaned inconsistent neighborhood names
- Removed rows with invalid location or price

### 🧠 Feature Engineering:
- `host_type`: *Multi-Listing Host* or *Single-Listing Host*
- `price_category`: *Low*, *Medium*, *High*
- `reviewed_recently`: Flag for reviews in last 180 days
- Prepared for binning: `minimum_nights`, `price`, `number_of_reviews_ltm`

✅ Output: `cleaned_airbnb_combined.csv`

📓 View the full Python code here:  
[📎Google Colab Notebook](https://github.com/Vishesh-Alag/Airbnb-DataViz-Dashboard-PowerBI/blob/main/Python_Airbnb_Dashboard.ipynb)

---

## 🧾 Final Dataset Schema Used in Power BI

| Column                      | Description |
|-----------------------------|-------------|
| `name`, `city`, `neighbourhood` | Listing identifiers |
| `latitude`, `longitude`         | Location coordinates for map |
| `room_type`, `price`, `minimum_nights` | Booking and pricing info |
| `number_of_reviews`, `reviews_per_month` | Engagement and popularity |
| `calculated_host_listings_count` | Number of listings per host |
| `availability_365`, `number_of_reviews_ltm` | Availability and recency |
| `host_type`, `price_category`, `reviewed_recently` | Engineered features |
| `minimum_nights (bins)`, `Price Range`, `Review LTM Group` | Grouped bins (in Power BI) |

ℹ️ Binned fields were created in Power BI using **Power Query** and **DAX**.

---

## 📊 Dashboard Structure in Power BI

Dashboard contains button also which act as a navigation between different pages like overview, property analysis, hosting analysis and pricing analysis.
you can use it by clciking CTRL + clicking on the button of that page you want to navigate.

The dashboard is divided into **4 main sections**:

### 1️⃣ Overview
- Total listings and reviews
- City-wise breakdown
- Monthly price trends
- Room type share
- Geo-mapped listings

### 2️⃣ Property Analysis
- Listings by neighborhood and city
- Property & room type shares
- Availability trends
- Map of coordinates
- Matrix: neighborhood × room type

### 3️⃣ Pricing Analysis
- Avg price by host type and room type
- Top 10 expensive neighborhoods
- Price-to-review value visuals
- Price bands using decomposition tree
- Price range and occupancy correlation

### 4️⃣ Host Analysis
- Host type share (%)
- Avg price and reviews by host type
- Matrix: host vs room type
- Review trends by host type
- Availability vs host type

---

## 📐 DAX Measures Used / KPI's 

To support the dashboard, I created several DAX measures and calculated columns for better insights:

- % of Total Listings → Contribution of listings relative to all records
  `% of Total Listings = 
DIVIDE(
    COUNTROWS(cleaned_airbnb_combined),
    CALCULATE(COUNTROWS(cleaned_airbnb_combined), ALL(cleaned_airbnb_combined))
)`

- Avg Annual Availability → Average availability in days across listings
  `Avg Annual Availability = 
FORMAT(AVERAGE(cleaned_airbnb_combined[availability_365]), "0") & " days"`

- Avg Daily Price → Average listing price per night
  `Avg Daily Price = 
FORMAT(AVERAGE(cleaned_airbnb_combined[price]), "$#,##0")`

- Avg Monthly Reviews → Average number of reviews per month
  `Avg Monthly Reviews = 
FORMAT(AVERAGE(cleaned_airbnb_combined[reviews_per_month]), "0.0") & "/mo"`

- Dates Table → Calendar table based on last_review for time intelligence
  `Dates = 
CALENDAR(
    MIN(cleaned_airbnb_combined[last_review]),
    MAX(cleaned_airbnb_combined[last_review])
)`

- Host Type Breakdown → Comparison of multi-listing vs single-listing hosts
  `Host Type Breakdown = 
VAR TotalListings = COUNTROWS(cleaned_airbnb_combined)
VAR MultiHosts = 
    CALCULATE(
        COUNTROWS(cleaned_airbnb_combined),
        cleaned_airbnb_combined[host_type] = "Multi-Listing Host"
    )
VAR SingleHosts = TotalListings - MultiHosts
RETURN
    "Multi: " & MultiHosts & " (" & FORMAT(DIVIDE(MultiHosts, TotalListings), "0%") & ")" &
    UNICHAR(10) &
    "Single: " & SingleHosts & " (" & FORMAT(DIVIDE(SingleHosts, TotalListings), "0%") & ")"`

- Interpretation → Price positioning (Above Market / Below Market / Around Median)
  `Interpretation = 
SWITCH(
    TRUE(),
    [Price Index] > 1.2, "Above Market",
    [Price Index] < 0.8, "Below Market",
    "Around Median"
)`

- Listings Count → Total listings count
  `Listings Count = COUNTROWS(cleaned_airbnb_combined)`

- MinNights Selected → Captures the selected minimum nights from slicers
  `MinNights Selected = SELECTEDVALUE(cleaned_airbnb_combined[minimum_nights])`

- Occupancy Rate → Availability-adjusted occupancy metric
  `Occupancy Rate = 
1 - DIVIDE(
    AVERAGE(cleaned_airbnb_combined[availability_365]),
    365
)`

- Price Index → Relative comparison of listing price to city median
  `Price Index = 
VAR MedianPrice =
    CALCULATE(
        MEDIAN(cleaned_airbnb_combined[price]),
        ALLEXCEPT(cleaned_airbnb_combined, cleaned_airbnb_combined[city])
    )
RETURN
    DIVIDE(AVERAGE(cleaned_airbnb_combined[price]), MedianPrice)`

- Price Range → Binned price categories
  `Price Range = 
SWITCH(
    TRUE(),
    cleaned_airbnb_combined[price] <= 50, "0 - 50",
    cleaned_airbnb_combined[price] <= 100, "51 - 100",
    cleaned_airbnb_combined[price] <= 150, "101 - 150",
    cleaned_airbnb_combined[price] <= 200, "151 - 200",
    cleaned_airbnb_combined[price] <= 300, "201 - 300",
    cleaned_airbnb_combined[price] <= 500, "301 - 500",
    cleaned_airbnb_combined[price] <= 1000, "501 - 1000",
    "> 1000"
)`

- Review LTM Group → Review volume categories for the last 12 months
  `Review LTM Group = 
SWITCH(
    TRUE(),
    cleaned_airbnb_combined[number_of_reviews_ltm] <= 50, "0-50",
    cleaned_airbnb_combined[number_of_reviews_ltm] <= 100, "51-100",
    cleaned_airbnb_combined[number_of_reviews_ltm] <= 150, "101-150",
    cleaned_airbnb_combined[number_of_reviews_ltm] <= 200, "151-200",
    cleaned_airbnb_combined[number_of_reviews_ltm] <= 300, "201-300",
    cleaned_airbnb_combined[number_of_reviews_ltm] <= 450, "301-450",
    "451+"
)`

- Review Rate → Ratio of recently reviewed listings to all listings
  `Review Rate = 
DIVIDE(
    CALCULATE(COUNTROWS(cleaned_airbnb_combined), cleaned_airbnb_combined[reviewed_recently] = "Yes"),
    COUNTROWS(cleaned_airbnb_combined)
)`

- Superhost % → Share of multi-listing hosts (proxy for "superhost")
  `Superhost % = 
DIVIDE(
    COUNTROWS(FILTER(cleaned_airbnb_combined, cleaned_airbnb_combined[calculated_host_listings_count] > 1)),
    COUNTROWS(cleaned_airbnb_combined)
)`

- Superhost % with Icon → Same metric with ★ icon formatting
  `Superhost % with Icon = 
VAR Percentage = [Superhost %]
RETURN
IF(
    Percentage > 0,
    UNICHAR(9733) & " " & FORMAT(Percentage, "0%"),
    FORMAT(Percentage, "0%")
)`

- Value Score → Composite metric: (reviews per month ÷ price) × 100
  `Value Score = 
VAR AvgReviews = AVERAGE(cleaned_airbnb_combined[reviews_per_month])
VAR Price = AVERAGE(cleaned_airbnb_combined[price])
RETURN
    IF(
        Price > 0,
        DIVIDE(AvgReviews, Price, 0) * 100,
        BLANK()
    )`

---

## 🚀 How to Run the Project

### ✅ Prerequisites:
- Power BI Desktop installed
- Google Colab access (for Python preprocessing)

### 🔍 1. Explore the Power BI Dashboard

You can directly download and open the Power BI dashboard file to explore all visuals and filters:

📥 Download: Airbnb_Dashboard.pbix

    🧰 Open it using Power BI Desktop (available for free on Windows).

Once opened, you can:

    View all 4 dashboard pages (Overview, Property, Pricing, Host)

    Use slicers to filter by city, host type, room type, and price

    Explore DAX measures, calculated fields, and transformed data in the “Data” view

### 2. Run Python Script on Google Colab (Optional)
Open the notebook here:
📓 [Colab Notebook](https://github.com/Vishesh-Alag/Airbnb-DataViz-Dashboard-PowerBI/blob/main/Python_Airbnb_Dashboard.ipynb)
- Upload both raw CSV files (`airbnb_chicago_listings.csv`, `airbnb_new_orleans_listings.csv`)
- Run the notebook to generate `cleaned_airbnb_combined.csv`

### 3. Open Power BI Dashboard
- Launch `Airbnb.pbix` in Power BI Desktop
- All visuals and filters will be loaded automatically
- Use slicers for city, room type, host type, and price category

---

## 📦 Accessing the Final Transformed Dataset

While the base dataset is cleaned in Python, the final Power BI dashboard includes additional columns and bins created via Power BI.

If you'd like to access the full transformed dataset:
1. Open the `.pbix` file in Power BI Desktop
2. Go to the "Data" view (left sidebar)
3. Right-click the table → Export data or choose Copy Table
4. Save as CSV or Excel workbook(`final_powerbi_dataset.csv`) or (`final_powerbi_dataset.xlsx`)

This allows you to explore fields like:
- `minimum_nights (bins)`
- `Price Range`
- `Review LTM Group`
- DAX-calculated fields like: Occupancy Rate, % of Listings, Avg Price, etc.

---

## 📷 Dashboard Snapshots

<img width="1917" height="1021" alt="airbnbdash1" src="https://github.com/user-attachments/assets/fa9f6ec4-3f02-4e26-bbc8-0023da57b5d1" />
<img width="1918" height="998" alt="airbnbdash2" src="https://github.com/user-attachments/assets/c0e2c585-e34a-47b8-9b9b-535f75b6289f" />
<img width="1916" height="1018" alt="airbnbdash3" src="https://github.com/user-attachments/assets/647de22d-2049-43ab-9ef6-5e938cb54e79" />
<img width="1918" height="1015" alt="airbnbdash4" src="https://github.com/user-attachments/assets/04e7ea4f-984a-4852-8bb6-47f8013d582b" />




---

## ✨ Key Takeaways

✅ Real-world data cleaning and transformation in Python  
✅ Powerful interactive dashboarding using Power BI  
✅ Application of bins, DAX, decomposition trees, slicers  
✅ Strong portfolio project for Data Analyst/Scientist roles  

---

# Airbnb Data Analysis - Chicago vs New Orleans

## 📈 Summary of Findings

These findings are based on default views of the Power BI dashboard without applying any slicers or filters. They represent general trends across both cities, but further exploration using filters (e.g., by room type, host type, city, or price range) may reveal deeper insights.
This Airbnb data visualization project provides a comparative analysis of Chicago and New Orleans, offering insights into listing trends, pricing strategies, and host behavior. Below are the key takeaways from the Power BI dashboard:

## 🗺️ Overview Insights

- A total of **11,696 listings** were analyzed: Chicago had around **6,100 listings** and New Orleans around **5,600**.
- The two cities combined had over **880,000 reviews**, showing strong guest engagement.
- The average price across all listings was approximately **$286**, with monthly fluctuations observed in both cities.
- The majority of listings (over **83%**) were **Entire home/apartment**, followed by Private rooms and Hotel rooms.
- Review volume peaked in **summer months**, especially from **May to August**, aligning with tourism trends.
- Around **68% of hosts** had multiple listings, indicating a strong presence of commercial hosts or professional property managers.

## 🏘️ Property Analysis

- The listing distribution was nearly equal between both cities: **Chicago (52.4%)**, **New Orleans (47.5%)**.
- Key neighborhoods like **West Town (Chicago)** and **Treme-Lafitte (New Orleans)** had high listing concentrations.
- The average availability was highest for **Hotel rooms (237 days)**, and lowest for **Shared rooms (170 days)**.
- Visuals highlighted a strong correlation between neighborhood and property type, with **"Entire homes"** dominating across most areas.

## 💰 Pricing Analysis

- **Hotel rooms** were the most expensive (**avg $414**), followed by **Entire homes ($315)** and **Private rooms ($127)**.
- Properties with recent reviews were priced lower on average **($275)** than those without recent reviews **($311)**, indicating a possible link between affordability and review frequency.
- **Top 10 most expensive neighborhoods** were mostly from **New Orleans**, with average prices exceeding **$1,000**.
- A **decomposition tree** revealed pricing differences based on host type, city, and room type, emphasizing how multiple factors influence rates.
- Price bands showed that the majority of listings fall under the **Low to Medium price category**.

## 👤 Host Analysis

- **Multi-listing hosts** made up nearly **68% of total listings** and had higher average pricing **($263)** compared to **Single-listing hosts ($234)**.
- Multi-listing hosts also tended to own more **Hotel and Entire home/apt properties**, suggesting a professional or commercial model.
- Despite pricing differences, the **average review rate per month** was similar for both host types (around **1.8 reviews/month**).
- **Availability** was also higher among multi-listing hosts **(214 days)** than single-listing hosts **(174 days)**, indicating more actively managed properties.

## Conclusion

These insights enable Airbnb stakeholders to make informed decisions around pricing strategies, neighborhood targeting, and host performance in two vibrant and highly competitive urban markets.

## 📌 Tags

`#PowerBI` `#Airbnb` `#EDA` `#Python` `#GoogleColab` `#DAX` `#DataVisualization` `#CapstoneProject` `#DashboardDesign` `#FeatureEngineering`

---

## 🙋‍♂️ Author

**Vishesh Alag**  
Aspiring Data Analyst / Data Scientist  
📫 [LinkedIn](https://www.linkedin.com/in/alaghvishesh03/)
