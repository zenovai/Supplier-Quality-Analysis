<img width="1649" height="862" alt="Supplier Quality 2 " src="https://github.com/user-attachments/assets/5d2e47ea-806d-4a1a-a950-a00566254d4c" />
<img width="1816" height="852" alt="Supplier Quality 1 " src="https://github.com/user-attachments/assets/77744f5b-14df-4cb6-8647-850465a0228d" />

# 1- Business Related Questions & KPIS
### **Supplier Performance**

1. Which suppliers have the highest defect rates? 
2. Which suppliers cause the most production downtime?
3. Which suppliers deliver the most rejected materials? 

### **Defect Analysis**

1. What are the most common defect types? 
2. Which defects cause the most downtime? 
3. How do defect types vary across material types? 
4. Are defects increasing or decreasing over time? 

### **Plant & Location Performance**

1. Which plants experience the most defects? 
2. How does downtime vary across plants? 

### **Material & Category Insights**

1. Which material types have the highest defect rates? 
2. Which defect categories contribute the most to downtime? 
----------------------------------------------------------------
# 2- Data Cleaning
1. Changed Defect ID From TEXT to Whole Number
2. Sub Category and Category contain the exact same data, so we will delete sub category and rename subcategory ID to Category ID
3. Removed Duplicates in Defect column In Defect Table, Duplicates Were (Bad Seams, Dents, Contamination, Scratches, Damaged in transit, Too Stiff, Damaged Parts, Missing Components, Warped, Not Certified, Odor, Leaking Packaging, Wrong Core, Out of spec, Not cleaned, Split Seams, No adhesive, bad welding, missing labels, holes, Wrong cut, Scrap Attached, water damage, other, packaging issues, no liner, wrong registration, )
4. After removing the duplicates our Defect ID is no longer incremented properly so a new indexed table had to be made which is our new Defect ID 
5. Repeat the Process for Vendor ID in the table Vendors
6. Fixed the Data Type in the Defect Report Table
7. There is a problem with the Defect table, the defect ID only goes up to 272 but the defect ID column in the defect report table goes up to 305 which will result in blank values therefore we can't do analysis related to defects 

--------------------------------------------------------------------------
# 3- Data Modeling 

![[Pasted image 20250223002801.png]]
Identified the unique key columns in each table then connected them to the defect reports table using one to many relationships 

as mentioned i got rid of the subcategory column since it served no purpose and renamed Subcategory ID to Category ID

this is a star model with no snowflakes which is optimal 
-------------------------------------------------------------------------------------------------------------
# 4- DAX
1. we create a measures table to store our measures and categorize it easier 
2. first we find the sum of total defect quantities ```Total Defect QTY = SUM('Defect Report'[Defect Qty]) ```
3. We then obtain the total number of reported defects using Count rows `Total Defects Reported = COUNTROWS('Defect Report')`
4. Then total downtime mins `Total Downtime Mins = SUM('Defect Report'[Downtime min])`
5. Then total number of rejections that occured `Total Rejections = CALCULATE(SUM('Defect Report'[Defect ID]), Defect[Defect ID] = 4)`
--------------------------------------------------------------------------
# 5- Insights

## Materials & Vendors 
- The Period from may to october in 2013 has seen a slight downward trend in total defects unlike 2014 of the same period where it has seen an extreme jump in defect rate 
- Vendor Reddoit Has the highest downtime and defect rates therefore they should be investegated as soon as possible
- Vendor Solholdings and Plustax have extremely high defect rates
- Raw Materials Have The Highest Defect Rates
- Most of the downtime is caused by defects that have the Impact type


## Plant Analysis 
- Most of the defect materials are rejected
- Our plant in detroit has the highest amount of defects, regions Michigan, Illinios and indiana are the main contributers to our defect quantity
- Springfield plant had the most downtime caused by defects of the type Impact
- Mechanical Category has the highest defect where electrical has the lowest 





## **Executive Summary**

This report analyzes supplier performance, defect trends, and plant-specific issues, aiming to identify key areas of improvement to optimize manufacturing processes. Several critical findings emerged, highlighting problematic vendors, materials, and plants. Immediate action is needed to address high defect rates, particularly from vendor Reddoit, and to reduce downtime caused by specific defect types. Further investigation and corrective measures are recommended to ensure better product quality and process efficiency moving forward.

---

## **Key Business Questions & KPIs**

### **Supplier Performance**

- **Highest Defect Rates by Supplier**: Identifying which suppliers have the highest defect rates and contributing to production inefficiencies.
- **Downtime Impact from Suppliers**: Analyzing which suppliers are responsible for the most production downtime.
- **Rejected Materials by Supplier**: Tracking which suppliers have the highest incidence of material rejections.

### **Defect Analysis**

- **Top Defect Categories**: Identifying the most common defect types and their implications.
- **Downtime Correlation with Defect Types**: Understanding which defects are most responsible for causing production downtime.
- **Defects by Material Type**: Analyzing how defect types vary across different materials.
- **Defect Trend Over Time**: Tracking whether defect frequency is increasing or decreasing.

### **Plant & Location Performance**

- **Defect Rates by Plant**: Identifying plants with the highest defect rates and evaluating their impact on overall production.
- **Downtime Variation by Plant**: Analyzing downtime across plants and identifying the most critical locations needing attention.

### **Material & Category Insights**

- **Material Defect Rates**: Identifying which material types have the highest defect rates.
- **Downtime Impact by Defect Category**: Understanding which defect categories most contribute to downtime.

---

## **Data Cleaning and Preprocessing**

1. **Defect ID Adjustment**: Corrected Defect IDs to align properly, ensuring accurate tracking.
2. **Subcategory Removal**: Removed the redundant **Subcategory** column and renamed **Subcategory ID** to **Category ID** for consistency.
3. **Duplicate Removal in Defect Table**: Removed duplicate defect entries to improve data integrity, including defects like “Bad Seams” and “Dents.”
4. **Indexed Defect ID**: Updated Defect ID to maintain proper indexing after duplicate removal.
5. **Vendor Table Cleaning**: Applied the same process of duplicate removal and ID renaming to the Vendor table.
6. **Data Type Correction**: Ensured data types are correct in the Defect Report table for accurate modeling and analysis.
7. **Defect ID Mismatch Issue**: Identified and addressed the discrepancy where Defect IDs in the Defect Report table exceeded the available Defect IDs, preventing blank values during analysis.

---

## **Data Modeling and Structure**

### **Star Schema Design**

- Established a **Star Schema** model, ensuring clear relationships between the **Defect Report** table and other entities like **Suppliers**, **Materials**, and **Defects**.
- Eliminated unnecessary **Subcategory** column, simplifying the schema and improving model efficiency.
- **One-to-Many Relationships**: Connected key columns (e.g., Defect ID, Vendor ID) to the central **Defect Report** table, ensuring optimized data structure.

---

## **DAX Measures**

1. **Total Defect Quantity**:
	`Total Defect QTY = SUM('Defect Report'[Defect Qty])`
    
2. **Total Defects Reported**:
    `Total Defects Reported = COUNTROWS('Defect Report')`
    
3. **Total Downtime Minutes**: 
	`Total Downtime Mins = SUM('Defect Report'[Downtime min])`
    
5. **Total Rejections**:
	`Total Rejections = CALCULATE(SUM('Defect Report'[Defect ID]), 'Defect'[Defect ID] = 4)`
    

---

## **Key Insights**

### **Materials & Vendors**

- **Defect Trends**: From May to October 2013, defect rates showed a slight decrease, but 2014 saw a drastic increase in defect rates for the same period.
- **High Defect Vendors**: Vendor **Reddoit** shows the highest downtime and defect rates, making them a prime candidate for further investigation. Other vendors such as **Solholdings** and **Plustax** also exhibit high defect rates, warranting a closer review of their materials and processes.
- **Material Defects**: **Raw Materials** are responsible for the highest defect rates, suggesting potential issues with material quality from suppliers.
- **Impact Defects**: Most downtime is attributed to defects categorized as “Impact,” highlighting the need to address both material and handling processes.

### **Plant & Location Performance**

- **Detroit Plant**: The **Detroit plant** has the highest number of defects, with regions such as **Michigan**, **Illinois**, and **Indiana** contributing most to the defect quantities. A targeted improvement plan should be developed for this location.
- **Springfield Plant Downtime**: The **Springfield plant** experiences the highest downtime due to **Impact-type defects**, requiring immediate focus on the handling and storage processes to reduce this downtime.
- **Mechanical vs. Electrical**: **Mechanical defects** dominate in terms of defect rates, while **electrical defects** remain minimal. This indicates potential issues with mechanical processes or components, requiring in-depth review.

---

## **Actionable Recommendations**

1. **Vendor Engagement**: Immediate investigations should be conducted for **Reddoit**, **Solholdings**, and **Plustax**. Consider alternative suppliers for critical materials and explore opportunities for process improvements with these vendors.
2. **Raw Material Quality Control**: Engage with raw material suppliers to tighten quality control procedures. Implement more rigorous incoming material inspections to reduce defect rates.
3. **Plant-Specific Improvements**: For the **Detroit plant**, a comprehensive quality control audit should be performed to address mechanical defects. For **Springfield**, focus on reducing downtime from impact-related defects through better handling and storage protocols.
4. **Defect Type Analysis**: Focus on addressing **Impact-type defects** across plants and suppliers. This category is consistently linked to the highest downtime and defects.
