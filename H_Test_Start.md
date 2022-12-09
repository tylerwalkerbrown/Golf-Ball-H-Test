```python
%matplotlib inline

import numpy as np
import pandas as pd
import scipy.stats as stats
import matplotlib.pyplot as plt
import math
import os
import seaborn as sns
```

# Hypothesis Test Old vs New Golf Ball


```python
os.chdir('Desktop/Practice data')
```


```python
df = pd.read_csv('Golf.csv')
```


```python
current = df.Current
new = df.New
```


```python
alll = df.melt(id_vars=["Current"], 
        var_name="Current")
```


```python
stats.ttest_1samp(a = df.Current,               # Sample data
                 popmean = alll.value.mean())  # Pop mean
```




    Ttest_1sampResult(statistic=2.0051035546145037, pvalue=0.051927680618661425)




```python
stats.t.ppf(q=0.025,  # Quantile to check
            df=49)  # Degrees of freedom
```




    -2.0095752344892093




```python
stats.t.ppf(q=0.975,  # Quantile to check
            df=49)  # Degrees of freedom
```




    2.009575234489209




```python
stats.t.cdf(x= -2.5742,      # T-test statistic
               df= 49) * 2   # Multiply by two for two tailed test *
```




    0.013121066545690117




```python
sigma = df.Current.std()/math.sqrt(40)  # Sample stdev/sample size

stats.t.interval(0.95,                        # Confidence level
                 df = 49,                     # Degrees of freedom
                 loc = alll.value.mean(), # Sample mean
                 scale= sigma)        
```




    (264.71881133626556, 270.28118866373444)




```python
stats.t.interval(alpha = 0.99,                # Confidence level
                 df = 49,                     # Degrees of freedom
                 loc = alll.value.mean(), # Sample mean
                 scale= sigma)                # Standard dev estimate
```




    (263.79103109926046, 271.20896890073954)



# Two-Sample T


```python
stats.ttest_ind(a= df.Current,
                b= df.New,
                equal_var=False)    # Assume samples have equal variance?
```




    Ttest_indResult(statistic=1.3283615935245678, pvalue=0.18798994530489838)



# Paired T-Test


```python
#Summary statistics of current,new and the difference 
weight_df = pd.DataFrame({"Current Drive":current,
                          "Drive New Ball":new,
                          "Difference":new-current})

weight_df.describe()             # Check a summary of the data
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Current Drive</th>
      <th>Drive New Ball</th>
      <th>Difference</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>40.000000</td>
      <td>40.000000</td>
      <td>40.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>270.275000</td>
      <td>267.500000</td>
      <td>-2.775000</td>
    </tr>
    <tr>
      <th>std</th>
      <td>8.752985</td>
      <td>9.896904</td>
      <td>13.743973</td>
    </tr>
    <tr>
      <th>min</th>
      <td>255.000000</td>
      <td>250.000000</td>
      <td>-32.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>263.000000</td>
      <td>262.000000</td>
      <td>-10.750000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>270.000000</td>
      <td>265.000000</td>
      <td>-2.500000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>275.250000</td>
      <td>274.500000</td>
      <td>6.500000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>289.000000</td>
      <td>289.000000</td>
      <td>27.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
stats.ttest_rel(a = current,
                b = new)
```




    Ttest_relResult(statistic=1.2769699827911767, pvalue=0.20916361823147053)




```python
sns.distplot(current,color='green',hist=False, label = 'current ball')
sns.distplot(new,color='red',hist=False, label = 'new ball')
plt.legend()
```

    /Users/tylerbrown/opt/anaconda3/lib/python3.9/site-packages/seaborn/distributions.py:2619: FutureWarning: `distplot` is a deprecated function and will be removed in a future version. Please adapt your code to use either `displot` (a figure-level function with similar flexibility) or `kdeplot` (an axes-level function for kernel density plots).
      warnings.warn(msg, FutureWarning)
    /Users/tylerbrown/opt/anaconda3/lib/python3.9/site-packages/seaborn/distributions.py:2619: FutureWarning: `distplot` is a deprecated function and will be removed in a future version. Please adapt your code to use either `displot` (a figure-level function with similar flexibility) or `kdeplot` (an axes-level function for kernel density plots).
      warnings.warn(msg, FutureWarning)





    <matplotlib.legend.Legend at 0x7fa1417e1cd0>




    
![png](output_17_2.png)
    



```python
lower_quantile = current(0.025)  # Lower cutoff value
upper_quantile = current.norm.ppf(0.975)  # Upper cutoff value

# Area under alternative, to the left the lower cutoff value
low = stats.norm.cdf(lower_quantile,    
                     loc=3,             
                     scale=2)

# Area under alternative, to the left the upper cutoff value
high = stats.norm.cdf(upper_quantile, 
                      loc=3, 
                      scale=2)          

# Area under the alternative, between the cutoffs (Type II error)
high-low
```


      Input In [106]
        lower_quantile = current.(0.025)  # Lower cutoff value
                                 ^
    SyntaxError: invalid syntax




```python

```
