### 1. Exploring your working directory
IPython, which is running on DataCamp's servers, has a bunch of cool commands, including its magic commands. For example, starting a line with ! gives you complete system shell access. This means that the IPython magic command ! ls will display the contents of your current directory. 
Magic commands: https://ipython.readthedocs.io/en/stable/overview.html

```Python
In [2]: ! ls
moby_dick.txt
```
### 2. Importing entire text files

```Python
# Open a file: file
file = open('moby_dick.txt', mode ='r')

# Print it
print(file.read())

# Check whether file is closed
print(file.closed)

# Close file
file.close()


<script.py> output:
    CHAPTER 1. Loomings.
    
    Call me Ishmael. Some years ago--never mind how long precisely--having
    little or no money in my purse, and nothing particular to interest me on
    shore, I thought I would sail about a little and see the watery part of
    the world. It is a way I have of driving off the spleen and regulating
    the circulation. Whenever I find myself growing grim about the mouth;
    whenever it is a damp, drizzly November in my soul; whenever I find

```
#### Importing text files line by line
* `file.readline()`
When a file called file is open, you can print out the first line by executing file.readline(). If you execute the same command again, the second line will print, and so on
* Context manager: --> No need to close the file
```Python
with open('huck_finn.txt') as file:
```

### 3. Flat files
#### Using NumPy to import flat files
`loadtxt()` and see just how easy it can be:
* The first argument will be the filename.
* `delimiter`: ',' for comma-delimited; '\t' for tab-delimited.
* `skiprows` allows you to specify how many rows (not indices) you wish to skip; for ex; skiprows = 1 ==> to skip first row
* `usecols` takes a list of the indices of the columns you wish to keep.

```Python
# Import package
import numpy as np

# Assign filename to variable: file
file = 'digits.csv'

# Load file as array: digits
digits = np.loadtxt(file, delimiter=',')

# Print datatype of digits
print(type(digits))

# Select and reshape a row
im = digits[21, 1:]
im_sq = np.reshape(im, (28, 28))

# Plot reshaped data (matplotlib.pyplot already loaded as plt)
plt.imshow(im_sq, cmap='Greys', interpolation='nearest')
plt.show()
<script.py> output:
    <class 'numpy.ndarray'>

```
<img width="420" alt="Screenshot 2019-11-17 at 2 49 30 PM" src="https://user-images.githubusercontent.com/47073386/69004161-8103e480-0949-11ea-8571-ab7c2de37bcd.png">

```Python
# Assign the filename: file
file = 'digits_header.txt'

# Load the data: data
data = np.loadtxt(file, delimiter='\t', skiprows=1, usecols=[0,2])
<script.py> output:
    [[1. 0.]
     [1. 0.]
     [2. 0.]
     [0. 0.]
     [5. 0.]]
```
#### Importing different datatypes
The file seaslug.txt: has a text header, consisting of strings; is tab-delimited.

Due to the header, if you tried to import it as-is using np.loadtxt(), Python would throw you a ValueError and tell you that it could not convert string to float. There are two ways to deal with this: firstly, you can set the data type argument dtype equal to str (for string).

```Python
# Assign filename: file
file = 'seaslug.txt'

# Import file: data
data = np.loadtxt(file, delimiter='\t', dtype=str)

# Print the first element of data
print(data[0])

# Import data as floats and skip the first row: data_float
data_float = np.loadtxt(file, delimiter='\t', dtype=float, skiprows=1)

# Print the 10th element of data_float
print(data_float[9,:])

# Plot a scatterplot of the data
plt.scatter(data_float[:, 0], data_float[:, 1])
plt.xlabel('time (min.)')
plt.ylabel('percentage of larvae')
plt.show()

<script.py> output:
    ['Time' 'Percent']
    [0.    0.357]
```
<img width="420" alt="Screenshot 2019-11-17 at 3 03 40 PM" src="https://user-images.githubusercontent.com/47073386/69004271-76e2e580-094b-11ea-90b7-21c564e678da.png">

#### Working with mixed datatypes 
mport datasets which have different datatypes in different columns; one column may contain strings and another floats, for example. 
The function np.loadtxt() will freak at this. 
`np.genfromtxt()`, which can handle such structures. If we pass dtype=None to it, it will figure out what types each column should be.

Import 'titanic.csv' using the function np.genfromtxt() as follows:

`data = np.genfromtxt('titanic.csv', delimiter=',', names=True, dtype=None)`
* First argument is the filename
* Second specifies the delimiter 
* Third argument names tells us there is a header. 

Because the data are of different types, data is an object called a structured array. 
Because numpy arrays have to contain elements that are all the same type, the structured array solves this by being a 1D array, where each element of the array is a row of the flat file imported. 
You can test this by checking out the array's shape in the shell by executing np.shape(data).

Accessing rows and columns of structured arrays 
* to get the ith row, execute data[i] 
* to get the column with name 'Fare', execute data['Fare'].

```Python
In [1]: data = np.genfromtxt('titanic.csv', delimiter=',', names=True, dtype=None)

In [2]: np.shape(data)
Out[2]: (891,) #a 1D array with length of 891, where each element of the array is a row of the flat file imported

In [3]: data['Survived'] #access how colummn
Out[3]: array([0, 1, 1, 1, 0, 0, 0, 0, 1, 1, 1, 1, 0, 0, 0, 1, 0, 1, 0, 1, 0, 1,
       1, 1, 0,
In [6]: data[0]
Out[6]: (1, 0, 3, b'male', 22., 1, 0, b'A/5 21171', 7.25, b'', b'S')
```
