### 1. Stateful Bot

In this exercise, you'll begin building a bot that lets users order coffee. They can choose between two types: Colombian and Kenyan. If the user provides unexpected input, your bot will handle this differently depending on where they are in the flow.

Your job here is to identify the appropriate state and next state based on the intents and response messages provided. For example, if the intent is "order", then the state changes from INIT to CHOOSE_COFFEE.

A function send_message(policy, state, message) has already been defined for you. It takes the policy, the current state, and message as arguments, and returns the new state as a result. Additionally, an interpret(message) function, similar to the one Alan described in the video, has been pre-defined for you.


```Python
# Define the INIT state
INIT = 0

# Define the CHOOSE_COFFEE state
CHOOSE_COFFEE =1

# Define the ORDERED state
ORDERED = 2

# Define the policy rules
# Create a dictionary called policy with tuples as keys and values. 
# Each key is a tuple containing a state and an intent, and 
# Each value is a tuple containing the next state and the response message.
policy = {
    (INIT, "order"): (CHOOSE_COFFEE, "ok, Colombian or Kenyan?"),
    (INIT, "none"): (INIT, "I'm sorry - I'm not sure how to help you"),
    (CHOOSE_COFFEE, "specify_coffee"): (ORDERED, "perfect, the beans are on their way!"),
    (CHOOSE_COFFEE, "none"): (CHOOSE_COFFEE, "I'm sorry - would you like Colombian or Kenyan?"),
}

# Create the list of messages
messages = [
    "I'd like to become a professional dancer",
    "well then I'd like to order some coffee",
    "my favourite animal is a zebra",
    "kenyan"
]

# Call send_message() for each message
state = INIT
for message in messages:    
    state = send_message(policy, state, message)

<script.py> output:
    USER : I'd like to become a professional dancer
    BOT : I'm sorry - I'm not sure how to help you
    USER : well then I'd like to order some coffee
    BOT : ok, Colombian or Kenyan?
    USER : my favourite animal is a zebra
    BOT : I'm sorry - would you like Colombian or Kenyan?
    USER : kenyan
    BOT : perfect, the beans are on their way!
```
#### Asking contextual questions

Sometimes your users need some help! They will have questions and expect the bot to help them.

In this exercise, you'll allow users to ask the coffee bot to explain the steps to them. As in the previous exercise, the answer they get will depend on where they are in the flow.

```Python
# Define the states
INIT=0 
CHOOSE_COFFEE=1
ORDERED=2

# Define the policy rules dictionary
# Add two rules to your policy_rules to handle the intent "ask_explanation" when in the states INIT or CHOOSE_COFFEE.
policy_rules = {
    (INIT, "ask_explanation"): (INIT, "I'm a bot to help you order coffee beans"),
    (INIT, "order"): (CHOOSE_COFFEE, "ok, Colombian or Kenyan?"),
    (CHOOSE_COFFEE, "specify_coffee"): (ORDERED, "perfect, the beans are on their way!"),
    (CHOOSE_COFFEE, "ask_explanation"): (CHOOSE_COFFEE, "We have two kinds of coffee beans - the Kenyan ones make a slightly sweeter coffee, and cost $6. The Brazilian beans make a nutty coffee and cost $5.")    
}

# Define send_messages()
def send_messages(messages):
    state = INIT
    for msg in messages:
        state = send_message(state, msg)

# Send the messages
send_messages([
    "what can you do for me?",
    "well then I'd like to order some coffee",
    "what do you mean by that?",
    "kenyan"
])
<script.py> output:
    USER : what can you do for me?
    BOT : I'm a bot to help you order coffee beans
    USER : well then I'd like to order some coffee
    BOT : ok, Colombian or Kenyan?
    USER : what do you mean by that?
    BOT : We have two kinds of coffee beans - the Kenyan ones make a slightly sweeter coffee, and cost $6. The Brazilian beans make a nutty coffee and cost $5.
    USER : kenyan
    BOT : perfect, the beans are on their way!
```

#### Dealing with rejection
What happens if you make a suggestion to your user and they don't like it? Your bot will look really silly if it makes the same suggestion again right away.

Here, you're going to modify your respond() function so that it accepts and returns 4 arguments:

* The user message as an argument, and the bot response as the first return value.
* A dictionary params including the entities the user has specified.
* A prev_suggestions list. When passed to respond(), this should contain the suggestions made in the previous bot message. When returned by respond(), it should contain the current suggestions.
* An excluded list, which contains all of the results your user has already explicitly rejected.
Your function should add the previous suggestions to the excluded list whenever it receives a "deny" intent. It should also filter out excluded suggestions from the response.

```Python
# Define respond()
def respond(message, params, prev_suggestions, excluded):
    # Interpret the message
    parse_data = interpret(message)
    print(parse_data)
    # Extract the intent
    intent = parse_data["intent"]["name"]
    # Extract the entities
    entities = parse_data["entities"]
    # Add the suggestion to the excluded list if intent is "deny"
    if intent == "deny":
        excluded.extend(prev_suggestions)
    # Fill the dictionary with entities	
    for ent in entities:
        params[ent["entity"]] = str(ent["value"])
    # Find matching hotels
    results = [
        r 
        for r in find_hotels(params, excluded) 
        if r[0] not in excluded
    ]
    # Extract the suggestions
    names = [r[0] for r in results]
    n = min(len(results), 3)
    suggestions = names[:2]
    return responses[n].format(*names), params, suggestions, excluded

# Initialize the empty dictionary and lists
params, suggestions, excluded = {}, [], []

# Send the messages
for message in ["I want a mid range hotel", "no that doesn't work for me"]:
    print("USER: {}".format(message))
    response, params, suggestions, excluded = respond(message, params, suggestions, excluded)
    print("BOT: {}".format(response))

---Output--
USER: I want a mid range hotel
{'intent': {'confidence': 0.0, 'name': ''}, 'entities': [], 'text': 'I want a mid range hotel'}
BOT: Hotel for Dogs is one option, but I know others too :)
USER: no that doesn't work for me
{'intent': {'confidence': 0.0, 'name': 'deny'}, 'entities': [], 'text': "no that doesn't work for me"}
BOT: Grand Hotel is one option, but I know others too :)
```
### 2. Asking questions & queuing answers
#### Pending actions
You can really improve the user experience of your bot by asking the user simple yes or no follow-up questions. One easy way to handle these follow-ups is to define pending actions which get executed as soon as the user says "yes", and wiped if the user says "no".

In this exercise, you're going to define a policy() function which takes the intent as its sole argument and returns two values: The next action to take and a pending action. The policy function should return this pending action when a "yes" or "affirm" intent is returned and should wipe the pending actions if a "no" or "deny" intent is returned.

Here, the interpret(message) function has been defined for you such that if "yes" is in the message, "affirm" is returned, and if "no" is in the message, then "deny" is returned.

```Python
# Define policy()
def policy(intent):
    # Return "do_pending" if the intent is "affirm"
    if intent == "affirm":
        return "do_pending", None
    # Return "Ok" if the intent is "deny"
    if intent == "deny":
        return "Ok", None
    if intent == "order":
        return "Unfortunately, the Kenyan coffee is currently out of stock, would you like to order the Brazilian beans?", "Alright, I've ordered that for you!"
        
# Define send_message()
def send_message(pending, message):
    print("USER : {}".format(message))
    action, pending_action = policy(interpret(message))
    if action == "do_pending" and pending is not None:
        print("BOT : {}".format(pending))
    else:
        print("BOT : {}".format(action))
    return pending_action
    
# Define send_messages()
def send_messages(messages):
    pending = None
    for msg in messages:
        pending = send_message(pending, msg)

# Send the messages
send_messages([
    "I'd like to order some coffee",
    "ok yes please"
])
<script.py> output:
    USER : I'd like to order some coffee
    BOT : Unfortunately, the Kenyan coffee is currently out of stock, would you like to order the Brazilian beans?
    USER : ok yes please
    BOT : Alright, I've ordered that for you!
```

#### Pending state transitions
You'll often need to briefly deviate from the flow of a conversation, for example to authenticate a user, before returning to the topic of discussion.

In these cases, it's often simpler - and easier to debug - if you save some actions/states as pending rather than adding ever more complicated rules.

Here, you're going to define a policy_rules dictionary, where the keys are tuples of the current state and the received intent, and the values are tuples of the next state, the bot's response, and a state for which to set a pending transition.

```Python
# Define the states
INIT=0
AUTHED=1
CHOOSE_COFFEE=2
ORDERED=3

# Define the policy rules
policy_rules = {
    (INIT, "order"): (INIT, "you'll have to log in first, what's your phone number?", AUTHED),
    (INIT, "number"): (AUTHED, "perfect, welcome back!", None),
    (AUTHED, "order"): (CHOOSE_COFFEE, "would you like Colombian or Kenyan?", None),    
    (CHOOSE_COFFEE, "specify_coffee"): (ORDERED, "perfect, the beans are on their way!", None)
}

# Define send_messages()
def send_messages(messages):
    state = INIT
    pending = None
    for msg in messages:
        state, pending = send_message(state, pending, msg)

# Send the messages
send_messages([
    "I'd like to order some coffee",
    "555-1234",
    "kenyan"
])


<script.py> output:
    USER : I'd like to order some coffee
    BOT : you'll have to log in first, what's your phone number?
    USER : 555-1234
    BOT : perfect, welcome back!
    BOT : would you like Colombian or Kenyan?
    USER : kenyan
    BOT : perfect, the beans are on their way!
    BOT : would you like Colombian or Kenyan?
```
