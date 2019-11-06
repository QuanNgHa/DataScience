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

#### Creating SQL from natural language
Now you'll write a respond() function that can handle messages like "I want an expensive hotel in the south of town" and respond appropriately according to the number of matching results in a database. This is an important functionality for any database-backed chatbot.

Your find_hotels() function from the previous exercises has already been defined for you, along with a Rasa NLU interpreter object, which can handle hotel queries, and a list of responses

```Python
# Define respond()
def respond(message):
    # Extract the entities
    entities = interpreter.parse(message)["entities"]
    # Initialize an empty params dictionary
    params = {}
    # Fill the dictionary with entities
    for ent in entities:
        params[ent["entity"]] = str(ent["value"])

    # Find hotels that match the dictionary
    results = find_hotels(params)
    # Get the names of the hotels and index of the response
    names = [r[0] for r in results]
    n = min(len(results),3)
    # Select the nth element of the responses array
    return responses[n].format(*names)
```
```Python
# Test the respond() function
print(respond("I want an expensive hotel in the south of town"))
Output: Grand Hotel is a great hotel!

print(entities)
[{'end': 19, 'start': 10, 'processors': ['ner_synonyms'], 'extractor': 'ner_crf', 'entity': 'price', 'value': 'hi'},      
 {'entity': 'area', 'end': 38, 'start': 33, 'value': 'south', 'extractor': 'ner_crf'}]
print(params)
{'area': 'south', 'price': 'hi'}
print(results)
[('Grand Hotel', 'hi', 'south', 5)]
print(names)
['Grand Hotel']
```
