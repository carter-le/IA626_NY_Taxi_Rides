# IA626_NY_Taxi_Rides
 
# NY Taxi Rides 

#### Introduction 

For this project I have analyzed a subset of a dataset of NYC Taxi Rides, which will be referred to in this document as the dataset. 

Because the dataset is so large, I could not load it into memory and then perform functions on it. I instead had to analyze the data line by line. 

To start the process, I loaded the file and defined the reader. 

``` python
import csv
import time

start = time.time()
fn = 'trip_data_10.csv'
f = open(fn,"r")
reader = csv.reader(f) 
```



#### How many rows are in the data set? 

There are a total of 15,004,557 rows in the data set. This was determined by counting the number of rows using a simple counter (in this case n) using the following code:

``` python
n = 0
for row in reader:
    n+=1
print(n)
```

The loop:

``` python
for row in reader:
```

is the method I use for this and all subsequent functions to iterate through the dataset without loading it into memory. 



#### Time range data covers: 

It is important to filter this data such that we throw away any values which are not in the proper datetime format or which are impossible dates (ex. the 88th of March). We do this by creating a variable, dts, which is row[5] and row[6], the two datetime columns in the dataset. We then set another variable, fdt, to None type, and use the following code to test whether or not the date in any given row is in the correct datetime format:



``` python
if n > 0:    
        dts = row[5]
        dts2 = row[6]
        fdt = None 
        fdt2 = None
        try:
            fdt = datetime.datetime.strptime(dts, "%Y-%m-%d %H:%M:%S")
        except ValueError:
            invaliddtpickup += 1 
        try:
            fdt2 = datetime.datetime.strptime(dts2, "%Y-%m-%d %H:%M:%S")
        except ValueError:
            invaliddtdropoff += 1 
```



Only if the datetime is in proper format is it considered for the maximum and minimum datetimes: 

```python
if fdt is not None:
            if n ==1:
                mintime = row[5]
            elif n > 1:
                if row[5] < mintime:
                    mintime = row[5]
        
        if fdt2 is not None:
            if n ==1:
                maxtime = row[6]
            elif n > 1:
                if row[6] > maxtime:
                    maxtime = row[6]
    else:
        pass 
    n+=1
```



The reason this relatively arduous process is necessary is because the columns are currently read as strings, and python can not identify invalid datetimes when they are in string format. If the data was perfect, it would theoretically work for this data set because the datetimes are in order from year, month, day, time, etc. from largest unit of time to smallest. It is more robust to use this method of checking to make sure the data conforms to a proper datetime. 

Note: The try, except function does make this program slow. If this dataset was collected for more time (this set of data only covers only a month's worth of data) the speed would become a big issue. 



#### Defining the Data: 

###### This table gives the field names, SQL type, and sample data for each field in the dataset 

. | Medallion |  hack_license |  vendor_id |  rate_code |  store_and_fwd_flag |  pickup_datetime |  dropoff_datetime |  passenger_count |  trip_time_in_secs |  trip_distance |  pickup_longitude |  pickup_latitude |  dropoff_longitude |  dropoff_latitude
--- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | ---
SQL Type | Char(32) | Char(32) | Char(3) | Varchar(3) | Boolean | datetime |datetime | int | int | float | decimal(2,6) | decimal(2,6) | decimal(2,6) | decimal(2,6) 
Sample Data | 740BD5BE61840BE4FE3905CC3EBE3E7E | E48B185060FB0FF49BE6DA43E69E624B | CMT | 1 | N | 2013-10-01 12:44:29 | 2013-10-01 12:53:26 | 1 | 536 | 1.20 | -73.974319 | 40.741859 | -73.99115 | 40.742424
. | EA05309C30F375695F44C96108ACB10F | 1D10D8AC5B07D80861761365A05A9AE2 | CMT | 1 | N | 2013-10-02 19:44:55 | 2013-10-02 20:03:55 | 1 | 1139 | 5.10 | -73.981239 | 40.729141 | -73.943695 | 40.719715
. | EA05309C30F375695F44C96108ACB10F | 1D10D8AC5B07D80861761365A05A9AE2 | CMT | 1 | N | 2013-10-02 22:01:18 | 2013-10-02 22:09:18 | 1 | 480 | 2.20 | -74.002991 | 40.73354 | -74.005615 | 40.727261
.| B36D7AB5B422EA1A0588EFD1D8155EF3 | 669F420B42A0739A5D1058184AD227ED | CMT | 1 | N | 2013-10-03 12:14:35 | 2013-10-03 12:26:04 | 1 | 689 | .90 | -73.967178 | 40.766563 | -73.976784 | 40.755947
. | 28172009F5513B25F4091C0929C4515D | ABE08BCFEBF3F5F7339CA2170EEC1DEE | CMT | 1 | N | 2013-10-02 00:33:37 | 2013-10-02 01:06:16 | 1 | 1959 | 14.80 | -73.784142 | 40.648621 | -73.976479 | 40.672001



I obtained the field names and the sample data by printing the first few rows of data in the dataset using the following code within the for loop:

``` python
if n <= 5
	print(row)
```



From the field names and sample data I was able to determine most of the SQL types, though I used dictionaries to confirm: vendor_id is Char(3) type, store_and_fwd_flag is Boolean type, and rate_code is Varchar(3) - I had originally thought it was Char(1). The following dictionary formatting was used in each case (this example is for store_and_fwd_flag):

```python
if n > 0:
        k = row[4]
        if k in dictflag.keys():
            dictflag[k] += 1 
        else:
            dictflag[k] = 1 
    n +=1
print(dictflag) 
```

The following were the results of the dictionary, confirming that store_and_fwd_flag is Boolean type:

#image 



It was by this method I clarified the SQL types I was unsure about. 



#### **Field Descriptions** 

###### Includes the fields which have distinct values vs are unique (n/a)

**medallion:** Unique to each taxi – a cab must have a medallion to operate

​	Distinct values: n/a 

**hack_license:** Gives a person authority to drive a taxicab. 

​	Distinct values: n/a 

**vendor_id:** The company which employs the cab and driver of a given taxi. 

​	Distinct values: CMT, VTS 

**rate_code:** Method of determining payment to charge. 

​	Distinct values: 1, 2, 3, 5, 4, 0, 6, 210, 28, 7, 9, 8 

**store_and_fwd_flag:** This field is a Boolean type about whether or not a route had to be stored in the cab’s memory or not 

​	Distinct values: Yes, No, Blank 

**pickup_datetime:** Gives the time a customer is picked up.

​	Distinct values: n/a

**dropoff_datetime:** Gives the time a customer is dropped off. 

​	Distinct values: n/a

**passenger_count:** Gives the number of passengers per taxi ride. 

​	Distinct values: n/a

**trip_time_in_secs:** Gives the time in between pickup and drop-off in seconds. 

​	Distinct values: n/a

**trip_distance:** Gives the trip distance in miles. 

​	Distinct values: n/a



#### Maximum and minimum values of other numeric types:

Numeric Type | Min Value | Max Value 
--- | --- | --- 
Passenger Count | 1 | 9
Trip Time (seconds) | 121 | 9999
Trip Distance | 1.00 | 99.2

I was able to determine the maximum and minimum values for these numeric types using an iterative process of checking whether the value of each line is greater than the then-current max or less than the then-current min. I had to make sure, for all three cases, the min value was not zero (which would not be a logical result). An example (for passenger count) of how I did this is shown in the following piece of code: 

```python
if n > o:
	if n == 1:
		minpassengers = row[7]
        maxpassengers = row[7]
```

``` python
elif n > 1:
    if 0 < int(row[7]) < int(minpassengers):
        minpassengers = row[7]
    if row[7] > maxpassengers:
        maxpassengers = row[7]  
```





#### What is the geographic range of the data?

###### The data is filtered using the assumption that trips are constrained by the most northern, southern, eastern, and western points in the 48 states.  

Here is the method by which I determined the most Northern, Southern, Eastern, and Western points in the dataset, constraining them to the most Northern, Southern, Eastern, and Western points in the 48 states. For reference, 49.38407 deg is the northern most point in the 48 states, and so on. The reason for the duplication is that there is a pickup latitude and a drop-off latitude, and both need to be accounted for both eastern and western - likewise with longitude for northern and southern. 



``` python
n = 0 
north = None
south = None
east = None
west = None 

for row in reader:
    if n % 100000 == 0:
        print(n)
    if n > 0:
        if n == 1:
            north = row[11]
            south = row[11]
            east = row[10]
            west = row[10]
            
        elif n > 1:
            if '49.38407' > row[11] > north:
                north = row[11]
            if '49.38407' > row[13] > north:
                north = row[13]
                
            if '25.11567' < row[11] < south:
                south = row[11]
            if '25.11567' < row[13] < south:
                south = row[13]
               
            if '-66.94975' > row[10] > east:
                east = row[10]
            if '-66.94975' > row[12] > east:
                east = row[12]
                
            if '-124.73004' < row[10] < west:
                west = row[10]
            if '-124.73004' < row[12] < west:
                west = row[12] 
            
    else:
        pass
    n+=1
```



And here is the resulting geographic region:

#image

We note that this is a much larger geographic area than we would expect to see. Although we have constrained the coordinates to being inside of the US, this being a NYC taxi dataset we expect the geographic region of the rides to be much smaller and concentrated in NYC and the immediate surrounding areas. The large geographic range of this data suggests that there are a few outliers we need to filter out. The way I would do this would be to figure out the mean latitude and longitude and determine the distance away all the other location points were to that mean point. I would then filter out any locations which were more than three standard deviations away from the mean point. Here would be an example of the expected geographic range of the data:

#image



#### Average number of passengers each hour of the day.

The following code creates a dictionary which sums up the total number of passengers for each hour of the day. They keys in the dictionary are the hours in the day, from 00 to 23, and their corresponding values are the sum of the passenger count for each trip taken in that hour.

```python
n = 0 
passengersperhr = {}
for row in reader: 
    if n > 0:
        k = row[5][11:13]
        if k in passengersperhr.keys():
            passengersperhr[k] += int(row[7])
        else:
            passengersperhr[k] = int(row[7])
    n += 1
```

The following code takes, for each key (hour), the value (passenger count) and divides it by 31. 31 is the number of days in this dataset. 

``` python
for k in passengersperhr.keys():
    avg = passengersperhr[k] / 31
print(avg)
```

The result is the average number of passengers per hour per day of the dataset. The results were graphed in excel and are represented in the following table and graph: 

Hour | 00 |01 | 02 | 03 | 04 | 05 | 06 | 07 | 08 | 09 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 
--- |--- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- 
**Average Passengers** | 32389 | 23430 | 17764 | 12888 | 9270 | 7840| 15742| 29414 | 36132 | 37007 | 36332 | 37645 | 39813 | 39173 | 40506 | 39001 | 32295| 40486 | 49318 | 52903 | 50865 | 49676 | 47795 | 427188



#image 



The original purpose of this question was to use two dictionaries, both with hours as their keys, to find an average. When I originally did this it looked like this. 

```python
n = 0
passengersperhr = {}
tripsperhr = {}
for row in reader: 
    if n > 0:
        k = row[5][11:13]
        if k in passengersperhr.keys():
            passengersperhr[k] += int(row[7])
        else:
            passengersperhr[k] = int(row[7])
        
        if k in tripsperhr.keys():
            tripsperhr[k] += 1
        else:
            tripsperhr[k] = 1
        #in this case we are counting the rows (number of rides) in each hr 
      
    n += 1 

```

Here we have the original passengersperhr dictionary, but we also have a dictionary which counts the total number of taxi trips taken each hour - which is achieved by counting the number of rows occurring in each hour since one row equals one taxi trip. 

``` python
for k in passengersperhr.keys():
    avg = passengersperhr[k] / tripsperhr[k]
print(avg)
```

The average is the calculated as it was intended to using the code above, but the results are rather boring and uninformative. For each hour the result is around 1.5 to 1.7, because the average which is being calculated is the average number of passengers per taxi per hour. 

It is for this reason that I chose to average the passengers per hour by 31, the total number of days in the data set. My original intent was to build a dictionary for which the keys were the hours of the day and the values for the keys were the number of days for which the hour appeared in the data set. This would be more robust because it would eliminate the need to use an static number for the number of days for which the hour appeared in the data set (which may be different for different hours).



#### Creating a new CSV file which has only one out of every thousand rows of the main dataset. 

The following code first reads the original data file. It then creates a brand new data file (first opening, writing blank, and closing the data file so as not to append to the data file each time the code is ran). It then sets the parameters for writing the new file, that the line terminators will be a new line and the delimiter within the line will be a comma. It then cycles through the data set with a for loop and writes every thousandth line, along with the first line where i = 0 to the new data file. 

```python
f2 = open(fn, 'r')
reader = csv.reader(f)
f2 = open('every_thousand_rows.csv','a')
writer = csv.writer(f2, delimiter =',',lineterminator='\n') 

for i, row in enumerate(reader):
    if i > 0:
        if i % 1000 == 0:
            writer.writerow(row)
    else:
        writer.writerow(row)
f.close()
f2.close()
```

The resulting file is small enough to actually open on my device, which could be very helpful in analyzing data because you are able to open the file in Notepad++ and view the raw data. 

#image 



#### Again finding the average number of passengers each hour of the day but using the reduced data set. 

Because the task was to perform the exact same function with the smaller dataset as previously done on the large dataset, I simply changed the file the whole code was referencing from 'trip_data_10.csv' to 'every_thousand_rows.csv' as follows:

```python
import csv
import time

start = time.time()
fn = 'every_thousand_rows.csv'
f = open(fn,"r")
reader = csv.reader(f) 
```

The results of running the same code used to find the average passenger count per hour of the full dataset on the reduced dataset are represented in the following table and graph: 



| Hour                   | 00   | 01   | 02   | 03   | 04   | 05   | 06   | 07   | 08   | 09   | 10   | 11   | 12   | 13   | 14   | 15   | 16   | 17   | 18   | 19   | 20   | 21   | 22   | 23   |
| ---------------------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| **Average Passengers** | 32   | 26   | 18   | 15   | 11   | 9    | 15   | 31   | 36   | 35   | 36   | 38   | 43   | 37   | 41   | 39   | 31   | 39   | 50   | 49   | 54   | 49   | 45   | 42   |

![Graph](/images/avg_passengers_subset.JPG)

#image 



Comparing this graph with the graph of the average number of passengers for every hour per day of the large dataset, we can see that the subset gives a fairly accurate representation of the dataset as a whole. 





