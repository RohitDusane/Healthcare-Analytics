# Healthcare-Analytics Power BI Project

# Table of Contents

1. [Introduction](#1-introduction)
2. [Objectives & Report Structure/Requirement 📝](#2-objectives--report-structurerequirement-)
    - [Overall Objectives 🎯](#overall-objectives-)
    - [Metrics Requirement 📏](#metrics-requirement-)
    - [Views Requirement 📊](#views-requirement-)
3. [Data Sources](#3-data-sources)
    - [STEP I: Data Extraction 🗂](#step-i-data-extraction-)
    - [STEP II: Data Connecting - Folder Connector 📤](#step-ii-data-connecting---folder-connector-)
    - [STEP III: Data Transformation and Cleaning 🧹](#step-iii-data-transformation-and-cleaning-)
    - [STEP IV: Data Modelling 🔗](#step-iv-data-modelling-)
4. [Analysis and Insights 🧮](#4-analysis-and-insights-)
    - [Key Metrics](#key-metrics)
    - [Charts and Visuals](#charts-and-visuals)
    - [Filters and Slicers](#filters-and-slicers)
    - [Case Type Breakdown](#case-type-breakdown)
    - [Key Insights](#key-insights)
    - [March 2021 📅 Trends](#march-2021--trends)
5. [Conclusion](#5-conclusion)
6. [Contact Information](#6-contact-information)



## 1. Introduction
This dashboard provides insights into waiting times for outpatient and inpatient healthcare services, 
broken down by **Age Profile, Specialty, Time Bands and, and patient Case Type**.

## 2. Objectives & Report Structure/Requirement 📝
### Overall Objectives 🎯
1) Track current status of patient waiting list.
2) Analyze historical monthly trend of waiting list in Inpatients and Outpatients categories.
3) Detailed specialty level & age profile analysis,

### Metrics Requirement 📏
1) Average & Median Waiting List
2) Current Total Wait List
3) Add as per needed

### Views Requirement 📊
1) Summary Page![Dashboard - Summary](./Summary%20View.PNG)

2) Detailed Page for Granular Analysis![Dashboard - Summary](./Detailed%20View.PNG)

## 3. Data Sources

### **STEP I: Data Extraction 🗂**
The Healthcare data consisits of 2 types of categories viz. Inpatients (Admitted) & Outpatients (Same day) datasets.
Each data category folder has multiple **" .csv "** for each year from 2018 to 2021.

Here is the table for the data category and sample size:
| **Data Category** | **Sample Size** |
|-------------------|-----------------|
| Outpatient        | 270,983         |
| Inpatient         | 182,136         |
| Total Patients    | 453,119         |

### **STEP II: Data Connecting - Folder Connector 📤** 
IN PBI we will use the **FOLDER Connector** to collate the data from each folder into a single file and upload in PBI.

1. Open Power BI desktop and click on GET DATA and select FOLDER option. Click Connect button at the bottom right.
2. Now browse the folder path where we have stored **Inpatient data** and press OK.
3. The next prompt will show you the files available in the folder path. Then click on Combine button and select Combine & Load.
4. Next popup window will show you a preview of the data. If everything looks good then click on Ok.
5. Now repeat these 4 steps again for loading the **Outpatient data** into power bi.


| **Column Name**      | **Description**                                                                                                                                                    |
|----------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Archive Date**     | The date the record was archived, in the format `DD-MM-YYYY`.                                                                                                      |
| **Specialty HIPE**   | A numerical code representing the specialty.                                                                                                                       |
| **Speciality**       | The medical specialty or discipline (e.g., Dermatology, Diabetes Mellitus, etc.).                                                                                  |
| **Adult/Child**      | Indicates whether the record pertains to adults or children.                                                                                                       |
| **Age Profile**      | The age group the data pertains to, with ranges like `0-15`, `16-64`, etc.                                                                                         |
| **Time Bands**       | A time period associated with the record, showing specific months or ranges like `0-3 Months`, `3-6 Months`, `9-12 Months`, etc.                                   |
| **Total**            | The total count associated with that entry for the given specialty, age profile, and time band.                                                                    |


### **STEP III: Data Transformation and Cleaning 🧹**

1. **Renaming Columns**: We standardize column names between tables to ensure consistency, such as renaming the "Specialty" column in the Outpatient table to match the "Specialty_Name" column in the Inpatient table.

2. **Rearranging Columns**: The columns in the Outpatient table are reordered to match the structure of the Inpatient table. Missing columns, like "Case_Type," are added to Outpatient by creating a **Custom Column** with a formula.

3. **Appending Tables**: After ensuring that both tables have the same structure, we combine them into a new table called "All_Data" using the **Append Queries** feature in Power Query Editor.

4. **Cleaning Data**: Redundant or inconsistent values in columns like "Age_Profile" and "Time_Band" are cleaned by replacing discrepancies and removing any trailing spaces.

5. **Data Validation**: Before proceeding, we validate the data by checking the data types and row counts in the **Transform Data** ribbon and cross-verifying them with the raw .csv files. Any missing columns (e.g., "Specialty_Name") are addressed, and the **Case_Type** column is added where needed.

6. **Finalization**: Once the data is cleaned and prepared, the changes are finalized by clicking **Close & Apply** in Power Query Editor.

These steps ensure that the data is clean, consistent, and ready for further analysis.

### Date Table Using DAX 
**Create a new DATE Table**.

***Date Table = 
VAR StartDate = MIN(All_Data[Archive_Date])
VAR EndDate = MAX(All_Data[Archive_Date])
VAR DateTable = ADDCOLUMNS(
    CALENDAR(StartDate,EndDate),
    "Year", YEAR([Date]),
    "Quarter Name", FORMAT([Date],"\QQ"),
    "Quarter Number", QUARTER([Date]),
    "Month Name", FORMAT([Date],"MMM"),
    "Month Number", MONTH([Date]),
    "Month Year", FORMAT([Date],"MM YYYY"),
    "Month Year Num", VALUE(FORMAT([Date], "YYYYMM")),
    "Week Num", "W" & WEEKNUM([Date]),
    "Day Name", FORMAT([Date],"DDD"),
    "Day Num", WEEKDAY([Date])
    )
Return
DateTable***.


### **STEP IV: Data Modelling 🔗**

Hide Inpatient & Outpatient Datasets.
Load a new mapping (specialty) table for specialty. Link it with our overall dataset using "Specialty_Name" column.
Transform and model mapping table.


## 4. Analysis and Insights 🧮

### **Key Metrics:**
- **Latest Month Wait List 👨‍⚖️**: Shows the wait list for the current month.
- **PY Latest Month Wait List 👨‍⚖️**: Shows the wait list for the same month in the previous year.

### **Charts and Visuals:**
- The dashboard includes several key visualizations:
  - **Doughnut Chart 🍩**: Displays the proportion of different waitlist categories.
  - **Clustered Column Chart 📊**: Breaks down data into clear, comparative bars.
  - **Top Five Multi-Row Card 🔟**: Highlights the top 5 categories with the most significant waiting lists.

### **Line Chart 📉:**
- Use the **Total** column directly with the **Archive_Date** 🗓️ to show trends over time. 
- Apply a **Case_Type** 🏥 visual filter to show:
  - One chart for **Day Case & Inpatients** 🛏️
  - One chart for **Outpatients** 🩺

### **Filters and Slicers:**
- Add slicers for:
  - **Archive_Date** 📅
  - **Case_Type** 🩺
  - **Specialty** 💉

---

### **Case Type Breakdown:**
| **Case Type**  | **Description**                    |
|----------------|------------------------------------|
| **Inpatient**  | 🛏️ Wait list of **12**.           |
| **Day Case**   | 🏥 Wait list of **19**.           |
| **Outpatient** | 🩺 Wait list of **80**.           |

### **Key Insights:**
- For **Outpatient 🩺** and **Inpatient 🛏️** cases the highest wait list in the **Pediatric 👶** specialty. 
- For **Day Case 🏥**, **Ophthalmology 👁️** has the highest wait list.

---

### **March 2021 📅 Trends:**
1. **Age Profile 16-64 👨‍👩‍👧‍👦**:
  - **Outpatient 🩺** in **Orthopaedics 🦵** had the largest wait list, with **49k/397k** patients.
  - **Inpatient 🛏️** cases were highest in **Pediatric 👶**.
  - **Day Case 🏥** had the largest wait list in **General Surgery 🔪**.

2. **Age Profile 65+ 👵👴:**
- **Outpatient 🩺** had the highest wait list in **Ophthalmology 👁️**.
- **Inpatient 🛏️** cases had the largest wait list in **Orthopaedics 🦵**.
- **Day Case 🏥** had the highest wait list again in **Ophthalmology 👁️**.

---

## 5. Conclusion
Overall, the dashboard helps identify critical areas where wait lists are most prominent, providing actionable insights for hospital administrators and healthcare professionals to prioritize resources, optimize scheduling, and address bottlenecks in patient care. 
By leveraging these insights, hospitals can work towards reducing wait times and improving patient satisfaction across different specialties and age groups.


## 6. Contact Information
For any questions or feedback regarding this report, please contact:
stat.data247@gmail.com
