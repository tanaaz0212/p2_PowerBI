# p2_PowerBI

#  Data Modeler – Building a Normalized Star Schema Data Model

##  Project Overview

This project is part of an internal Power BI assignment focused purely on **Data Modeling**. The objective is to design a **clean, normalized Star Schema data model** using Power BI, demonstrating a strong understanding of:

* Table relationships
* Cardinality
* Star vs Snowflake schema
* Active and inactive relationships
* Filter flow and ambiguity handling

 **Note:** No DAX, calculated columns, or charts are used. All logic is implemented using **Power Query** and **Model View** only. A **Matrix visual** is used strictly for relationship verification.

---

##  Dataset Details

The project uses the following Excel files as source data:

### 1. Sales_Fact.xlsx (Central Fact Table)

* SalesID (PK)
* CustomerID (FK)
* ProductID (FK)
* RegionID (FK)
* DateKey (FK)
* Quantity
* Revenue
* Discount

### 2. Customer_Dim.xlsx

* CustomerID (PK)
* FullName
* Age
* Gender
* Segment

### 3. Product_Dim.xlsx

* ProductID (PK)
* ProductName
* Category
* Subcategory
* Brand

### 4. Region_Dim.xlsx

* RegionID (PK)
* Country
* State
* City

### 5. Date_Dim.xlsx

* DateKey (PK)
* Date
* Month
* Quarter
* Year
* Fiscal Year

### 6. Returns_Fact.xlsx (Secondary Fact Table)

* ReturnID (PK)
* SalesID (FK → Sales_Fact)
* ReturnDateKey (FK → Date_Dim)
* Reason

---

##  Data Preparation (Power Query)

* Imported all Excel files using **Get Data → Excel**
* Removed blank rows from all tables
* Assigned correct data types (Whole Number, Date, Text, Decimal)
* Removed null values from foreign key columns to avoid relationship warnings
* Loaded cleaned data into the data model

---

##  Data Model & Relationships

###  Star Schema Design

* **Sales_Fact** acts as the central fact table
* Dimension tables connect to Sales_Fact using **Many-to-One** relationships

#### Active Relationships

* Sales_Fact[CustomerID] → Customer_Dim[CustomerID]
* Sales_Fact[ProductID] → Product_Dim[ProductID]
* Sales_Fact[RegionID] → Region_Dim[RegionID]
* Sales_Fact[DateKey] → Date_Dim[DateKey]

All active relationships use:

* Cardinality: Many to One (*:1)
* Cross-filter direction: Single

---

###  Snowflake / Secondary Fact Modeling

* **Returns_Fact** is modeled as a second fact table

#### Relationships

* Returns_Fact[SalesID] → Sales_Fact[SalesID] (Active)
* Returns_Fact[ReturnDateKey] → Date_Dim[DateKey] (**Inactive**)

The inactive relationship prevents multiple active paths to the Date table and avoids filter ambiguity.

---

##  Advanced Model Settings

* Single-direction filters used throughout the model
* No bidirectional filters enabled
* Inactive relationship used to simulate alternative date context
* Filter ambiguity resolved through proper schema design

---

##  Data Model Enhancements

### Data Formatting

* Revenue → Currency
* Quantity → Whole Number
* Date → Date format
* Discount → Percentage

### Data Categories

* Country → Country
* State → State
* City → City
* ProductName → Product

---

##  Hierarchies Created

### Date_Dim Hierarchy

* Year → Quarter → Month → Date

### Region_Dim Hierarchy

* Country → State → City

### Product_Dim Hierarchy

* Category → Subcategory → ProductName

---

##  Verification (Matrix Visuals)

Only **Matrix visuals** were used to verify correct relationship flow:

1. **Sales by Product Category and Region**
2. **Return Reasons by Fiscal Year**
3. **Revenue by Customer Segment**

These matrices confirm that filters propagate correctly across the data model.

---

##  Deliverables

* Single `.pbix` file containing:

  * Cleaned tables via Power Query
  * Star schema data model
  * Active and inactive relationships
  * Hierarchies and formatted fields
  * Matrix visuals for verification

* This README.md file explaining:

  * Schema design
  * Relationship logic
  * Filter flow
  * Issues encountered and resolution
