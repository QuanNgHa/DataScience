
### map(lambda function, list): 
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

### filter(lambda function with filter criteria, list ):
* The function `filter()` offers a way to filter out elements from a list that don't satisfy certain criteria.

```
# Create a list of strings: fellowship
fellowship = ['frodo', 'samwise', 'merry', 'pippin', 'aragorn', 'boromir', 'legolas', 'gimli', 'gandalf']

# Use filter() to apply a lambda function over fellowship: result
result = filter(lambda a: len(a)>6, fellowship)
# Convert result to list:
result_list = list(result)
```
