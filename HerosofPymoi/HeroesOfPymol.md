
# Hero of Pymoli Data Analysis

Observed Trend One: Within all the player, number of male players are dominated than female players' number(954 vs 187), so male players tend to be the main purchasing power within the pool

Observed Trend Two: In terms of age demographic, age 14-25 players contributed the most purchasing power. 

Observed Trend Three: The most populer items doesnt really 100% shwoing that they are the most profitable items. 



```python
import pandas as pd
import numpy as np
filepath1 = "generated_data/players_complete.csv"
players = pd.read_csv(filepath1)

filepath2 = "generated_data/items_complete.csv"
items = pd.read_csv(filepath2)

filepath3 = "generated_data/purchase_data_3.csv"
purchase = pd.read_csv(filepath3)
```

# Player Count

Total Number of Players




```python
dictt1 = pd.DataFrame({"Total Number of Players": [players["Player ID"].count()]})
dictt1
#x = pd.DataFrame(dictt1)

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Total Number of Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1163</td>
    </tr>
  </tbody>
</table>
</div>



# Purchasing Analysis (Total)

Number of Unique Items

Average Purchase Price

Total Number of Purchases

Total Revenue


```python

dictt2 = {}

dictt2["Number of Unique Items"] =  [items["Item Name"].nunique()]
dictt2["Average Purchase Price"] =  [round(items["Price"].mean(), 2)]
dictt2["Total Number of Purchases"] =  [items["Item ID"].count()]
dictt2["Total Revenue"] =  [items["Price"].sum()]

pd.DataFrame(dictt2)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Purchase Price</th>
      <th>Number of Unique Items</th>
      <th>Total Number of Purchases</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2.89</td>
      <td>186</td>
      <td>190</td>
      <td>549.13</td>
    </tr>
  </tbody>
</table>
</div>



# Gender Demographics

Percentage and Count of Male Players

Percentage and Count of Female Players

Percentage and Count of Other / Non-Disclosed




```python
mask1 = players["Gender"] == "Male"
mask2 = players["Gender"] == "Female"
mask3 = players["Gender"] == "Other / Non-Disclosed"
Total_count = players["Gender"].count()
male = players[mask1]
female = players[mask2]
other = players[mask3]

dictt3 = {}

dictt3["Percentage of Player"] = [round(male["Player ID"].count()/ Total_count, 2), round(female["Player ID"].count()/ Total_count,2), round(other["Player ID"].count()/ Total_count,2) ]

dictt3["Title"] = ["Male", "Female", "Other / Non-Disclosed"]

dictt3["Count of Players"]= [male["Player ID"].count(), female["Player ID"].count(), other["Player ID"].count()]

x = pd.DataFrame(dictt3)
y = x[["Title", "Percentage of Player", "Count of Players"]]
y["Percentage of Player"] = (y["Percentage of Player"]*100).map("%{:.2f}".format)
y

# still need to format it as percentage 

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Title</th>
      <th>Percentage of Player</th>
      <th>Count of Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Male</td>
      <td>%82.00</td>
      <td>954</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Female</td>
      <td>%16.00</td>
      <td>187</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Other / Non-Disclosed</td>
      <td>%2.00</td>
      <td>22</td>
    </tr>
  </tbody>
</table>
</div>



# Purchasing Analysis (Gender)

The below each broken by gender

Purchase Count

Average Purchase Price

Total Purchase Value

Normalized Totals


```python

newPurchase = purchase.groupby("Gender")
x = newPurchase.agg({ "Price":"sum"})
y = newPurchase.agg({"Purchase ID":"count", "Price":"mean"})
z = x.merge(y, how = "left", left_index= True, right_index = True)
z.rename(columns = {"Price_x" :"Purchase Total Value", "Purchase ID":"Purchase Count", "Price_y":"Average Purchase Value"}, inplace = True)

#print(z)
mask1 = purchase["Gender"] == "Male"
mask2 = purchase["Gender"] == "Female"
mask3 = players["Gender"] == "Other / Non-Disclosed"

malecount = purchase[mask1]["Gender"].count()
femalecount = purchase[mask2]["Gender"].count()
othercount = purchase[mask3]["Gender"].count()




NormMale =z.ix[1,0]/malecount
NormFe = z.ix[0,0]/femalecount
NormOther = z.ix[2,0]/othercount

dictt4 = {}
dictt4["Title"] = ["Female", "Male", "Other / Non-Disclosed"]
dictt4["Normalized Total"] = [NormFe, NormMale, NormOther]
v = pd.DataFrame(dictt4).set_index("Title")
v.merge(z, how = "left", left_index= True, right_index = True)

```

    /Users/apple/anaconda3/lib/python3.6/site-packages/ipykernel_launcher.py:15: UserWarning: Boolean Series key will be reindexed to match DataFrame index.
      from ipykernel import kernelapp as app
    /Users/apple/anaconda3/lib/python3.6/site-packages/ipykernel_launcher.py:20: DeprecationWarning: 
    .ix is deprecated. Please use
    .loc for label based indexing or
    .iloc for positional indexing
    
    See the documentation here:
    http://pandas.pydata.org/pandas-docs/stable/indexing.html#ix-indexer-is-deprecated





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Normalized Total</th>
      <th>Purchase Total Value</th>
      <th>Purchase Count</th>
      <th>Average Purchase Value</th>
    </tr>
    <tr>
      <th>Title</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>3.183077</td>
      <td>41.38</td>
      <td>13</td>
      <td>3.183077</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>2.884375</td>
      <td>184.60</td>
      <td>64</td>
      <td>2.884375</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>1.060000</td>
      <td>2.12</td>
      <td>1</td>
      <td>2.120000</td>
    </tr>
  </tbody>
</table>
</div>



# Age Demographics

The below each broken into bins of 4 years (i.e. <10, 10-14, 15-19, etc.)

Purchase Count

Average Purchase Price

Total Purchase Value

Normalized Totals


```python
purchase.head()
# <10, 10,11,12,13 (<14)/ 14,15,16,17(<18) / 18,19,20 21(<22)/ 22, 23,24,25(<26)/26,27,28,29(<30)/30,31,32,33(<34)/34,35,36,37

bins = [0, 10, 14, 18, 22, 26, 30, 34, 38, 40]
group_names = ["<10", "10-13", "14-17", "18-21", "22-25", "26-29", "20-33", "34-37", ">38"]

purchase["Age Levels"] = pd.cut(purchase["Age"], bins, labels=group_names)

newdata1 = purchase.groupby("Age Levels").agg({"Purchase ID":"count", "Price":"mean", "Age Levels":"count"})
newdata2 = purchase.groupby("Age Levels").agg({"Price":"sum"})
newdata2.rename(columns= {"Price":"Total Purchase Value"}, inplace = True)

output = newdata1.merge(newdata2, how = "left",left_index=True, right_index=True ).rename(columns= {"Purchase ID":"Purchase Count", "Price":"Avg Purchase Price", "Age Levels":"Level Count"})
output["Avg Purchase Price"] = round(output["Avg Purchase Price"], 2)
output["Normalized Totals"] = round(output["Total Purchase Value"]/output["Level Count"], 2)
output
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Avg Purchase Price</th>
      <th>Level Count</th>
      <th>Total Purchase Value</th>
      <th>Normalized Totals</th>
    </tr>
    <tr>
      <th>Age Levels</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>5</td>
      <td>2.76</td>
      <td>5</td>
      <td>13.82</td>
      <td>2.76</td>
    </tr>
    <tr>
      <th>10-13</th>
      <td>3</td>
      <td>2.99</td>
      <td>3</td>
      <td>8.96</td>
      <td>2.99</td>
    </tr>
    <tr>
      <th>14-17</th>
      <td>11</td>
      <td>2.76</td>
      <td>11</td>
      <td>30.41</td>
      <td>2.76</td>
    </tr>
    <tr>
      <th>18-21</th>
      <td>20</td>
      <td>3.02</td>
      <td>20</td>
      <td>60.34</td>
      <td>3.02</td>
    </tr>
    <tr>
      <th>22-25</th>
      <td>23</td>
      <td>2.94</td>
      <td>23</td>
      <td>67.61</td>
      <td>2.94</td>
    </tr>
    <tr>
      <th>26-29</th>
      <td>4</td>
      <td>2.69</td>
      <td>4</td>
      <td>10.77</td>
      <td>2.69</td>
    </tr>
    <tr>
      <th>20-33</th>
      <td>5</td>
      <td>2.03</td>
      <td>5</td>
      <td>10.17</td>
      <td>2.03</td>
    </tr>
    <tr>
      <th>34-37</th>
      <td>5</td>
      <td>3.74</td>
      <td>5</td>
      <td>18.72</td>
      <td>3.74</td>
    </tr>
    <tr>
      <th>&gt;38</th>
      <td>2</td>
      <td>3.65</td>
      <td>2</td>
      <td>7.30</td>
      <td>3.65</td>
    </tr>
  </tbody>
</table>
</div>



# Top Spenders

Identify the the top 5 spenders in the game by total purchase value, then list (in a table):

SN

Purchase Count

Average Purchase Price

Total Purchase Value


```python
spender = purchase.groupby("SN")
spenders = spender.agg({"Purchase ID":"count", "Price": "sum"})
spenders = spenders.sort_values("Price", ascending = False).head(5)
spenders.rename(columns = {"Purchase ID":"Purchase Count", "Price":"Total Purchase Value"}, inplace = True)
spenders["Average Purchase Price"] = round(spenders["Total Purchase Value"]/spenders["Purchase Count"], 2)
spenders
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
      <th>Average Purchase Price</th>
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
      <th>Sundaky74</th>
      <td>2</td>
      <td>7.41</td>
      <td>3.70</td>
    </tr>
    <tr>
      <th>Aidaira26</th>
      <td>2</td>
      <td>5.13</td>
      <td>2.56</td>
    </tr>
    <tr>
      <th>Eusty71</th>
      <td>1</td>
      <td>4.81</td>
      <td>4.81</td>
    </tr>
    <tr>
      <th>Chanirra64</th>
      <td>1</td>
      <td>4.78</td>
      <td>4.78</td>
    </tr>
    <tr>
      <th>Alarap40</th>
      <td>1</td>
      <td>4.71</td>
      <td>4.71</td>
    </tr>
  </tbody>
</table>
</div>



# Most Popular Items

Identify the 5 most popular items by purchase count, then list (in a table):

Item ID

Item Name

Purchase Count

Item Price

Total Purchase Value


```python
u = purchase.groupby(["Item ID", "Item Name"])
y = u.agg({"Purchase ID": "count", "Price":"sum"}).sort_values("Purchase ID", ascending = False).head(5)
y.reset_index(inplace = True)
y.rename(columns = {"Purchase ID" : "Purchase Count", "Price":"Total Purchase Value"}, inplace = True)
y["Item Price"] = y["Total Purchase Value"]/y["Purchase Count"]
y

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
      <th>Item Price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>94</td>
      <td>Mourning Blade</td>
      <td>3</td>
      <td>10.92</td>
      <td>3.64</td>
    </tr>
    <tr>
      <th>1</th>
      <td>90</td>
      <td>Betrayer</td>
      <td>2</td>
      <td>8.24</td>
      <td>4.12</td>
    </tr>
    <tr>
      <th>2</th>
      <td>111</td>
      <td>Misery's End</td>
      <td>2</td>
      <td>3.58</td>
      <td>1.79</td>
    </tr>
    <tr>
      <th>3</th>
      <td>64</td>
      <td>Fusion Pummel</td>
      <td>2</td>
      <td>4.84</td>
      <td>2.42</td>
    </tr>
    <tr>
      <th>4</th>
      <td>154</td>
      <td>Feral Katana</td>
      <td>2</td>
      <td>8.22</td>
      <td>4.11</td>
    </tr>
  </tbody>
</table>
</div>



# Most Profitable Items

Identify the 5 most profitable items by total purchase value, then list (in a table):

Item ID

Item Name

Purchase Count

Item Price

Total Purchase Value


```python
h = purchase.groupby(["Item ID", "Item Name"])
r = h.agg({"Purchase ID": "count", "Price":"sum"}).sort_values("Price", ascending = False).head(5)
r.reset_index(inplace = True)
r.rename(columns = {"Purchase ID" : "Purchase Count", "Price":"Total Purchase Value"}, inplace = True)
r["Item Price"] = r["Total Purchase Value"]/r["Purchase Count"]
r
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
      <th>Item Price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>94</td>
      <td>Mourning Blade</td>
      <td>3</td>
      <td>10.92</td>
      <td>3.64</td>
    </tr>
    <tr>
      <th>1</th>
      <td>117</td>
      <td>Heartstriker, Legacy of the Light</td>
      <td>2</td>
      <td>9.42</td>
      <td>4.71</td>
    </tr>
    <tr>
      <th>2</th>
      <td>93</td>
      <td>Apocalyptic Battlescythe</td>
      <td>2</td>
      <td>8.98</td>
      <td>4.49</td>
    </tr>
    <tr>
      <th>3</th>
      <td>90</td>
      <td>Betrayer</td>
      <td>2</td>
      <td>8.24</td>
      <td>4.12</td>
    </tr>
    <tr>
      <th>4</th>
      <td>154</td>
      <td>Feral Katana</td>
      <td>2</td>
      <td>8.22</td>
      <td>4.11</td>
    </tr>
  </tbody>
</table>
</div>




```python
`
```
