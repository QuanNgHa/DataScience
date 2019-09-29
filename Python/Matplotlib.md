### Clean up the plot
```Python
plt.clf()
```

### Customize the plot
```Python
# Scatter plot
plt.scatter(gdp_cap, life_exp)

# Previous customizations
plt.xscale('log') 
plt.xlabel('GDP per Capita [in USD]')
plt.ylabel('Life Expectancy [in years]')
plt.title('World Development in 2007')

# Definition of tick_val and tick_lab
tick_val = [1000, 10000, 100000]
tick_lab = ['1k', '10k', '100k']

# Adapt the ticks on the x-axis
plt.xticks(tick_val, tick_lab)

# After customizing, display the plot
plt.show()
```
<img width="392" alt="Screenshot 2019-09-29 at 12 34 11 PM" src="https://user-images.githubusercontent.com/47073386/65826331-8633a400-e2b5-11e9-9d09-cecefcfccc10.png">
