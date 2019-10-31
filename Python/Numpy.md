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
### 3. Random Number:
#### 3.1 random package: seed(), rand()
All the functionality is the random package, a sub-package of numpy. In this exercise, you'll be using two functions from this package:

* `seed()`: sets the random seed, so that your results are reproducible between simulations. As an argument, it takes an integer of your choosing. If you call the function, no output will be generated.
* `rand()`: if you don't specify any arguments, it generates a random float between zero and one.

```Python
# Import numpy as np
import numpy as np

# Set the seed
np.random.seed(123)

# Generate and print random float
print(np.random.rand())
```

#### 3.2. Generate a random iteger: randInt()
* randint(): also a function of the random package, to generate integers randomly. 
```Python
import numpy as np
np.random.randint(4, 8)
```
The above call generates the integer 4, 5, 6 or 7 randomly. 8 is not included.

* Example: Random Walk
```Python
import numpy as np
np.random.seed(123)

# Initialize random_walk
random_walk = [0]

for x in range(100) :
    # Set step: last element in random_walk
    step = random_walk[-1]

    # Roll the dice
    dice = np.random.randint(1,7)

    # Determine next step
    if dice <= 2:
        step = step - 1
    elif dice <= 5:
        step = step + 1
    else:
        step = step + np.random.randint(1,7)

    # append next_step to random_walk
    random_walk.append(step)

# Print random_walk
print(random_walk)

<script.py> output:
    [0, 3, 4, 5, 4, 5, 6, 7, 6, 5, 4, 3, 2, 1, 0, -1, 0, 5, 4, 3, 4, 3, 4, 5, 6, 7, 8, 7, 8, 7, 8, 9, 10, 11, 10, 14, 15, 14, 15, 14, 15, 16, 17, 18, 19, 20, 21, 24, 25, 26, 27, 32, 33, 37, 38, 37, 38, 39, 38, 39, 40, 42, 43, 44, 43, 42, 43, 44, 43, 42, 43, 44, 46, 45, 44, 45, 44, 45, 46, 47, 49, 48, 49, 50, 51, 52, 53, 52, 51, 52, 51, 52, 53, 52, 55, 56, 57, 58, 57, 58, 59]
    
# Import matplotlib.pyplot as plt
import matplotlib.pyplot as plt
# Plot random_walk
plt.plot(random_walk)
# Show the plot
plt.show()
<img width="313" alt="Screenshot 2019-10-06 at 11 03 46 AM" src="https://user-images.githubusercontent.com/47073386/66263654-082d4b00-e829-11e9-8e6e-5b9598ca9b45.png">

```
### 2D Array:

```Python

In [1]: import numpy as np
In [2]: a = np.array([[2,0],[3,0],[3,1],[5,0],[5,1],[5,2]])
In [3]: b = np.zeros((6,3), dtype='int32')
#
In [4]: b[a[:,0], a[:,1]] = 10

In [5]: b
Out[5]: 
array([[ 0,  0,  0],
       [ 0,  0,  0],
       [10,  0,  0],
       [10, 10,  0],
       [ 0,  0,  0],
       [10, 10, 10]])
```

Explanation:
b[x, y] = z
where x being the first column of a and y being the second column of a. Thus, choose x = a[:, 0], and y = a[:, 1].
