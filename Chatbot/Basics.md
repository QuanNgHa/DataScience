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
