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
### Change Index of Data Frame:
```Python
Specify row labels of cars
cars.index = ['US', 'AUS', 'JPN', 'IN', 'RU', 'MOR', 'EG']
```
<img width="470" alt="Screenshot 2019-10-01 at 11 05 52 PM" src="https://user-images.githubusercontent.com/47073386/65974774-1c2b2280-e4a0-11e9-8fff-5d2a921cc219.png">
