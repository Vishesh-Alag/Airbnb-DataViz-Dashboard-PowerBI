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
you can use it by clciking CTRIL + clicking on the button of that page you want to navigate.

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

*Add your dashboard screenshots here*

---

## ✨ Key Takeaways

✅ Real-world data cleaning and transformation in Python  
✅ Powerful interactive dashboarding using Power BI  
✅ Application of bins, DAX, decomposition trees, slicers  
✅ Strong portfolio project for Data Analyst/Scientist roles  

---

## 📌 Tags

`#PowerBI` `#Airbnb` `#EDA` `#Python` `#GoogleColab` `#DAX` `#DataVisualization` `#CapstoneProject` `#DashboardDesign` `#FeatureEngineering`

---

## 🙋‍♂️ Author

**Vishesh Alag**  
Aspiring Data Analyst / Data Scientist  
📫 [LinkedIn](https://www.linkedin.com/in/alaghvishesh03/)
