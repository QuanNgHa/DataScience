### 1. Convert Python List to Numpy Array:


```Python
# Import numpy as np
import numpy as np

# Store pop as a numpy array: np_pop <= Conver pop List to Numpy Array
np_pop = np.array(pop)

# Double np_pop: Double the values in np_pop by assigning np_pop * 2 to np_pop again. 
# Because np_pop is a Numpy array, each array element will be doubled.
np_pop = np_pop*2
```

### 2. Comparison between Numpy Array:
#### 2.1. Operational operators like < and >=

```Python
my_house = np.array([18.0, 20.0, 10.75, 9.50])
your_house = np.array([14.0, 24.0, 14.25, 9.0])

# my_house greater than or equal to 18
print(my_house>=18)

# my_house less than your_house
print(my_house < your_house)

<script.py> output:
    [ True  True False False]
    [False  True  True False]
```

#### 2.2. Boolean operators like and, or, not

To use these operators with Numpy, you will need `np.logical_and()`, `np.logical_or()` and `np.logical_not()`

```Python
# my_house greater than 18.5 or smaller than 10
print(np.logical_or(my_house>18.5, my_house <10))

# Both my_house and your_house smaller than 11
print(np.logical_and(my_house<11, your_house<11))
```
