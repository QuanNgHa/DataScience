### 1. SQL statements in Python

```Python
# Import sqlite3
import sqlite3

# Open connection to DB
conn = sqlite3.connect('hotels.db')

# Create a cursor
c = conn.cursor()

# Define area and price
area, price = "south", "hi"
t = (area, price)

# Execute the query
c.execute('SELECT * FROM hotels WHERE area=? AND price=?', t)

# Print the results using .fetchall() method
print(c.fetchall())

<script.py> output:
    [('Grand Hotel', 'hi', 'south', 5)]

```
### 2. Exploring a DB with natural language
#### Creating queries from parameters

```Python
# Define find_hotels()
def find_hotels(params):
    # Create the base query
    query = 'SELECT * FROM hotels'
    # Add filter clauses for each of the parameters
    if len(params) > 0:
        filters = ["{}=?".format(k) for k in params]
        query += " WHERE " + " and ".join(filters)
    # Create the tuple of values
    t = tuple(params.values())
    
    # Open connection to DB
    conn = sqlite3.connect("hotels.db")
    # Create a cursor
    c = conn.cursor()
    # Execute the query
    c.execute(query, t)
    # Return the results
    return c.fetchall()

find_hotels({"area": "south", "price":"lo"}). 
Output: [('Cozy Cottage', 'lo', 'south', 2)]
```
