# Chicago Census Data - SQL (Python)

# Introduction

Using this Python notebook I will:

1.  Explore 3 Chicago datasets  
2.  Load the 3 datasets into 3 tables in a Db2 database
3.  Execute SQL queries  


## Understanding the datasets

To complete this notebook I will be using three datasets that are available on the city of Chicago's Data Portal:

1.  <a href="https://data.cityofchicago.org/Health-Human-Services/Census-Data-Selected-socioeconomic-indicators-in-C/kn9c-c2s2">Socioeconomic Indicators in Chicago</a>
2.  <a href="https://data.cityofchicago.org/Education/Chicago-Public-Schools-Progress-Report-Cards-2011-/9xs2-f89t">Chicago Public Schools</a>
3.  <a href="https://data.cityofchicago.org/Public-Safety/Crimes-2001-to-present/ijzp-q8t2">Chicago Crime Data</a>

### 1. Socioeconomic Indicators in Chicago

This dataset contains a selection of six socioeconomic indicators of public health significance and a “hardship index,” for each Chicago community area, for the years 2008 – 2012.

For this notebook I will use a snapshot of this dataset which can be downloaded from:<a href="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork/labs/FinalModule_SKO/data/Census_Data_-_Selected_socioeconomic_indicators_in_Chicago__2008___2012-v2.csv" target="_blank"> Census Data </a>

A detailed description of this dataset and the original dataset can be obtained from the Chicago Data Portal at:
[https://data.cityofchicago.org/Health-Human-Services/Census-Data-Selected-socioeconomic-indicators-in-C/kn9c-c2s2](https://data.cityofchicago.org/Health-Human-Services/Census-Data-Selected-socioeconomic-indicators-in-C/kn9c-c2s2?cm_mmc=Email_Newsletter-_-Developer_Ed%2BTech-_-WW_WW-_-SkillsNetwork-Courses-IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork-20127838&cm_mmca1=000026UJ&cm_mmca2=10006555&cm_mmca3=M12345678&cvosrc=email.Newsletter.M12345678&cvo_campaign=000026UJ&cm_mmc=Email_Newsletter-_-Developer_Ed%2BTech-_-WW_WW-_-SkillsNetwork-Courses-IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork-20127838&cm_mmca1=000026UJ&cm_mmca2=10006555&cm_mmca3=M12345678&cvosrc=email.Newsletter.M12345678&cvo_campaign=000026UJ)

### 2. Chicago Public Schools

This dataset shows all school level performance data used to create CPS School Report Cards for the 2011-2012 school year. This dataset is provided by the city of Chicago's Data Portal.

For this notebook I will use a snapshot of this dataset which can be downloaded from: <a href="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork/labs/FinalModule_SKO/data/Chicago_Public_Schools_-_Progress_Report_Cards__2011-2012-v3.csv" target="_blank"> Chicago Public School </a>

A detailed description of this dataset and the original dataset can be obtained from the Chicago Data Portal at:
[https://data.cityofchicago.org/Education/Chicago-Public-Schools-Progress-Report-Cards-2011-/9xs2-f89t](https://data.cityofchicago.org/Education/Chicago-Public-Schools-Progress-Report-Cards-2011-/9xs2-f89t?cm_mmc=Email_Newsletter-_-Developer_Ed%2BTech-_-WW_WW-_-SkillsNetwork-Courses-IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork-20127838&cm_mmca1=000026UJ&cm_mmca2=10006555&cm_mmca3=M12345678&cvosrc=email.Newsletter.M12345678&cvo_campaign=000026UJ)

### 3. Chicago Crime Data

This dataset reflects reported incidents of crime (with the exception of murders where data exists for each victim) that occurred in the City of Chicago from 2001 to present, minus the most recent seven days. 

This dataset is quite large - over 1.5GB in size with over 6.5 million rows. For the purposes of this notebook I will use a much smaller sample of this dataset which can be downloaded from:<a href="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork/labs/FinalModule_SKO/data/Chicago_Crime_Data-v2.csv" target="_blank"> Chicago Crime Data </a>

A detailed description of this dataset and the original dataset can be obtained from the Chicago Data Portal at:
[https://data.cityofchicago.org/Public-Safety/Crimes-2001-to-present/ijzp-q8t2](https://data.cityofchicago.org/Public-Safety/Crimes-2001-to-present/ijzp-q8t2?cm_mmc=Email_Newsletter-_-Developer_Ed%2BTech-_-WW_WW-_-SkillsNetwork-Courses-IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork-20127838&cm_mmca1=000026UJ&cm_mmca2=10006555&cm_mmca3=M12345678&cvosrc=email.Newsletter.M12345678&cvo_campaign=000026UJ)


### Downloading the datasets

.CSV files:

1.  **CENSUS_DATA:** <a href="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork/labs/FinalModule_Coursera/data/Census_Data_-_Selected_socioeconomic_indicators_in_Chicago__2008___2012-v2.csv" target="_blank">Census Dataset</a>

2.  **CHICAGO_PUBLIC_SCHOOLS**  <a href="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork/labs/FinalModule_Coursera/data/Chicago_Public_Schools_-_Progress_Report_Cards__2011-2012-v3.csv" target="_blank"> Chicago Public School</a>

3.  **CHICAGO_CRIME_DATA:** <a href="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork/labs/FinalModule_Coursera/data/Chicago_Crime_Data-v2.csv" target="_blank"> Chicago Crime Data </a>




### Storing the datasets in database tables

To analyze the data using SQL, it first needs to be stored in the database.

While it is easier to read the dataset into a Pandas dataframe and then PERSIST it into the database, its results in mapping to default datatypes which may not be optimal for SQL querying. For example a long textual field may map to a CLOB instead of a VARCHAR. 

I loaded to the datsets into the IBM DB2 cloud console.



### Connecting to the database

I first load the SQL extension and establish a connection with the database



```python
%load_ext sql
```

Then in the next cell, enter the db2 connection string. 


```python
# Connection string is in the format:
# %sql ibm_db_sa://my-username:my-password@my-hostname:my-port/my-db-name
# Entered the connection string for the Db2 on Cloud database instance below
%sql ibm_db_sa://fwh43441:nc7qjqmk-czlqdbv@dashdb-txn-sbox-yp-dal09-08.services.dal.bluemix.net:50000/BLUDB
```




    'Connected: fwh43441@BLUDB'



## Exploring the Dataset


##### Finding the total number of crimes recorded in the CRIME table



```python
# Rows in Crime table
%sql Select Count(*) FROM CHICAGO_CRIME_DATA;
```

     * ibm_db_sa://fwh43441:***@dashdb-txn-sbox-yp-dal09-08.services.dal.bluemix.net:50000/BLUDB
    Done.
    




<table>
    <thead>
        <tr>
            <th>1</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>533</td>
        </tr>
    </tbody>
</table>





##### Retrieving the first 10 rows from the CRIME table



```python
%sql Select * from CHICAGO_CRIME_DATA LIMIT 10;
```

     * ibm_db_sa://fwh43441:***@dashdb-txn-sbox-yp-dal09-08.services.dal.bluemix.net:50000/BLUDB
    Done.
    




<table>
    <thead>
        <tr>
            <th>id</th>
            <th>case_number</th>
            <th>DATE</th>
            <th>block</th>
            <th>iucr</th>
            <th>primary_type</th>
            <th>description</th>
            <th>location_description</th>
            <th>arrest</th>
            <th>domestic</th>
            <th>beat</th>
            <th>district</th>
            <th>ward</th>
            <th>community_area_number</th>
            <th>fbicode</th>
            <th>x_coordinate</th>
            <th>y_coordinate</th>
            <th>YEAR</th>
            <th>updatedon</th>
            <th>latitude</th>
            <th>longitude</th>
            <th>location</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>3512276</td>
            <td>HK587712</td>
            <td>2004-08-28 17:50:56</td>
            <td>047XX S KEDZIE AVE</td>
            <td>890</td>
            <td>THEFT</td>
            <td>FROM BUILDING</td>
            <td>SMALL RETAIL STORE</td>
            <td>FALSE</td>
            <td>FALSE</td>
            <td>911</td>
            <td>9</td>
            <td>14</td>
            <td>58</td>
            <td>6</td>
            <td>1155838</td>
            <td>1873050</td>
            <td>2004</td>
            <td>2018-02-10 15:50:01</td>
            <td>41.80744050</td>
            <td>-87.70395585</td>
            <td>(41.8074405, -87.703955849)</td>
        </tr>
        <tr>
            <td>3406613</td>
            <td>HK456306</td>
            <td>2004-06-26 12:40:00</td>
            <td>009XX N CENTRAL PARK AVE</td>
            <td>820</td>
            <td>THEFT</td>
            <td>$500 AND UNDER</td>
            <td>OTHER</td>
            <td>FALSE</td>
            <td>FALSE</td>
            <td>1112</td>
            <td>11</td>
            <td>27</td>
            <td>23</td>
            <td>6</td>
            <td>1152206</td>
            <td>1906127</td>
            <td>2004</td>
            <td>2018-02-28 15:56:25</td>
            <td>41.89827996</td>
            <td>-87.71640551</td>
            <td>(41.898279962, -87.716405505)</td>
        </tr>
        <tr>
            <td>8002131</td>
            <td>HT233595</td>
            <td>2011-04-04 05:45:00</td>
            <td>043XX S WABASH AVE</td>
            <td>820</td>
            <td>THEFT</td>
            <td>$500 AND UNDER</td>
            <td>NURSING HOME/RETIREMENT HOME</td>
            <td>FALSE</td>
            <td>FALSE</td>
            <td>221</td>
            <td>2</td>
            <td>3</td>
            <td>38</td>
            <td>6</td>
            <td>1177436</td>
            <td>1876313</td>
            <td>2011</td>
            <td>2018-02-10 15:50:01</td>
            <td>41.81593313</td>
            <td>-87.62464213</td>
            <td>(41.815933131, -87.624642127)</td>
        </tr>
        <tr>
            <td>7903289</td>
            <td>HT133522</td>
            <td>2010-12-30 16:30:00</td>
            <td>083XX S KINGSTON AVE</td>
            <td>840</td>
            <td>THEFT</td>
            <td>FINANCIAL ID THEFT: OVER $300</td>
            <td>RESIDENCE</td>
            <td>FALSE</td>
            <td>FALSE</td>
            <td>423</td>
            <td>4</td>
            <td>7</td>
            <td>46</td>
            <td>6</td>
            <td>1194622</td>
            <td>1850125</td>
            <td>2010</td>
            <td>2018-02-10 15:50:01</td>
            <td>41.74366532</td>
            <td>-87.56246276</td>
            <td>(41.743665322, -87.562462756)</td>
        </tr>
        <tr>
            <td>10402076</td>
            <td>HZ138551</td>
            <td>2016-02-02 19:30:00</td>
            <td>033XX W 66TH ST</td>
            <td>820</td>
            <td>THEFT</td>
            <td>$500 AND UNDER</td>
            <td>ALLEY</td>
            <td>FALSE</td>
            <td>FALSE</td>
            <td>831</td>
            <td>8</td>
            <td>15</td>
            <td>66</td>
            <td>6</td>
            <td>1155240</td>
            <td>1860661</td>
            <td>2016</td>
            <td>2018-02-10 15:50:01</td>
            <td>41.77345530</td>
            <td>-87.70648047</td>
            <td>(41.773455295, -87.706480471)</td>
        </tr>
        <tr>
            <td>7732712</td>
            <td>HS540106</td>
            <td>2010-09-29 07:59:00</td>
            <td>006XX W CHICAGO AVE</td>
            <td>810</td>
            <td>THEFT</td>
            <td>OVER $500</td>
            <td>PARKING LOT/GARAGE(NON.RESID.)</td>
            <td>FALSE</td>
            <td>FALSE</td>
            <td>1323</td>
            <td>12</td>
            <td>27</td>
            <td>24</td>
            <td>6</td>
            <td>1171668</td>
            <td>1905607</td>
            <td>2010</td>
            <td>2018-02-10 15:50:01</td>
            <td>41.89644677</td>
            <td>-87.64493868</td>
            <td>(41.896446772, -87.644938678)</td>
        </tr>
        <tr>
            <td>10769475</td>
            <td>HZ534771</td>
            <td>2016-11-30 01:15:00</td>
            <td>050XX N KEDZIE AVE</td>
            <td>810</td>
            <td>THEFT</td>
            <td>OVER $500</td>
            <td>STREET</td>
            <td>FALSE</td>
            <td>FALSE</td>
            <td>1713</td>
            <td>17</td>
            <td>33</td>
            <td>14</td>
            <td>6</td>
            <td>1154133</td>
            <td>1933314</td>
            <td>2016</td>
            <td>2018-02-10 15:50:01</td>
            <td>41.97284491</td>
            <td>-87.70860008</td>
            <td>(41.972844913, -87.708600079)</td>
        </tr>
        <tr>
            <td>4494340</td>
            <td>HL793243</td>
            <td>2005-12-16 16:45:00</td>
            <td>005XX E PERSHING RD</td>
            <td>860</td>
            <td>THEFT</td>
            <td>RETAIL THEFT</td>
            <td>GROCERY FOOD STORE</td>
            <td>TRUE</td>
            <td>FALSE</td>
            <td>213</td>
            <td>2</td>
            <td>3</td>
            <td>38</td>
            <td>6</td>
            <td>1180448</td>
            <td>1879234</td>
            <td>2005</td>
            <td>2018-02-28 15:56:25</td>
            <td>41.82387989</td>
            <td>-87.61350386</td>
            <td>(41.823879885, -87.613503857)</td>
        </tr>
        <tr>
            <td>3778925</td>
            <td>HL149610</td>
            <td>2005-01-28 17:00:00</td>
            <td>100XX S WASHTENAW AVE</td>
            <td>810</td>
            <td>THEFT</td>
            <td>OVER $500</td>
            <td>STREET</td>
            <td>FALSE</td>
            <td>FALSE</td>
            <td>2211</td>
            <td>22</td>
            <td>19</td>
            <td>72</td>
            <td>6</td>
            <td>1160129</td>
            <td>1838040</td>
            <td>2005</td>
            <td>2018-02-28 15:56:25</td>
            <td>41.71128051</td>
            <td>-87.68917910</td>
            <td>(41.711280513, -87.689179097)</td>
        </tr>
        <tr>
            <td>3324217</td>
            <td>HK361551</td>
            <td>2004-05-13 14:15:00</td>
            <td>033XX W BELMONT AVE</td>
            <td>820</td>
            <td>THEFT</td>
            <td>$500 AND UNDER</td>
            <td>SMALL RETAIL STORE</td>
            <td>FALSE</td>
            <td>FALSE</td>
            <td>1733</td>
            <td>17</td>
            <td>35</td>
            <td>21</td>
            <td>6</td>
            <td>1153590</td>
            <td>1921084</td>
            <td>2004</td>
            <td>2018-02-28 15:56:25</td>
            <td>41.93929582</td>
            <td>-87.71092344</td>
            <td>(41.939295821, -87.710923442)</td>
        </tr>
    </tbody>
</table>





##### How many crimes involve an arrest?



```python
%sql Select COUNT(*) from CHICAGO_CRIME_DATA Where arrest = "TRUE";
```

     * ibm_db_sa://fwh43441:***@dashdb-txn-sbox-yp-dal09-08.services.dal.bluemix.net:50000/BLUDB
    Done.
    




<table>
    <thead>
        <tr>
            <th>1</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>163</td>
        </tr>
    </tbody>
</table>





##### Which unique types of crimes have been recorded at GAS STATION locations?



```python
%sql Select distinct primary_type from CHICAGO_CRIME_DATA where location_description = 'GAS STATION';
```

     * ibm_db_sa://fwh43441:***@dashdb-txn-sbox-yp-dal09-08.services.dal.bluemix.net:50000/BLUDB
    Done.
    




<table>
    <thead>
        <tr>
            <th>primary_type</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>CRIMINAL TRESPASS</td>
        </tr>
        <tr>
            <td>NARCOTICS</td>
        </tr>
        <tr>
            <td>ROBBERY</td>
        </tr>
        <tr>
            <td>THEFT</td>
        </tr>
    </tbody>
</table>





##### In the CENUS_DATA table: Listing all Community Areas whose names start with the letter ‘B’.



```python
%sql Select COMMUNITY_AREA_NAME from CENSUS where COMMUNITY_AREA_NAME like'B%';
```

     * ibm_db_sa://fwh43441:***@dashdb-txn-sbox-yp-dal09-08.services.dal.bluemix.net:50000/BLUDB
    Done.
    




<table>
    <thead>
        <tr>
            <th>community_area_name</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Belmont Cragin</td>
        </tr>
        <tr>
            <td>Burnside</td>
        </tr>
        <tr>
            <td>Brighton Park</td>
        </tr>
        <tr>
            <td>Bridgeport</td>
        </tr>
        <tr>
            <td>Beverly</td>
        </tr>
    </tbody>
</table>





##### Which schools in Community Areas 10 to 15 are healthy school certified?



```python
%sql select name_of_school from SCHOOLS where community_area_number >= 10 and community_area_number <=15 and healthy_school_certified = 'Yes';
```

     * ibm_db_sa://fwh43441:***@dashdb-txn-sbox-yp-dal09-08.services.dal.bluemix.net:50000/BLUDB
    Done.
    




<table>
    <thead>
        <tr>
            <th>name_of_school</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Rufus M Hitch Elementary School</td>
        </tr>
    </tbody>
</table>





##### What is the average school Safety Score?



```python
%sql Select AVG(SAFETY_SCORE)as AVG_SAFETY_SCORE FROM SCHOOLS;
```

     * ibm_db_sa://fwh43441:***@dashdb-txn-sbox-yp-dal09-08.services.dal.bluemix.net:50000/BLUDB
    Done.
    




<table>
    <thead>
        <tr>
            <th>avg_safety_score</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>49.504873</td>
        </tr>
    </tbody>
</table>





##### Listing the top 5 Community Areas by average College Enrollment [number of students]



```python
%sql select community_area_name as comm_area_name, avg(college_enrollment) as avg_enroll \
from SCHOOLS group by community_area_name \
order by avg_enroll desc limit 5;
```

     * ibm_db_sa://fwh43441:***@dashdb-txn-sbox-yp-dal09-08.services.dal.bluemix.net:50000/BLUDB
    Done.
    




<table>
    <thead>
        <tr>
            <th>comm_area_name</th>
            <th>avg_enroll</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ARCHER HEIGHTS</td>
            <td>2411.500000</td>
        </tr>
        <tr>
            <td>MONTCLARE</td>
            <td>1317.000000</td>
        </tr>
        <tr>
            <td>WEST ELSDON</td>
            <td>1233.333333</td>
        </tr>
        <tr>
            <td>BRIGHTON PARK</td>
            <td>1205.875000</td>
        </tr>
        <tr>
            <td>BELMONT CRAGIN</td>
            <td>1198.833333</td>
        </tr>
    </tbody>
</table>





##### Using a sub-query to determine which Community Area has the least value for school Safety Score



```python
%sql select COMMUNITY_AREA_NAME,SAFETY_SCORE from SCHOOLS \
WHERE SAFETY_SCORE = (SELECT MIN(SAFETY_SCORE) FROM SCHOOLS);
```

     * ibm_db_sa://fwh43441:***@dashdb-txn-sbox-yp-dal09-08.services.dal.bluemix.net:50000/BLUDB
    Done.
    




<table>
    <thead>
        <tr>
            <th>community_area_name</th>
            <th>safety_score</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>WASHINGTON PARK</td>
            <td>1</td>
        </tr>
    </tbody>
</table>



Finding the Per Capita Income of the Community Area which has a school Safety Score of 1.



```python
%sql select  COMMUNITY_AREA_NAME,per_capita_income \
from CENSUS \
where community_area_number = (select community_area_number from SCHOOLS where safety_score = 1);

```

    UsageError: Line magic function `%sql` not found.
    
