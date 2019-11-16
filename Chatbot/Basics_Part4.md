Combining the coffee ordering bot with the ELIZA rules from chapter 1.

To begin, you'll define a function called chitchat_response(), which calls the predefined function match_rule() from back in chapter 1. This returns a response if the message matched an ELIZA template, and otherwise, None.

The ELIZA rules are contained in a dictionary called eliza_rules

```Python
# Define chitchat_response()
def chitchat_response(message):
    # Call match_rule()
    response, phrase = match_rule(eliza_rules, message)
    # Return none if response is "default"
    if response == "default":
        return None
    if '{0}' in response:
        # Replace the pronouns of phrase
        phrase = replace_pronouns(phrase)
        # Calculate the response
        response = response.format(phrase)
    return response
    
 ```
