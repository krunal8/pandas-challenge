# Pandas-HeroesOfPymoli
Krunal Modi's Heroes Of Pymoli Game Data Analysis using Pandas Library and Jupyter Notebook


### Heroes Of Pymoli Data Analysis

* Of the 780 active players, the vast majority are male (84%). There also exists, a smaller, but notable proportion of female     layers (14%).

* Our peak age demographic falls between 20-24 (46.8%) with secondary groups falling between 15-19 (17.4%) and 25-29 (13%).

* The majority of purchases are also done by the age group 20-24 (46.8%) with secondary groups falling between 15-19 (17.4%) and   25-29 (13%).  

* Out of 183 items offered, the most popular and profitable ones are "Oathbreaker, Last Hope of the Breaking Storm" (12 buys), 
  brought $51 and "Nirvana" and "Fiery Glass Crusader" having (9 buys) each and brought $44 and $41 respectively. 
  Generally, all players (780) prefer different items, there are no significantly more popular item(s) than others.

* Average purchase is about $3 per person with the top spenders paying up to $19 for their purchases. Still, 97% are paying way   under $10. The total profit from the sold items is about $2400 for 780 players.

-----


```python
# Dependencies and Setup
import pandas as pd
import numpy as np

# Raw data file
file_to_load = "Resources/purchase_data.csv"

# Read purchasing file and store into pandas data frame
purchase_data = pd.read_csv(file_to_load)
purchase_data_df = pd.DataFrame(purchase_data)
purchase_data_df.head()
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase ID</th>
      <th>SN</th>
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Lisim78</td>
      <td>20</td>
      <td>Male</td>
      <td>108</td>
      <td>Extraction, Quickblade Of Trembling Hands</td>
      <td>3.53</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Lisovynya38</td>
      <td>40</td>
      <td>Male</td>
      <td>143</td>
      <td>Frenzied Scimitar</td>
      <td>1.56</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Ithergue48</td>
      <td>24</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>4.88</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Chamassasya86</td>
      <td>24</td>
      <td>Male</td>
      <td>100</td>
      <td>Blindscythe</td>
      <td>3.27</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Iskosia90</td>
      <td>23</td>
      <td>Male</td>
      <td>131</td>
      <td>Fury</td>
      <td>1.44</td>
    </tr>
  </tbody>
</table>
</div>



## Player Count

* Display the total number of players


```python
total_players = purchase_data_df["SN"].count()
total_players
```




    780



## Purchasing Analysis (Total)

* Run basic calculations to obtain number of unique items, average price, etc.


* Create a summary data frame to hold the results


* Optional: give the displayed data cleaner formatting


* Display the summary data frame



```python
unique_items = len(purchase_data_df["Item ID"].unique())
unique_items

total_purchases = purchase_data_df["Purchase ID"].count()
total_purchases

total_revenue = purchase_data_df["Price"].sum()
total_revenue

average_price1 = purchase_data_df["Price"].mean()
average_price1

average_price2 = total_revenue/total_purchases
average_price2

# average_price1 = average_price2

# Display the summary data frame
purchase_analysis_df = pd.DataFrame([{"Number of Unique Items": unique_items, "Average Price": average_price1,
                                      "Number of Purchases": total_purchases, "Total Revenue": total_revenue}])
purchase_analysis_df["Average Price"] = purchase_analysis_df["Average Price"].map("${:,.2f}".format)
purchase_analysis_df["Total Revenue"] = purchase_analysis_df["Total Revenue"].map("${:,.2f}".format)
purchase_analysis_df

# reorganazing the column order for summary dataframe
org_purchase_analysis_df = purchase_analysis_df[["Number of Unique Items","Average Price","Number of Purchases", "Total Revenue" ]]
org_purchase_analysis_df

```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Number of Unique Items</th>
      <th>Average Price</th>
      <th>Number of Purchases</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>183</td>
      <td>$3.05</td>
      <td>780</td>
      <td>$2,379.77</td>
    </tr>
  </tbody>
</table>
</div>



## Gender Demographics

* Run basic calculations to obtain number of unique items, average price, etc.


* Create a summary data frame to hold the results


* Optional: give the displayed data cleaner formatting


* Display the summary data frame



```python
# The value counts method counts unique values in a column, then dataframe created to hold results
gender_demo_df = pd.DataFrame(purchase_data_df["Gender"].value_counts())
gender_demo_df

percentage_of_players = (purchase_data_df["Gender"].value_counts()/total_players)*100
percentage_of_players

# Calculations performed and added into Data Frame as a new column
gender_demo_df["Percentage of Players"] = percentage_of_players
gender_demo_df["Percentage of Players"] = gender_demo_df["Percentage of Players"].map("{:,.2f}%".format)
gender_demo_df

# Change the order of the columns 
org_gender_demo_df = gender_demo_df[["Percentage of Players", "Gender"]]
org_gender_demo_df

# Rename the column "Gender" to "Total Counts" using .rename(columns={})
fin_gender_demo_df = org_gender_demo_df.rename(columns={"Gender":"Total Count"})
fin_gender_demo_df

```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Percentage of Players</th>
      <th>Total Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>83.59%</td>
      <td>652</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>14.49%</td>
      <td>113</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>1.92%</td>
      <td>15</td>
    </tr>
  </tbody>
</table>
</div>




## Purchasing Analysis (Gender)

* Run basic calculations to obtain purchase count, avg. purchase price, etc. by gender


* For normalized purchasing, divide total purchase value by purchase count, by gender


* Create a summary data frame to hold the results


* Optional: give the displayed data cleaner formatting


* Display the summary data frame


```python
# Using GroupBy in order to separate the data into fields according to "Gender" values
gender_grouped_purchased_data_df = purchase_data_df.groupby(["Gender"])

# The object returned is a "GroupBy" object and cannot be viewed normally...
# print(gender_grouped_purchased_data_df)

# In order to be visualized, a data function must be used...
gender_grouped_purchased_data_df["Purchase ID"].count().head(10)

# Get total purchase value by gender
total_purchase_value = gender_grouped_purchased_data_df["Price"].sum()
total_purchase_value.head()
dlr_total_purchase_value = total_purchase_value.map("${:,.2f}".format)
dlr_total_purchase_value.head()

# Average purchase price by gender
avg_purchase_price = gender_grouped_purchased_data_df["Price"].mean()
avg_purchase_price.head()
dlr_avg_purchase_price = avg_purchase_price.map("${:,.2f}".format)
dlr_avg_purchase_price.head()

# Normalized totals, total purchase value divided by purchase count by gender
normalized_totals = total_purchase_value/gender_grouped_purchased_data_df["Purchase ID"].count()
dlr_normalized_totals = normalized_totals.map("${:,.2f}".format)
dlr_normalized_totals.head()

# Organize summary gender data, get all columns to organized Data Frame, add needed columns to it
org_gender_purchased_data_df = pd.DataFrame(gender_grouped_purchased_data_df["Purchase ID"].count())
org_gender_purchased_data_df["Average Purchase Price"] = dlr_avg_purchase_price  
org_gender_purchased_data_df["Total Purchase Value"] = dlr_total_purchase_value 
org_gender_purchased_data_df["Normalized Totals"] = dlr_normalized_totals 
org_gender_purchased_data_df

# Summary purchasing analysis DF grouped by gender, rename "Purchase ID" column, using .rename(columns={})
summary_gender_purchased_data_df = org_gender_purchased_data_df.rename(columns={"Purchase ID":"Purchase Count"})
summary_gender_purchased_data_df

```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Normalized Totals</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>113</td>
      <td>$3.20</td>
      <td>$361.94</td>
      <td>$3.20</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>652</td>
      <td>$3.02</td>
      <td>$1,967.64</td>
      <td>$3.02</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>15</td>
      <td>$3.35</td>
      <td>$50.19</td>
      <td>$3.35</td>
    </tr>
  </tbody>
</table>
</div>



## Age Demographics

* Establish bins for ages


* Categorize the existing players using the age bins. Hint: use pd.cut()


* Calculate the numbers and percentages by age group


* Create a summary data frame to hold the results


* Optional: round the percentage column to two decimal points


* Display Age Demographics Table



```python
# Create the bins in which Purchse Data will be held
# Bins are 0, 9.90, 14.90, 19.90, 24.90, 29.90, 34.90, 39.90, 99999
# Establish bins for ages
age_bins = [0, 9.90, 14.90, 19.90, 24.90, 29.90, 34.90, 39.90, 99999]

# # Create the names for the four bins
group_names = ["<10", "10-14", "15-19", "20-24", "25-29", "30-34", "35-39", "40+"]

grp_by_age_purchase_data_df = purchase_data_df
grp_by_age_purchase_data_df["Age Summary"] = pd.cut(grp_by_age_purchase_data_df["Age"], age_bins, labels=group_names)
grp_by_age_purchase_data_df

# Creating a group based off of the bins and saving it
grp_by_age_purchase_data_df = grp_by_age_purchase_data_df.groupby("Age Summary")
grp_by_age_purchase_data_df.count()

# Let's work with this new dataframe
summary_by_age_df = pd.DataFrame(grp_by_age_purchase_data_df.count())
summary_by_age_df 

# Calculations performed on "Purchase Id"" column of summary df
summary_by_age_df["Purchase ID"] = (summary_by_age_df["Purchase ID"]/total_players)*100
summary_by_age_df 

# format percentages 
summary_by_age_df["Purchase ID"] = summary_by_age_df["Purchase ID"].map("{:,.2f}%".format)
summary_by_age_df

#  Reduce number of columns to "Age Summary", "Purcahe ID", format percentages and write into org Data Frame
org_summary_by_age_df = summary_by_age_df[["Purchase ID","SN"]]
org_summary_by_age_df

# Rename the columns for the final age dempgraphic df using .rename(columns={})
fin_grp_by_age_summary_df = org_summary_by_age_df.rename(columns={"Purchase ID":"Percentage of Players", "SN":"Total Count"})
fin_grp_by_age_summary_df

```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Percentage of Players</th>
      <th>Total Count</th>
    </tr>
    <tr>
      <th>Age Summary</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>2.95%</td>
      <td>23</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>3.59%</td>
      <td>28</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>17.44%</td>
      <td>136</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>46.79%</td>
      <td>365</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>12.95%</td>
      <td>101</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>9.36%</td>
      <td>73</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>5.26%</td>
      <td>41</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>1.67%</td>
      <td>13</td>
    </tr>
  </tbody>
</table>
</div>



## Purchasing Analysis (Age)

* Bin the purchase_data data frame by age


* Run basic calculations to obtain purchase count, avg. purchase price, etc. in the table below


* Calculate Normalized Purchasing


* Create a summary data frame to hold the results


* Optional: give the displayed data cleaner formatting


* Display the summary data frame


```python
# Let's work with data sorted by age new dataframe
analysis_by_age_df = pd.DataFrame(grp_by_age_purchase_data_df["Purchase ID"].count())
analysis_by_age_df

# Get Total purchase value by age
total_purchase_value_age = grp_by_age_purchase_data_df["Price"].sum()
total_purchase_value_age
dlr_total_purchase_value_age = total_purchase_value_age.map("${:,.2f}".format)
dlr_total_purchase_value_age

# Get Average purchase price by age
avg_purchase_price_age = grp_by_age_purchase_data_df["Price"].mean()
avg_purchase_price_age
dlr_avg_purchase_price_age = avg_purchase_price_age.map("${:,.2f}".format)
dlr_avg_purchase_price_age

# Get Normalized totals by age, total purchase value divided by purchase count by age
normalized_totals_age = total_purchase_value_age/grp_by_age_purchase_data_df["Purchase ID"].count()
dlr_normalized_totals_age = normalized_totals_age.map("${:,.2f}".format)
dlr_normalized_totals_age

# Organize summary gender data, get all columns to organized Data Frame, add needed columns to it
analysis_by_age_df["Average Purchase Price"] = dlr_avg_purchase_price_age  
analysis_by_age_df["Total Purchase Value"] = dlr_total_purchase_value_age 
analysis_by_age_df["Normalized Totals"] = dlr_normalized_totals_age 
analysis_by_age_df

# Summary purchasing analysis DF grouped by age, rename "Purchase ID" column, using .rename(columns={})summary_gender_purchased_data_df = org_gender_purchased_data_df.rename(columns={"Purchase ID":"Purchase Count"})
summary_age_purchased_data_df = analysis_by_age_df.rename(columns={"Purchase ID":"Purchase Count"})
summary_age_purchased_data_df

```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Normalized Totals</th>
    </tr>
    <tr>
      <th>Age Summary</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>23</td>
      <td>$3.35</td>
      <td>$77.13</td>
      <td>$3.35</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>28</td>
      <td>$2.96</td>
      <td>$82.78</td>
      <td>$2.96</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>136</td>
      <td>$3.04</td>
      <td>$412.89</td>
      <td>$3.04</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>365</td>
      <td>$3.05</td>
      <td>$1,114.06</td>
      <td>$3.05</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>101</td>
      <td>$2.90</td>
      <td>$293.00</td>
      <td>$2.90</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>73</td>
      <td>$2.93</td>
      <td>$214.00</td>
      <td>$2.93</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>41</td>
      <td>$3.60</td>
      <td>$147.67</td>
      <td>$3.60</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>13</td>
      <td>$2.94</td>
      <td>$38.24</td>
      <td>$2.94</td>
    </tr>
  </tbody>
</table>
</div>



## Top Spenders

* Run basic calculations to obtain the results in the table below


* Create a summary data frame to hold the results


* Sort the total purchase value column in descending order


* Optional: give the displayed data cleaner formatting


* Display a preview of the summary data frame




```python
# Let's work with original purchase data grouped by player("SN") 
orig_purchase_data_df = pd.DataFrame(purchase_data)
orig_purchase_data_df.head()

# Group by Spendors ( "SN" )
grp_SN_top_spendor_df = orig_purchase_data_df.groupby("SN")
grp_SN_top_spendor_df.count()

# Let's work with data sorted by SN new dataframe
analysis_by_SPENDOR_df = pd.DataFrame(grp_SN_top_spendor_df["Purchase ID"].count())
analysis_by_SPENDOR_df

# Get Total purchase value by SN
total_purchase_value_SN = grp_SN_top_spendor_df["Price"].sum()
total_purchase_value_SN

# Get Average purchase price by SN
avg_purchase_price_SN = grp_SN_top_spendor_df["Price"].mean()
avg_purchase_price_SN
dlr_avg_purchase_price_SN = avg_purchase_price_SN.map("${:,.2f}".format)
dlr_avg_purchase_price_SN

# Organize summary Top Spender data, get all columns to organized Data Frame, add needed columns to it
analysis_by_SPENDOR_df["Average Purchase Price"] = dlr_avg_purchase_price_SN
analysis_by_SPENDOR_df["Total Purchase Value"] = total_purchase_value_SN 
analysis_by_SPENDOR_df

# Summary Top Spendor analysis grouped by SN, rename "Purchase ID" column, using .rename(columns={})
SUM_SN_purchased_data_df = analysis_by_SPENDOR_df.rename(columns={"Purchase ID":"Purchase Count"})
TOP5_spendors_df=SUM_SN_purchased_data_df.sort_values("Total Purchase Value", ascending=False)
TOP5_spendors_df.head()

# Format Total Purchase Price to be in dollar form
dlr_total_purchase_value_SN = total_purchase_value_SN.map("${:,.2f}".format)
TOP5_spendors_df["Total Purchase Value"] = dlr_total_purchase_value_SN
TOP5_spendors_df.head()

```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>SN</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Lisosia93</th>
      <td>5</td>
      <td>$3.79</td>
      <td>$18.96</td>
    </tr>
    <tr>
      <th>Idastidru52</th>
      <td>4</td>
      <td>$3.86</td>
      <td>$15.45</td>
    </tr>
    <tr>
      <th>Chamjask73</th>
      <td>3</td>
      <td>$4.61</td>
      <td>$13.83</td>
    </tr>
    <tr>
      <th>Iral74</th>
      <td>4</td>
      <td>$3.40</td>
      <td>$13.62</td>
    </tr>
    <tr>
      <th>Iskadarya95</th>
      <td>3</td>
      <td>$4.37</td>
      <td>$13.10</td>
    </tr>
  </tbody>
</table>
</div>



## Most Popular Items

* Retrieve the Item ID, Item Name, and Item Price columns


* Group by Item ID and Item Name. Perform calculations to obtain purchase count, item price, and total purchase value


* Create a summary data frame to hold the results


* Sort the purchase count column in descending order


* Optional: give the displayed data cleaner formatting


* Display a preview of the summary data frame




```python
# Let's work again with original purchase data grouped by "Item ID" and "Item Name"

# Group by Item ( "Item ID" and "Item Name" )
grp_top_item_df = orig_purchase_data_df.groupby(["Item ID", "Item Name"])
grp_top_item_df.count()

# Let's work with data sorted by Item new dataframe
analysis_by_ITEM_df = pd.DataFrame(grp_top_item_df["Purchase ID"].count())
analysis_by_ITEM_df

# Get Total purchase value by Item
total_purchase_value_ITEM = grp_top_item_df["Price"].sum()
total_purchase_value_ITEM
dlr_total_purchase_value_ITEM = total_purchase_value_ITEM.map("${:,.2f}".format)
dlr_total_purchase_value_ITEM

# Get purchase price by Item
purchase_price_ITEM = grp_top_item_df["Price"].mean()
purchase_price_ITEM
dlr_purchase_price_ITEM = purchase_price_ITEM.map("${:,.2f}".format)
dlr_purchase_price_ITEM

# Organize summary Item data, get all columns to organized Data Frame, add needed columns to it
analysis_by_ITEM_df["Item Price"] = dlr_purchase_price_ITEM
analysis_by_ITEM_df["Total Purchase Value"] = dlr_total_purchase_value_ITEM
analysis_by_ITEM_df

# Summary Most Popular Item analysis grouped by Item, rename "Purchase ID" column, using .rename(columns={})
SUM_ITEM_purchased_data_df = analysis_by_ITEM_df.rename(columns={"Purchase ID":"Purchase Count"})
TOP5_ITEMS_df=SUM_ITEM_purchased_data_df.sort_values("Purchase Count", ascending=False)
TOP5_ITEMS_df.head()

```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>178</th>
      <th>Oathbreaker, Last Hope of the Breaking Storm</th>
      <td>12</td>
      <td>$4.23</td>
      <td>$50.76</td>
    </tr>
    <tr>
      <th>145</th>
      <th>Fiery Glass Crusader</th>
      <td>9</td>
      <td>$4.58</td>
      <td>$41.22</td>
    </tr>
    <tr>
      <th>108</th>
      <th>Extraction, Quickblade Of Trembling Hands</th>
      <td>9</td>
      <td>$3.53</td>
      <td>$31.77</td>
    </tr>
    <tr>
      <th>82</th>
      <th>Nirvana</th>
      <td>9</td>
      <td>$4.90</td>
      <td>$44.10</td>
    </tr>
    <tr>
      <th>19</th>
      <th>Pursuit, Cudgel of Necromancy</th>
      <td>8</td>
      <td>$1.02</td>
      <td>$8.16</td>
    </tr>
  </tbody>
</table>
</div>



## Most Profitable Items

* Sort the above table by total purchase value in descending order


* Optional: give the displayed data cleaner formatting


* Display a preview of the data frame




```python
# Alter analysis by Item_df to have total purchase price column in numeric form to be able to sort it numerically
SUM_ITEM_purchased_data_df["Total Purchase Value"] = grp_top_item_df["Price"].sum()
SUM_ITEM_purchased_data_df

# Summary Most Popular Item analysis grouped by Item, rename "Purchase ID" column, using .rename(columns={})
TOP5_ITEMS_df=SUM_ITEM_purchased_data_df.sort_values("Total Purchase Value", ascending=False)

# Format Total Purchase Price to be in dollar form
dlr_total_purchase_value_ITEM = total_purchase_value_ITEM.map("${:,.2f}".format)
TOP5_ITEMS_df["Total Purchase Value"] = dlr_total_purchase_value_ITEM
TOP5_ITEMS_df.head()

```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>178</th>
      <th>Oathbreaker, Last Hope of the Breaking Storm</th>
      <td>12</td>
      <td>$4.23</td>
      <td>$50.76</td>
    </tr>
    <tr>
      <th>82</th>
      <th>Nirvana</th>
      <td>9</td>
      <td>$4.90</td>
      <td>$44.10</td>
    </tr>
    <tr>
      <th>145</th>
      <th>Fiery Glass Crusader</th>
      <td>9</td>
      <td>$4.58</td>
      <td>$41.22</td>
    </tr>
    <tr>
      <th>92</th>
      <th>Final Critic</th>
      <td>8</td>
      <td>$4.88</td>
      <td>$39.04</td>
    </tr>
    <tr>
      <th>103</th>
      <th>Singed Scalpel</th>
      <td>8</td>
      <td>$4.35</td>
      <td>$34.80</td>
    </tr>
  </tbody>
</table>
</div>




# Conclusions: 

* Of the 780 active players, the vast majority are male (84%). There also exists, a smaller, but notable proportion of female     layers (14%).

* Our peak age demographic falls between 20-24 (46.8%) with secondary groups falling between 15-19 (17.4%) and 25-29 (13%).

* The majority of purchases are also done by the age group 20-24 (46.8%) with secondary groups falling between 15-19 (17.4%) and   25-29 (13%).  

* Out of 183 items offered, the most popular and profitable ones are "Oathbreaker, Last Hope of the Breaking Storm" (12 buys), 
  brought $51 and "Nirvana" and "Fiery Glass Crusader" having (9 buys) each and brought $44 and $41 respectively. 
  Generally, all players (780) prefer different items, there are no significantly more popular item(s) than others.

* Average purchase is about $3 per person with the top spenders paying up to $19 for their purchases. Still, 97% are paying way   under $10. The total profit from the sold items is about $2400 for 780 players.

