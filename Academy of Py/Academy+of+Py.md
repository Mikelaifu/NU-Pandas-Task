
# PyCity School Analysis

Observe Trend1: In terms of grade within those schools in the database, schools are doing excellent in teaching math(100% passing rate) and pretty decent amount of student passed reading and overall passing rate is extremely high in those school.

Observe Trend2: In terms of school type, Charter schools are generally doing better than District school especially in reading teaching.

Observe Trend3: Schools usually have fewer student tend to have better grade in general. 


```python
import pandas as pd
import numpy as np
filepath = ("generated_data/schools_complete.csv")
school = pd.read_csv(filepath)
school
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
      <th>School ID</th>
      <th>school_name</th>
      <th>type</th>
      <th>size</th>
      <th>budget</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Miller High School</td>
      <td>Charter</td>
      <td>2424</td>
      <td>1418040</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Sherman High School</td>
      <td>District</td>
      <td>3213</td>
      <td>2152710</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Galloway High School</td>
      <td>Charter</td>
      <td>2471</td>
      <td>1445535</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Smith High School</td>
      <td>District</td>
      <td>4954</td>
      <td>3210192</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Kelly High School</td>
      <td>District</td>
      <td>3307</td>
      <td>2225611</td>
    </tr>
    <tr>
      <th>5</th>
      <td>5</td>
      <td>Hawkins High School</td>
      <td>District</td>
      <td>4555</td>
      <td>2851430</td>
    </tr>
    <tr>
      <th>6</th>
      <td>6</td>
      <td>Gonzalez High School</td>
      <td>Charter</td>
      <td>1855</td>
      <td>1192765</td>
    </tr>
    <tr>
      <th>7</th>
      <td>7</td>
      <td>Glass High School</td>
      <td>District</td>
      <td>3271</td>
      <td>2155589</td>
    </tr>
    <tr>
      <th>8</th>
      <td>8</td>
      <td>Gomez High School</td>
      <td>Charter</td>
      <td>2154</td>
      <td>1288092</td>
    </tr>
    <tr>
      <th>9</th>
      <td>9</td>
      <td>Campbell High School</td>
      <td>Charter</td>
      <td>271</td>
      <td>157993</td>
    </tr>
    <tr>
      <th>10</th>
      <td>10</td>
      <td>Macdonald High School</td>
      <td>Charter</td>
      <td>901</td>
      <td>550511</td>
    </tr>
  </tbody>
</table>
</div>




```python
filepath2 = ("generated_data/students_complete.csv")
student = pd.read_csv(filepath2)
student["math_score"] < 60
```




    0        False
    1        False
    2        False
    3        False
    4        False
    5        False
    6        False
    7        False
    8        False
    9        False
    10       False
    11       False
    12       False
    13       False
    14       False
    15       False
    16       False
    17       False
    18       False
    19       False
    20       False
    21       False
    22       False
    23       False
    24       False
    25       False
    26       False
    27       False
    28       False
    29       False
             ...  
    29346    False
    29347    False
    29348    False
    29349    False
    29350    False
    29351    False
    29352    False
    29353    False
    29354    False
    29355    False
    29356    False
    29357    False
    29358    False
    29359    False
    29360    False
    29361    False
    29362    False
    29363    False
    29364    False
    29365    False
    29366    False
    29367    False
    29368    False
    29369    False
    29370    False
    29371    False
    29372    False
    29373    False
    29374    False
    29375    False
    Name: math_score, Length: 29376, dtype: bool



# District Summary

Create a high level snapshot (in table form) of the district's key metrics, including:

Total Schools

Total Students

Total Budget

Average Math Score

Average Reading Score

% Passing Math

% Passing Reading

Overall Passing Rate (Average of the above two)





```python


a = {"Total School":[school["School ID"].count()]}
b = {"Total Students" : [student["Student ID"].count()]}
c = {"Total Budget": [school["budget"].sum()]}
d = {"Average Math Score" : [student["math_score"].mean()]}
e = {"Average Reading Score" : [student["reading_score"].mean()]}

reading_rate = student["reading_score"][student["reading_score"] >= 60].count()/student["reading_score"].count()
math_rate = student["math_score"][student["math_score"] >= 60].count()/student["math_score"].count()

f = {"% Passing Math" : [math_rate]}

g={"%Passing Reading" : [reading_rate]}
h= {"% Over Passing Rate": [(math_rate + reading_rate)/2]}

x = pd.DataFrame(
              {"% Over Passing Rate": [(math_rate + reading_rate)/2],
               "%Passing Reading" : [reading_rate],
               "% Passing Math" : [math_rate],
               "Average Reading Score" : [student["reading_score"].mean()],
               "Average Math Score" : [student["math_score"].mean()],
               "Total Budget": [school["budget"].sum()],
               "Total Students" : [student["Student ID"].count()],
              "Total School": [school["School ID"].count()]}
)


x["%Passing Reading"] = (x["%Passing Reading"]*100).map("%{:.2f}".format)
x["% Over Passing Rate"] = (x["% Over Passing Rate"]*100).map("%{:.2f}".format)
x["% Passing Math"] = (x["% Passing Math"]*100).map("%{:.2f}".format)
x[["Total School","Total Students","Total Budget", "Average Math Score","Average Reading Score","% Passing Math","%Passing Reading", "% Over Passing Rate"    ]]


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
      <th>Total School</th>
      <th>Total Students</th>
      <th>Total Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>%Passing Reading</th>
      <th>% Over Passing Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>11</td>
      <td>29376</td>
      <td>18648468</td>
      <td>82.269846</td>
      <td>82.865877</td>
      <td>%100.00</td>
      <td>%92.77</td>
      <td>%96.38</td>
    </tr>
  </tbody>
</table>
</div>



# School Summary

Create an overview table that summarizes key metrics about each school, including:

School Name

School Type

Total Students

Total School Budget

Per Student Budget

Average Math Score

Average Reading Score

% Passing Math

% Passing Reading

Overall Passing Rate (Average of the above two)


```python
student.
```


```python
# get a datafram "o" to include total student, avg math score, avg reading score for each school 
student_school = student.groupby("school_name")
o = student_school.agg({"Student ID":"count" , "math_score" :"mean" , "reading_score" : "mean"})
o.rename(columns = {"Student ID": "Total Student", "math_score":"Avg Math Score", "reading_score":"Avg Reading Score"}, inplace = True)

# get a data frame "d" to include sudent %pass math, % pass reading, and total passing score, and student count for each school
mask = student["math_score"] >= 60
student1 = student[mask]
mask2 = student["reading_score"]>=60
student2 = student[mask2]
k = student1.groupby("school_name").agg({"math_score":"count"}).rename(columns ={"math_score" :"mathP_count"} )
j = student2.groupby("school_name").agg({"reading_score":"count"}).rename(columns = {"reading_score": "readingP_count"})
h = k.merge(j, how = "left", left_index = True, right_index = True)

i = student_school.agg({"math_score": "count", "reading_score":"count" })
d = h.merge(i, how = "left", right_index = True, left_index = True)

d["% Math Passing"] = d["mathP_count"]/d["math_score"]
d["% Reading Passing"] = d["readingP_count"]/d["reading_score"]
d["% overall passing"] = (d["% Math Passing"] + d["% Reading Passing"])/2
d.drop(columns = ["mathP_count", "readingP_count", "math_score", "reading_score" ], inplace = True)

# to get a data frame "school2" to include type, and budget for each school 
school2 = school[["school_name", "type", "budget"]].set_index("school_name").sort_index()


# merge, o, d, and school2 to get a final daat frame  
FinalReport = d.merge(o, how = "left",right_index = True, left_index = True ).merge(school2, how = "left",right_index = True, left_index = True)

FinalReport['Budget per student'] = FinalReport["budget"]/FinalReport["Total Student"]

FinalReport = FinalReport[["Total Student", "type", "Total Student", "Avg Math Score", "Avg Reading Score", "budget", "Budget per student", "% Math Passing", "% Reading Passing", "% overall passing"]]


FinalReport
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
      <th>Total Student</th>
      <th>type</th>
      <th>Total Student</th>
      <th>Avg Math Score</th>
      <th>Avg Reading Score</th>
      <th>budget</th>
      <th>Budget per student</th>
      <th>% Math Passing</th>
      <th>% Reading Passing</th>
      <th>% overall passing</th>
    </tr>
    <tr>
      <th>school_name</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Campbell High School</th>
      <td>271</td>
      <td>Charter</td>
      <td>271</td>
      <td>83.594096</td>
      <td>93.771218</td>
      <td>157993</td>
      <td>583.0</td>
      <td>1.0</td>
      <td>1.000000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>Galloway High School</th>
      <td>2471</td>
      <td>Charter</td>
      <td>2471</td>
      <td>83.566168</td>
      <td>94.029543</td>
      <td>1445535</td>
      <td>585.0</td>
      <td>1.0</td>
      <td>1.000000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>Glass High School</th>
      <td>3271</td>
      <td>District</td>
      <td>3271</td>
      <td>81.293183</td>
      <td>76.888108</td>
      <td>2155589</td>
      <td>659.0</td>
      <td>1.0</td>
      <td>0.887190</td>
      <td>0.943595</td>
    </tr>
    <tr>
      <th>Gomez High School</th>
      <td>2154</td>
      <td>Charter</td>
      <td>2154</td>
      <td>83.838440</td>
      <td>94.027391</td>
      <td>1288092</td>
      <td>598.0</td>
      <td>1.0</td>
      <td>1.000000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>Gonzalez High School</th>
      <td>1855</td>
      <td>Charter</td>
      <td>1855</td>
      <td>83.442588</td>
      <td>94.140701</td>
      <td>1192765</td>
      <td>643.0</td>
      <td>1.0</td>
      <td>1.000000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>Hawkins High School</th>
      <td>4555</td>
      <td>District</td>
      <td>4555</td>
      <td>81.723820</td>
      <td>77.005928</td>
      <td>2851430</td>
      <td>626.0</td>
      <td>1.0</td>
      <td>0.887157</td>
      <td>0.943578</td>
    </tr>
    <tr>
      <th>Kelly High School</th>
      <td>3307</td>
      <td>District</td>
      <td>3307</td>
      <td>81.678258</td>
      <td>76.829755</td>
      <td>2225611</td>
      <td>673.0</td>
      <td>1.0</td>
      <td>0.887511</td>
      <td>0.943756</td>
    </tr>
    <tr>
      <th>Macdonald High School</th>
      <td>901</td>
      <td>Charter</td>
      <td>901</td>
      <td>83.779134</td>
      <td>93.932297</td>
      <td>550511</td>
      <td>611.0</td>
      <td>1.0</td>
      <td>1.000000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>Miller High School</th>
      <td>2424</td>
      <td>Charter</td>
      <td>2424</td>
      <td>83.610149</td>
      <td>93.997525</td>
      <td>1418040</td>
      <td>585.0</td>
      <td>1.0</td>
      <td>1.000000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>Sherman High School</th>
      <td>3213</td>
      <td>District</td>
      <td>3213</td>
      <td>81.502023</td>
      <td>77.290694</td>
      <td>2152710</td>
      <td>670.0</td>
      <td>1.0</td>
      <td>0.894491</td>
      <td>0.947246</td>
    </tr>
    <tr>
      <th>Smith High School</th>
      <td>4954</td>
      <td>District</td>
      <td>4954</td>
      <td>81.539160</td>
      <td>77.146952</td>
      <td>3210192</td>
      <td>648.0</td>
      <td>1.0</td>
      <td>0.892814</td>
      <td>0.946407</td>
    </tr>
  </tbody>
</table>
</div>



# Top Performing Schools (By Passing Rate)

Create a table that highlights the top 5 performing schools based on Overall Passing Rate. Include:
    
School Name

School Type

Total Students

Total School Budget

Per Student Budget

Average Math Score

Average Reading Score

% Passing Math
% Passing Reading

Overall Passing Rate (Average of the above two)


```python
Top_performing = FinalReport.sort_values("% overall passing", ascending = False)

Top_performing["% Reading Passing"] = (Top_performing["% Reading Passing"]*100).map("%{:.2f}".format)
Top_performing["% overall passing"] = (Top_performing["% overall passing"]*100).map("%{:.2f}".format)
Top_performing["% Math Passing"] = (Top_performing["% Math Passing"]*100).map("%{:.2f}".format)

Top_performing.sort_values("% overall passing", ascending = True).head(5)
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
      <th>Total Student</th>
      <th>type</th>
      <th>Total Student</th>
      <th>Avg Math Score</th>
      <th>Avg Reading Score</th>
      <th>budget</th>
      <th>Budget per student</th>
      <th>% Math Passing</th>
      <th>% Reading Passing</th>
      <th>% overall passing</th>
    </tr>
    <tr>
      <th>school_name</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Campbell High School</th>
      <td>271</td>
      <td>Charter</td>
      <td>271</td>
      <td>83.594096</td>
      <td>93.771218</td>
      <td>157993</td>
      <td>583.0</td>
      <td>%100.00</td>
      <td>%100.00</td>
      <td>%100.00</td>
    </tr>
    <tr>
      <th>Galloway High School</th>
      <td>2471</td>
      <td>Charter</td>
      <td>2471</td>
      <td>83.566168</td>
      <td>94.029543</td>
      <td>1445535</td>
      <td>585.0</td>
      <td>%100.00</td>
      <td>%100.00</td>
      <td>%100.00</td>
    </tr>
    <tr>
      <th>Gomez High School</th>
      <td>2154</td>
      <td>Charter</td>
      <td>2154</td>
      <td>83.838440</td>
      <td>94.027391</td>
      <td>1288092</td>
      <td>598.0</td>
      <td>%100.00</td>
      <td>%100.00</td>
      <td>%100.00</td>
    </tr>
    <tr>
      <th>Gonzalez High School</th>
      <td>1855</td>
      <td>Charter</td>
      <td>1855</td>
      <td>83.442588</td>
      <td>94.140701</td>
      <td>1192765</td>
      <td>643.0</td>
      <td>%100.00</td>
      <td>%100.00</td>
      <td>%100.00</td>
    </tr>
    <tr>
      <th>Macdonald High School</th>
      <td>901</td>
      <td>Charter</td>
      <td>901</td>
      <td>83.779134</td>
      <td>93.932297</td>
      <td>550511</td>
      <td>611.0</td>
      <td>%100.00</td>
      <td>%100.00</td>
      <td>%100.00</td>
    </tr>
  </tbody>
</table>
</div>



# Top Performing Schools (By Passing Rate)

Create a table that highlights the bottom 5 performing schools based on Overall Passing Rate. Include all of the same metrics as above.



```python
Top_performing.sort_values("% overall passing").tail()
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
      <th>Total Student</th>
      <th>type</th>
      <th>Total Student</th>
      <th>Avg Math Score</th>
      <th>Avg Reading Score</th>
      <th>budget</th>
      <th>Budget per student</th>
      <th>% Math Passing</th>
      <th>% Reading Passing</th>
      <th>% overall passing</th>
    </tr>
    <tr>
      <th>school_name</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Glass High School</th>
      <td>3271</td>
      <td>District</td>
      <td>3271</td>
      <td>81.293183</td>
      <td>76.888108</td>
      <td>2155589</td>
      <td>659.0</td>
      <td>%100.00</td>
      <td>%88.72</td>
      <td>%94.36</td>
    </tr>
    <tr>
      <th>Hawkins High School</th>
      <td>4555</td>
      <td>District</td>
      <td>4555</td>
      <td>81.723820</td>
      <td>77.005928</td>
      <td>2851430</td>
      <td>626.0</td>
      <td>%100.00</td>
      <td>%88.72</td>
      <td>%94.36</td>
    </tr>
    <tr>
      <th>Kelly High School</th>
      <td>3307</td>
      <td>District</td>
      <td>3307</td>
      <td>81.678258</td>
      <td>76.829755</td>
      <td>2225611</td>
      <td>673.0</td>
      <td>%100.00</td>
      <td>%88.75</td>
      <td>%94.38</td>
    </tr>
    <tr>
      <th>Smith High School</th>
      <td>4954</td>
      <td>District</td>
      <td>4954</td>
      <td>81.539160</td>
      <td>77.146952</td>
      <td>3210192</td>
      <td>648.0</td>
      <td>%100.00</td>
      <td>%89.28</td>
      <td>%94.64</td>
    </tr>
    <tr>
      <th>Sherman High School</th>
      <td>3213</td>
      <td>District</td>
      <td>3213</td>
      <td>81.502023</td>
      <td>77.290694</td>
      <td>2152710</td>
      <td>670.0</td>
      <td>%100.00</td>
      <td>%89.45</td>
      <td>%94.72</td>
    </tr>
  </tbody>
</table>
</div>



# Math Scores by Grade

Create a table that lists the average Math Score for students of each grade level (9th, 10th, 11th, 12th) at each school.


```python
x = student.groupby(["school_name", "grade"]).agg({"math_score":"mean"})
x["math_score"]= round(x["math_score"], 2)
x.unstack()
#x.pivot(index = 'school_name', columns = "grade", values = 'math_score')
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead tr th {
        text-align: left;
    }

    .dataframe thead tr:last-of-type th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th colspan="4" halign="left">math_score</th>
    </tr>
    <tr>
      <th>grade</th>
      <th>10th</th>
      <th>11th</th>
      <th>12th</th>
      <th>9th</th>
    </tr>
    <tr>
      <th>school_name</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Campbell High School</th>
      <td>84.27</td>
      <td>83.94</td>
      <td>82.06</td>
      <td>83.84</td>
    </tr>
    <tr>
      <th>Galloway High School</th>
      <td>83.55</td>
      <td>83.98</td>
      <td>83.20</td>
      <td>83.53</td>
    </tr>
    <tr>
      <th>Glass High School</th>
      <td>81.04</td>
      <td>81.39</td>
      <td>80.82</td>
      <td>81.87</td>
    </tr>
    <tr>
      <th>Gomez High School</th>
      <td>83.97</td>
      <td>83.87</td>
      <td>83.83</td>
      <td>83.68</td>
    </tr>
    <tr>
      <th>Gonzalez High School</th>
      <td>83.95</td>
      <td>83.20</td>
      <td>82.84</td>
      <td>83.55</td>
    </tr>
    <tr>
      <th>Hawkins High School</th>
      <td>81.48</td>
      <td>81.89</td>
      <td>81.94</td>
      <td>81.67</td>
    </tr>
    <tr>
      <th>Kelly High School</th>
      <td>81.88</td>
      <td>81.50</td>
      <td>81.45</td>
      <td>81.79</td>
    </tr>
    <tr>
      <th>Macdonald High School</th>
      <td>83.81</td>
      <td>83.48</td>
      <td>83.52</td>
      <td>84.26</td>
    </tr>
    <tr>
      <th>Miller High School</th>
      <td>83.62</td>
      <td>83.64</td>
      <td>83.30</td>
      <td>83.82</td>
    </tr>
    <tr>
      <th>Sherman High School</th>
      <td>81.53</td>
      <td>81.23</td>
      <td>81.74</td>
      <td>81.50</td>
    </tr>
    <tr>
      <th>Smith High School</th>
      <td>81.00</td>
      <td>81.83</td>
      <td>81.55</td>
      <td>81.91</td>
    </tr>
  </tbody>
</table>
</div>



# Reading Scores by Grade

Create a table that lists the average Reading Score for students of each grade level (9th, 10th, 11th, 12th) at each school.



```python
y = student.groupby(["school_name", "grade"]).agg({"reading_score":"mean"})
y["reading_score"]= round(y["reading_score"], 2)
y.unstack()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead tr th {
        text-align: left;
    }

    .dataframe thead tr:last-of-type th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th colspan="4" halign="left">reading_score</th>
    </tr>
    <tr>
      <th>grade</th>
      <th>10th</th>
      <th>11th</th>
      <th>12th</th>
      <th>9th</th>
    </tr>
    <tr>
      <th>school_name</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Campbell High School</th>
      <td>93.88</td>
      <td>94.08</td>
      <td>93.71</td>
      <td>93.47</td>
    </tr>
    <tr>
      <th>Galloway High School</th>
      <td>93.96</td>
      <td>93.98</td>
      <td>94.13</td>
      <td>94.07</td>
    </tr>
    <tr>
      <th>Glass High School</th>
      <td>77.32</td>
      <td>77.13</td>
      <td>76.62</td>
      <td>76.44</td>
    </tr>
    <tr>
      <th>Gomez High School</th>
      <td>93.97</td>
      <td>93.81</td>
      <td>94.13</td>
      <td>94.19</td>
    </tr>
    <tr>
      <th>Gonzalez High School</th>
      <td>94.10</td>
      <td>94.42</td>
      <td>94.04</td>
      <td>94.04</td>
    </tr>
    <tr>
      <th>Hawkins High School</th>
      <td>77.17</td>
      <td>77.53</td>
      <td>76.85</td>
      <td>76.52</td>
    </tr>
    <tr>
      <th>Kelly High School</th>
      <td>77.27</td>
      <td>76.64</td>
      <td>76.97</td>
      <td>76.37</td>
    </tr>
    <tr>
      <th>Macdonald High School</th>
      <td>94.14</td>
      <td>93.80</td>
      <td>93.67</td>
      <td>94.05</td>
    </tr>
    <tr>
      <th>Miller High School</th>
      <td>94.04</td>
      <td>94.24</td>
      <td>93.82</td>
      <td>93.90</td>
    </tr>
    <tr>
      <th>Sherman High School</th>
      <td>77.11</td>
      <td>77.31</td>
      <td>77.50</td>
      <td>77.29</td>
    </tr>
    <tr>
      <th>Smith High School</th>
      <td>76.81</td>
      <td>77.34</td>
      <td>77.75</td>
      <td>76.86</td>
    </tr>
  </tbody>
</table>
</div>



# Scores by School Spending

Create a table that breaks down school performances based on average Spending Ranges (Per Student). Use 4 reasonable bins to group school spending. Include in the table each of the following:

Average Math Score

Average Reading Score

% Passing Math

% Passing Reading

Overall Passing Rate (Average of the above two


```python
NewFinal= FinalReport.copy()
bins = [580, 606 , 628, 650, 680]
binname = ["580-606", "606-628", "628-650", "650-680"]
NewFinal["School Spending (Per Student)"]= pd.cut(NewFinal["Budget per student"], bins, labels = binname)
schoolspending = NewFinal.groupby("School Spending (Per Student)").agg({"Avg Math Score":"mean", "Avg Reading Score":"mean", "% Math Passing":"mean", "% Reading Passing": "mean", "% overall passing":"mean"})
schoolspending = round(schoolspending[["Avg Math Score", "Avg Reading Score", "% Math Passing", "% Reading Passing", "% overall passing"]], 2)

schoolspending["% overall passing"] = (schoolspending["% overall passing"]*100).map("%{:.2f}".format)
schoolspending["% Reading Passing"] = (schoolspending["% Reading Passing"]*100).map("%{:.2f}".format)
schoolspending["% Math Passing"] = (schoolspending["% Math Passing"]*100).map("%{:.2f}".format)
schoolspending
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
      <th>Avg Math Score</th>
      <th>Avg Reading Score</th>
      <th>% Math Passing</th>
      <th>% Reading Passing</th>
      <th>% overall passing</th>
    </tr>
    <tr>
      <th>School Spending (Per Student)</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>580-606</th>
      <td>83.65</td>
      <td>93.96</td>
      <td>%100.00</td>
      <td>%100.00</td>
      <td>%100.00</td>
    </tr>
    <tr>
      <th>606-628</th>
      <td>82.75</td>
      <td>85.47</td>
      <td>%100.00</td>
      <td>%94.00</td>
      <td>%97.00</td>
    </tr>
    <tr>
      <th>628-650</th>
      <td>82.49</td>
      <td>85.64</td>
      <td>%100.00</td>
      <td>%95.00</td>
      <td>%97.00</td>
    </tr>
    <tr>
      <th>650-680</th>
      <td>81.49</td>
      <td>77.00</td>
      <td>%100.00</td>
      <td>%89.00</td>
      <td>%94.00</td>
    </tr>
  </tbody>
</table>
</div>



# Scores by School Size

Repeat the above breakdown, but this time group schools based on a reasonable approximation of school size (Small, Medium, Large).



```python
NewFinal2 = FinalReport.copy()
school2 = school.copy()
school2 = school2.sort_values("school_name")

size = []
for i in school2["size"]: 
    size.append(i)
NewFinal2["size"] = size
bins = [0, 1800, 3400, 5000 ]
binsname = ["Small", "Medium", "Large"]
NewFinal2["Size Level"] = pd.cut(NewFinal2["size"], bins, labels = binsname)
SchoolSize = NewFinal2.groupby("Size Level").agg({"Avg Math Score":"mean", "Avg Reading Score":"mean", "% Math Passing":"mean", "% Reading Passing": "mean", "% overall passing":"mean"})
SchoolSize = round(SchoolSize[["Avg Math Score", "Avg Reading Score", "% Math Passing", "% Reading Passing", "% overall passing"]], 2)

SchoolSize["% overall passing"] = (SchoolSize["% overall passing"]*100).map("%{:.2f}".format)
SchoolSize["% Reading Passing"] = (SchoolSize["% Reading Passing"]*100).map("%{:.2f}".format)
SchoolSize["% Math Passing"] = (SchoolSize["% Math Passing"]*100).map("%{:.2f}".format)
SchoolSize



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
      <th>Avg Math Score</th>
      <th>Avg Reading Score</th>
      <th>% Math Passing</th>
      <th>% Reading Passing</th>
      <th>% overall passing</th>
    </tr>
    <tr>
      <th>Size Level</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Small</th>
      <td>83.69</td>
      <td>93.85</td>
      <td>%100.00</td>
      <td>%100.00</td>
      <td>%100.00</td>
    </tr>
    <tr>
      <th>Medium</th>
      <td>82.70</td>
      <td>86.74</td>
      <td>%100.00</td>
      <td>%95.00</td>
      <td>%98.00</td>
    </tr>
    <tr>
      <th>Large</th>
      <td>81.63</td>
      <td>77.08</td>
      <td>%100.00</td>
      <td>%89.00</td>
      <td>%94.00</td>
    </tr>
  </tbody>
</table>
</div>



# Scores by School Type

Repeat the above breakdown, but this time group schools based on school type (Charter vs. District).



```python
NewFinal3 = FinalReport.copy()
NewFinal3["type"]
SchoolType = NewFinal3.groupby("type").agg({"Avg Math Score":"mean", "Avg Reading Score":"mean", "% Math Passing":"mean", "% Reading Passing": "mean", "% overall passing":"mean"})
SchoolType = round(SchoolType[["Avg Math Score", "Avg Reading Score", "% Math Passing", "% Reading Passing", "% overall passing"]], 2)


SchoolType["% overall passing"] = (SchoolType["% overall passing"]*100).map("%{:.2f}".format)
SchoolType["% Reading Passing"] = (SchoolType["% Reading Passing"]*100).map("%{:.2f}".format)
SchoolType["% Math Passing"] = (SchoolType["% Math Passing"]*100).map("%{:.2f}".format)

SchoolType
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
      <th>Avg Math Score</th>
      <th>Avg Reading Score</th>
      <th>% Math Passing</th>
      <th>% Reading Passing</th>
      <th>% overall passing</th>
    </tr>
    <tr>
      <th>type</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Charter</th>
      <td>83.64</td>
      <td>93.98</td>
      <td>%100.00</td>
      <td>%100.00</td>
      <td>%100.00</td>
    </tr>
    <tr>
      <th>District</th>
      <td>81.55</td>
      <td>77.03</td>
      <td>%100.00</td>
      <td>%89.00</td>
      <td>%94.00</td>
    </tr>
  </tbody>
</table>
</div>


