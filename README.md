# PYLEAPS

This package wants to be a simple porting of the regsubset function from R.
Unlike `leaps` in `R`, the package is not optimized yet, and requires extra work to improve code readability.
For now, I have implemented forward/backward/best subset selection for linear regression, building on top
of the excellent `statsmodels` package.

In addition, for now, the user needs to manually code the categorical variable contrasts. This will be fixed in the future.

The package can be easily installed through pip. Check out https://pypi.org/project/pyleaps/ for details

# TO INSTALL

`pip install pyleaps`

The relevant dependencies should automatically get installed, in case they are not present in the environment

# COLLABORATION

Any help/collaboration is very welcome. Just let me know what kind of edits you propose and I will be very happy to discuss them.

# TO DOs
This section contains a list of future edits:

1. Improve general code readability  
1. Figure out a way to speed up best subset section. So far, it is way slower than the R counterpart.

# USAGE EXAMPLE
```python
import pandas as pd
import pyleaps
import matplotlib.pyplot as plt
```


```python
df = pd.read_csv("https://archive.ics.uci.edu/ml/machine-learning-databases/00291/airfoil_self_noise.dat", sep="\t", header=None)
df.columns = ["freq", "aoa", "ch_len", "u", "suc_thick", "sound_db"]
```

## 1. Best Model Selection


```python
pyleaps.regsubsets(df, "sound_db", df.columns.to_list(), intercept=True, method="full").summary
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
      <th>r2</th>
      <th>r2_adj</th>
      <th>bic</th>
      <th>aic</th>
      <th>ssr</th>
      <th>vars</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>0.152655</td>
      <td>0.152091</td>
      <td>9835.55871</td>
      <td>9824.928273</td>
      <td>60570.206223</td>
      <td>[intercept, freq]</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.323783</td>
      <td>0.322882</td>
      <td>9503.806546</td>
      <td>9487.860891</td>
      <td>48337.58386</td>
      <td>[intercept, freq, suc_thick]</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.43992</td>
      <td>0.438799</td>
      <td>9227.905534</td>
      <td>9206.64466</td>
      <td>40035.855465</td>
      <td>[intercept, freq, ch_len, suc_thick]</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.484574</td>
      <td>0.483198</td>
      <td>9110.342833</td>
      <td>9083.766741</td>
      <td>36843.885086</td>
      <td>[intercept, u, freq, aoa, ch_len]</td>
    </tr>
    <tr>
      <th>5</th>
      <td>0.51571</td>
      <td>0.514092</td>
      <td>9024.006788</td>
      <td>8992.115478</td>
      <td>34618.219133</td>
      <td>[intercept, u, freq, aoa, ch_len, suc_thick]</td>
    </tr>
  </tbody>
</table>
</div>




```python
pyleaps.regsubsets(df, "sound_db", df.columns.to_list(), intercept=False, method="full").summary
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
      <th>r2</th>
      <th>r2_adj</th>
      <th>bic</th>
      <th>aic</th>
      <th>ssr</th>
      <th>vars</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>0.915417</td>
      <td>0.915361</td>
      <td>15074.73871</td>
      <td>15069.423491</td>
      <td>1987206.653961</td>
      <td>[u]</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.925304</td>
      <td>0.925204</td>
      <td>14895.227275</td>
      <td>14884.596838</td>
      <td>1754927.356895</td>
      <td>[u, ch_len]</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.937199</td>
      <td>0.937073</td>
      <td>14641.845804</td>
      <td>14625.900149</td>
      <td>1475469.985069</td>
      <td>[u, aoa, ch_len]</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.938688</td>
      <td>0.938524</td>
      <td>14613.094355</td>
      <td>14591.833481</td>
      <td>1440485.372375</td>
      <td>[u, freq, aoa, ch_len]</td>
    </tr>
    <tr>
      <th>5</th>
      <td>0.939991</td>
      <td>0.93979</td>
      <td>14588.128228</td>
      <td>14561.552136</td>
      <td>1409876.596034</td>
      <td>[u, freq, aoa, ch_len, suc_thick]</td>
    </tr>
  </tbody>
</table>
</div>



## 2. Forward Selection


```python
pyleaps.regsubsets(df, "sound_db", df.columns.to_list(), intercept=True, method="forward").summary
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
      <th>r2</th>
      <th>r2_adj</th>
      <th>bic</th>
      <th>aic</th>
      <th>ssr</th>
      <th>vars</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>0.152655</td>
      <td>0.152091</td>
      <td>9835.55871</td>
      <td>9824.928273</td>
      <td>60570.206223</td>
      <td>[intercept, freq]</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.323783</td>
      <td>0.322882</td>
      <td>9503.806546</td>
      <td>9487.860891</td>
      <td>48337.58386</td>
      <td>[intercept, freq, suc_thick]</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.43992</td>
      <td>0.438799</td>
      <td>9227.905534</td>
      <td>9206.64466</td>
      <td>40035.855465</td>
      <td>[intercept, freq, suc_thick, ch_len]</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.477646</td>
      <td>0.476251</td>
      <td>9130.411111</td>
      <td>9103.835019</td>
      <td>37339.129014</td>
      <td>[intercept, freq, suc_thick, ch_len, u]</td>
    </tr>
    <tr>
      <th>5</th>
      <td>0.51571</td>
      <td>0.514092</td>
      <td>9024.006788</td>
      <td>8992.115478</td>
      <td>34618.219133</td>
      <td>[intercept, freq, suc_thick, ch_len, u, aoa]</td>
    </tr>
  </tbody>
</table>
</div>




```python
pyleaps.regsubsets(df, "sound_db", df.columns.to_list(), intercept=False, method="forward").summary
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
      <th>r2</th>
      <th>r2_adj</th>
      <th>bic</th>
      <th>aic</th>
      <th>ssr</th>
      <th>vars</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>0.915417</td>
      <td>0.915361</td>
      <td>15074.73871</td>
      <td>15069.423491</td>
      <td>1987206.653961</td>
      <td>[u]</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.925304</td>
      <td>0.925204</td>
      <td>14895.227275</td>
      <td>14884.596838</td>
      <td>1754927.356895</td>
      <td>[u, ch_len]</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.937199</td>
      <td>0.937073</td>
      <td>14641.845804</td>
      <td>14625.900149</td>
      <td>1475469.985069</td>
      <td>[u, ch_len, aoa]</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.938688</td>
      <td>0.938524</td>
      <td>14613.094355</td>
      <td>14591.833481</td>
      <td>1440485.372375</td>
      <td>[u, ch_len, aoa, freq]</td>
    </tr>
    <tr>
      <th>5</th>
      <td>0.939991</td>
      <td>0.93979</td>
      <td>14588.128228</td>
      <td>14561.552136</td>
      <td>1409876.596034</td>
      <td>[u, ch_len, aoa, freq, suc_thick]</td>
    </tr>
  </tbody>
</table>
</div>



## 3. Backward Selection


```python
pyleaps.regsubsets(df, "sound_db", df.columns.to_list(), intercept=True, method="backward").summary
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
      <th>r2</th>
      <th>r2_adj</th>
      <th>bic</th>
      <th>aic</th>
      <th>ssr</th>
      <th>vars</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>5</th>
      <td>0.51571</td>
      <td>0.514092</td>
      <td>9024.006788</td>
      <td>8992.115478</td>
      <td>34618.219133</td>
      <td>[intercept, u, freq, aoa, ch_len, suc_thick]</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.484574</td>
      <td>0.483198</td>
      <td>9110.342833</td>
      <td>9083.766741</td>
      <td>36843.885086</td>
      <td>[intercept, u, freq, aoa, ch_len]</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.427997</td>
      <td>0.426852</td>
      <td>9259.565127</td>
      <td>9238.304253</td>
      <td>40888.126119</td>
      <td>[intercept, freq, aoa, ch_len]</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.227202</td>
      <td>0.226172</td>
      <td>9704.462269</td>
      <td>9688.516614</td>
      <td>55241.410754</td>
      <td>[intercept, freq, aoa]</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.152655</td>
      <td>0.152091</td>
      <td>9835.55871</td>
      <td>9824.928273</td>
      <td>60570.206223</td>
      <td>[intercept, freq]</td>
    </tr>
  </tbody>
</table>
</div>




```python
pyleaps.regsubsets(df, "sound_db", df.columns.to_list(), intercept=False, method="backward").summary
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
      <th>r2</th>
      <th>r2_adj</th>
      <th>bic</th>
      <th>aic</th>
      <th>ssr</th>
      <th>vars</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>5</th>
      <td>0.939991</td>
      <td>0.93979</td>
      <td>14588.128228</td>
      <td>14561.552136</td>
      <td>1409876.596034</td>
      <td>[u, freq, aoa, ch_len, suc_thick]</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.938688</td>
      <td>0.938524</td>
      <td>14613.094355</td>
      <td>14591.833481</td>
      <td>1440485.372375</td>
      <td>[u, freq, aoa, ch_len]</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.937199</td>
      <td>0.937073</td>
      <td>14641.845804</td>
      <td>14625.900149</td>
      <td>1475469.985069</td>
      <td>[u, aoa, ch_len]</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.925304</td>
      <td>0.925204</td>
      <td>14895.227275</td>
      <td>14884.596838</td>
      <td>1754927.356895</td>
      <td>[u, ch_len]</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.915417</td>
      <td>0.915361</td>
      <td>15074.73871</td>
      <td>15069.423491</td>
      <td>1987206.653961</td>
      <td>[u]</td>
    </tr>
  </tbody>
</table>
</div>


