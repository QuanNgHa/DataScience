### I. Asking questions
Responding to statements with a question and responding to questions with answers.

```Python
import random
def respond(message):
    # Check for a question mark
    if message.endswith('?'):
        # Return a random question
        return random.choice(responses["question"])
    # Return a random statement
    return random.choice(responses["statement"])

```
### II.  Extracting key phrases: Regular expressions
#### re.search()

A dictionary called `rules` has already been defined, which matches the following patterns:

* "do you think (.*)"
* "do you remember (.*)"
* "I want (.*)"
* "if (.*)"

```Python
# Rules dict:
{'I want (.*)': ['What would it mean if you got {0}', 'Why do you want {0}', "What's stopping you from getting {0}"], 
'do you think (.*)': ['if {0}? Absolutely.', 'No chance'], 
'do you remember (.*)': ['Did you think I would forget {0}', "Why haven't you been able to forget {0}", 'What about {0}', 'Yes .. and?'], 
'if (.*)': ["Do you really think it's likely that {0}", 'Do you wish that {0}', 'What do you think about {0}', 'Really--if {0}']}
```

### match_rule()

```Python
# Define match_rule()
def match_rule(rules, message):
    response, phrase = "default", None
    
    # Iterate over the rules dictionary
    for pattern, responses in rules.items():
        # Create a match object
        match = re.search(pattern, message)
        if match is not None:
            # Choose a random response
            response = random.choice(responses)
            if '{0}' in response:
                phrase = match.group(1)
    # Return the response and phrase
    return response.format(phrase)

# Test match_rule
print(match_rule(rules, "do you remember your last birthday"))

<script.py> output:
    Why haven't you been able to forget your last birthday

```
#### re.sub(): replace_pronouns

```Python
 re.sub('you','me', message)
 ```
 ### replace_pronouns()
 ```Python
 # Define replace_pronouns()
def replace_pronouns(message):

    message = message.lower()
    if 'me' in message:
        # Replace 'me' with 'you'
        return re.sub('me','you', message)
    if 'my' in message:
        # Replace 'my' with 'your'
        return re.sub('my','your', message)
    if 'your' in message:
        # Replace 'your' with 'my'
        return re.sub('your','my', message)
    if 'you' in message:
        # Replace 'you' with 'me'
        return re.sub('you','me', message)
 ```

### Put Everything Together:
```Python
# Define respond()
def respond(message):
    # Call match_rule
    response, phrase = match_rule(rules, message)
    if '{0}' in response:
        # Replace the pronouns in the phrase
        phrase = replace_pronouns(phrase)
        # Include the phrase in the response
        response = response.format(phrase)
    return response

# Send the messages
send_message("do you remember your last birthday")
send_message("do you think humans should be worried about AI")

<script.py> output:
    USER : do you remember your last birthday
    BOT : What about my last birthday
    USER : do you think humans should be worried about AI
    BOT : No chance
```
### III. Intents and Entities

#### Intent classification with regex
```
The keywords dictionary:
{'goodbye': ['bye', 'farewell'], 'thankyou': ['thank', 'thx'], 'greet': ['hello', 'hi', 'hey']}
------------------------------------------------------------
# Define a dictionary of patterns
patterns = {}

# Iterate over the keywords dictionary
for intent, keys in keywords.items():
    # Create regular expressions and compile them into pattern objects
    patterns[intent] = re.compile('|'.join(keys))
    
# Print the patterns
print(patterns)

<script.py> output:
    {'goodbye': re.compile('bye|farewell'), 'greet': re.compile('hello|hi|hey'), 'thankyou': re.compile('thank|thx')}
```
### Define a function to find the intent of a message:
```Python
# Define a function to find the intent of a message
def match_intent(message):
    matched_intent = None
    for intent, pattern in patterns.items():
        # Check if the pattern occurs in the message 
        if pattern.search(message):
            matched_intent = intent
    return matched_intent

# Define a respond function
def respond(message):
    # Call the match_intent function
    intent = match_intent(message)
    # Fall back to the default response
    key = "default"
    if intent in responses:
        key = intent
    return responses[key]

# Send messages
send_message("hello!")
send_message("bye byeee")
send_message("thanks very much!")

<script.py> output:
    USER : hello!
    BOT : Hello you! :)
    USER : bye byeee
    BOT : goodbye for now
    USER : thanks very much!
    BOT : you are very welcome
```

### Entity extraction with regex
Now you'll use another simple method, this time for finding a person's name in a sentence, such as "hello, my name is David Copperfield".

You'll look for the keywords "name" or "call(ed)", and find capitalized words using regex and assume those are names. Your job in this exercise is to define a find_name() function to do this.

```Python
# Define find_name()
def find_name(message):
    name = None
    # Create a pattern for checking if the keywords occur
    name_keyword = re.compile("name|call")
    # Create a pattern for finding capitalized words - Only First Letter {1} is captialized 
    name_pattern = re.compile("[A-Z]{1}[a-z]*")
    if name_keyword.search(message):
        # Get the matching words in the string
        name_words = name_pattern.findall(message)
        if len(name_words) > 0:
            # Return the name if the keywords are present
            name = ' '.join(name_words)
    return name

# Define respond()
def respond(message):
    # Find the name
    name = find_name(message)
    if name is None:
        return "Hi there!"
    else:
        return "Hello, {0}!".format(name)

# Send messages
send_message("my name is David Copperfield")
send_message("call me Ishmael")
send_message("People call me Cassandra")

<script.py> output:
    USER : my name is David Copperfield
    BOT : Hello, David Copperfield!
    USER : call me Ishmael
    BOT : Hello, Ishmael!
    USER : People call me Cassandra
    BOT : Hello, People Cassandra!
```
### IV. Word vectors with spaCy
Task: create a 2D array X with as many rows as there are sentences in the dataset, where each row is a vector describing that sentence.
```Python
import spacy
# Load the spacy model: nlp
nlp = spacy.load('en')

# Calculate the length of sentences - sentences is a list of sentence
n_sentences = len(sentences)

# Calculate the dimensionality of nlp
embedding_dim = nlp.vocab.vectors_length
# Initialize the 2D-array with zeros: X - # of row = n_sentences; # of column = embedding_dim
X = np.zeros((n_sentences, embedding_dim))

# Iterate over the sentences
for idx, sentence in enumerate(sentences):
    # Pass each each sentence to the nlp object to create a document
    doc = nlp(sentence)
    # Save the document's .vector attribute to the corresponding row in X
    # X[idx, :] == idx th row of 2D numpy array X; if want to choose idx th column of X ->  X[:, idx]
    X[idx, :] = doc.vector
```
### V. Intent classification with sklearn:
An array `X` containing vectors describing each of the sentences in the ATIS dataset has been created for you, along with a 1D array y containing the labels. The labels are integers corresponding to the intents in the dataset. For example, label 0 corresponds to the intent atis_flight.

```Python
# Import SVC
from sklearn.svm import SVC

# Create a support vector classifier
clf = SVC(C=1)
# Fit the classifier using the training data
clf.fit(X_train, y_train)

# Predict the labels of the test set
y_pred = clf.predict(X_test)

# Count the number of correct predictions
n_correct = 0
for i in range(len(y_test)):
    if y_pred[i] == y_test[i]:
        n_correct += 1

print("Predicted {0} correctly out of {1} test examples".format(n_correct, len(y_test)))


Predicted 162 correctly out of 201 test examples
```
### VI. Entity extraction: Using spaCy's entity recognizer

In this exercise, you'll use spaCy's built-in entity recognizer to extract names, dates, and organizations from search queries.
```Python
import spacy

nlp = spacy.load('en')
# Define included_entities
include_entities = ['DATE', 'ORG', 'PERSON']

# Define extract_entities()
def extract_entities(message):
    # Create a dict to hold the entities
    ents = dict.fromkeys(include_entities)
    
    # Create a spacy document
    doc = nlp(message)
    print(doc.ents)
    for ent in doc.ents:
        
        if ent.label_ in ents:
            # Save interesting entities
            ents[ent.label_] = ent.text
    return ents

print(extract_entities('friends called Mary who have worked at Google since 2010'))
print(extract_entities('people who graduated from MIT in 1999')

Output:
{'ORG': Google, 'DATE': '2010', 'PERSON': Mary}
()
{'ORG': MIT, 'DATE': 1999, 'PERSON': None}
```
### Assigning roles using spaCy's parser

In this exercise you'll use spaCy's powerful syntax parser to assign roles to the entities in your users' messages. 

* Functions used from Spacy doc: .ancestors; entity_type(); 
```Python
# Iterate over parents in parse tree until an item entity is found
def find_parent_item(word):
    # Iterate over the word's ancestors
    for parent in word.ancestors:
        # Check for an "item" entity
        if entity_type(parent) == "item":
            return parent.text
    return None

# For all color entities, find their parent item
def assign_colors(doc):
    # Iterate over the document
    for word in doc:
        # Check for "color" entities
        if entity_type(word) == "color":
            # Find the parent
            item =  find_parent_item(word)
            print("item: {0} has color : {1}".format(item, word))

# Assign the colors
# Create the Spacy document
doc = nlp("let's see that jacket in red and some blue jeans")

assign_colors(doc)

<script.py> output:
    item: jacket has color : red
    item: jeans has color : blue

```
