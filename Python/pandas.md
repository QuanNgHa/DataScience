# 1. How to create a data frame:
## 1.1 Convert Dictionary to Data Frame

```Python
# Pre-defined lists
names = ['United States', 'Australia', 'Japan', 'India', 'Russia', 'Morocco', 'Egypt']
dr =  [True, False, False, False, True, True, True]
cpc = [809, 731, 588, 18, 200, 70, 45]

# Import pandas as pd
import pandas as pd

# Create dictionary my_dict with three key:value pairs: my_dict
my_dict = {'country': names, 'drives_right': dr, 'cars_per_cap': cpc}

# Build a DataFrame cars from my_dict: cars
cars = pd.DataFrame(my_dict)
```
<img width="445" alt="Screenshot 2019-09-30 at 11 17 26 PM" src="https://user-images.githubusercontent.com/47073386/65895276-caba5f00-e3dd-11e9-9595-374d9ee9ffdf.png">

## 1.2 Read CSV file:
```Python
cars = pd.read_csv('cars.csv', index_col=0)
```
### Parse date
The `read_csv` method doesn't automatically detect which columns contain dates. You can tell read_csv this by giving a list of the date columns as the optional argument `parse_dates`
```Python
my_df = pd.read_csv("datasets/my_data.csv", parse_dates=["date"])
```
### Change Index of Data Frame:
```Python
Specify row labels of cars
cars.index = ['US', 'AUS', 'JPN', 'IN', 'RU', 'MOR', 'EG']
```
<img width="470" alt="Screenshot 2019-10-01 at 11 05 52 PM" src="https://user-images.githubusercontent.com/47073386/65974774-1c2b2280-e4a0-11e9-8fff-5d2a921cc219.png">

# 2. Select Data from Data Frame:

### 2.1. Square Brackets 
#### Select Column
```Python
cars['cars_per_cap', 'country']
cars[['cars_per_cap', 'country']]
```
The single bracket version gives a `Pandas Series`, the double bracket version gives a `Pandas DataFrame`.

### Select Row
For ex: selects the first five rows from the cars
```Python
cars[0:5]
```

### loc & iloc
`loc` is label-based, which means that you have to specify rows and columns based on their row and column labels. 
`iloc` is integer index based, so you have to specify rows and columns by their integer index like you did in the previous exercise.
```Python
# Select rows using loc & iloc
cars.loc['RU'] ---> Return as Pandas Series
cars.iloc[4]

cars.loc[['RU']] ---> Return as Pandas Data Frame
cars.iloc[[4]]
```

```Python
# Select both rows & column using loc & iloc
cars.loc['IN', 'cars_per_cap'] ---> Return as Pandas Series
cars.iloc[3, 0]

cars.loc[['IN', 'RU'], 'cars_per_cap']---> Return as Pandas Data Frame
cars.iloc[[3, 4], 0]

cars.loc[['IN', 'RU'], ['cars_per_cap', 'country']]
cars.iloc[[3, 4], [0, 1]]
```
It's also possible to select only columns with loc and iloc. In both cases, you simply put a slice going from beginning to end in front of the comma
```Python
# Select on columns with loc & iloc
cars.loc[:, 'country']---> Return as Pandas Series
cars.iloc[:, 1]

cars.loc[:, ['country']]---> Return as Pandas Data Frame
cars.loc[:, ['country','drives_right']]
cars.iloc[:, [1, 2]]
```
# 3. Filtering Pandas DataFrame

```Python
# Import cars data
import pandas as pd
cars = pd.read_csv('cars.csv', index_col = 0)
```
**Original Method:**
```Python
# Step 1: Extract drives_right column as Series with True value: dr
#dr = cars['drives_right'] == True
dr = cars['drives_right']

# Step 2: Use dr to subset cars: sel
sel = cars[dr]
```
**Shortcut:**
```Python
sel = cars[cars['drives_right']]
```
```
# Print sel
print(sel)
<script.py> output:
         cars_per_cap        country  drives_right
    US            809  United States          True
    RU            200         Russia          True
    MOR            70        Morocco          True
    EG             45          Egypt          True

```
More examples:

```Python
# Create medium: observations with cars_per_cap between 100 and 500
medium = cars[np.logical_and(cars["cars_per_cap"]>100, cars["cars_per_cap"]<500)]
```
# 4. Drop:
## 4.1. Drop Columm:
* axis : {0 or ‘index’~ rows, 1 or ‘columns’}, default 0
```Python
iris.drop(["Species"], axis=1) 
```
