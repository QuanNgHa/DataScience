
### map(): 
* is to apply the lambda function to all elements in the list
* return as a object --> need to convert back to list using 'list()'
```Python
# Create a list of strings: spells
spells = ["protego", "accio", "expecto patronum", "legilimens"]

# Use map() to apply a lambda function over spells: shout_spells
shout_spells = map(lambda a: a + '!!!', spells)
print(type(shout_spells))
```
Output: <class 'map'>
```
# Convert shout_spells to a list: shout_spells_list
shout_spells_list = list(shout_spells)

# Print the result
print(shout_spells_list)
```
Output: ['protego!!!', 'accio!!!', 'expecto patronum!!!', 'legilimens!!!']
