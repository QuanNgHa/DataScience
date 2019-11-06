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
#### Refining your search
Now you'll write a bot that allows users to add filters incrementally, just in case they don't specify all of their preferences in one message.

To do this, initialize an empty dictionary params outside of your respond() function (as opposed to inside the function, like in the previous exercise). Your respond() function will take in this dictionary as an argument.

```Python
# Define a respond function, taking the message and existing params as input
def respond(message,params):
    # Extract the entities
    entities = interpreter.parse(message)["entities"]
    # Fill the dictionary with entities
    for ent in entities:
        params[ent["entity"]] = str(ent["value"])

    # Find the hotels
    results = find_hotels(params)
    names = [r[0] for r in results]
    n = min(len(results), 3)
    # Return the appropriate response
    return responses[n].format(*names), params

# Initialize params dictionary
params = {}

# Pass the messages to the bot
for message in ["I want an expensive hotel", "in the north of town"]:
    print("USER: {}".format(message))
    response, params = respond(message, params)
    print("BOT: {}".format(response))

<script.py> output:
    USER: I want an expensive hotel
    BOT: Grand Hotel is one option, but I know others too :)
    USER: in the north of town
    BOT: Ben's BnB is a great hotel!
```
### 3. Basic negation
Quite often, you'll find your users telling you what they don't want - and that's important to understand! In general, negation is a difficult problem in NLP. Here, we'll take a very simple approach that works for many cases.

A list of tests called tests has been defined for you. Explore it in the Shell - you'll find that each test is a tuple consisting of:
* A string containing a message with entities.
* A dictionary containing the entities as keys and a Boolean saying whether they are negated as the key.

Your job is to define a function called negated_ents() which looks for negated entities in a message.



```
# Define negated_ents()
def negated_ents(phrase):
    # Extract the entities using keyword matching
    # check if the words "south" or "north" appear in the message and extract those entities.
    ents = [e for e in ["south", "north"] if e in phrase]
    
    #Split the sentence into chunks ending with each entity. To do this
    
    #Step1: Use the .index() method of phrase to find the starting index of each entity e and add the entity's length to it to find the index of the end of the entity.
    # Find the index of the final character of each entity
    ends = sorted([phrase.index(e) + len(e) for e in ents])
    
    # Initialise a list to store sentence chunks
    chunks = []
    # Take slices of the sentence up to and including each entitiy
    #Starting with start=0, take slices of the string from start to end for each end in ends. 
    #Append each slice of the sentence to the list, chunks. Ensure you update your starting position with each iteration.
    start = 0
    for end in ends:
        chunks.append(phrase[start:end])
        start = end
    result = {}
    # Iterate over the chunks and look for entities
    for chunk in chunks:
        for ent in ents:
            if ent in chunk:
                # If the entity contains a negation, assign the key to be False
                if "not" in chunk or "n't" in chunk:
                    result[ent] = False
                else:
                    result[ent] = True
    return result  
```

Check that the entities are correctly assigned as True or False
tests = [("no I don't want to be in the south", {'south': False}), 
         ('no it should be in the south', {'south': True}), 
         ('no in the south not the north', {'south': True, 'north': False}), 
         ('not north', {'north': False})]
for test in tests:
    print(negated_ents(test[0]) == test[1])

