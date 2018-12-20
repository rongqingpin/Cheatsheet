### System

```python
import sys
sys.path.append('otherDirectories') # add other paths to current directory
... # for debugging purpose; stop the code at exit()
sys.exit()
```

```python
import time
t0 = time.time()
# here goes some codes
print(time.time() - t0) # execution time
```

```python
import os
dir_path = os.path.dirname(os.path.realpath(__file__)) # current working directory (without /)
for filename in os.listdir(dir_path):
    # loop through all files in folder
```

### Math & Stats Related

```python
import math
x2 = math.floor(x)
```

```python
import sklearn.preprocessing as prep
X2 = prep.StandardScaler().fit_transform(X)

from sklearn.decomposition import PCA
pca = PCA(n_components = ..., svd_solver = 'full')
pca.fit(X)
PCA(copy = True, n_components = ..., whiten = False)
Y = pca.transform(X) # these are the scores for each PC
lamda = pca.explained_variance_ratio_
```

```python
import statsmodels.api as sm

x = sm.add_constant(x) # add intercept - no included by default
results = sm.OLS(y, x).fit()

# or fit from formula:
fformula = 'np.function(y) ~ 1 + x + np.power(x, 2) + np.log(...)' # y & x should be columns of dataframe X
results = sm.OLS.from_formula(formula = fformula, data = X).fit()

print(results.summary())
y2 = results.fittedvalues # least square line
ynew = results.predict(xnew)
XconfInterval = results.conf_int(alpha = 0.05) # no header: cols = None
residuals = results.resid
rsquare   = results.rsquared
tvals     = results.tvalues
pvals     = results.pvalues
betas     = results.params
betaStds  = results.bse

from statsmodels.stats import outliers_influence
VIF_i = outliers_influence.variance_inflation_factor(X.values, icol)

# diagnostic plots
fig = sm.qqplot(results.resid_pearson) # QQ plot. if add to existing plot: ax = ax1 (see matplotlib for ax)
plt.show()
infl = results.get_influence()
resd_std = infl.resid_studentized_internal # outliers > +/- 3
hatDiag  = infl.hat_diag_factor            # high leverage points > (Ncol + 1) / Nrow
# or use 
fig, ax = plt.subplots(figsize = (Nw, Nh))
fig = sm.graphics.influence_plot(results, ax = ax)

from statsmodels.stats import anova
anova.anova_lm(results1, results2)
```

### Database Related

```python
# MySQL
import MySQLdb
conn = MySQLdb.connect(host=<'host'>, user=<'user'>, passwd=<'password'>, db=<'database'>)
```

[pyodbc](https://github.com/mkleehammer/pyodbc/wiki/Getting-started)
```python
# MS Access
import pyodbc
fdriver = 'DRIVER={Microsoft Access Driver (*.mdb, *.accdb)};'
fDB = 'DBQ=' + <'path'> + <'filename'> + ';'
conn = pyodbc.connect(fdriver + fDB)
cursor = conn.cursor()
cursor.execute(query_string) # query_string: the gramma of MS Access is very similar to SQL
X = cursor.fetchone() # return one line as row objects, can access through X[index] or X.columnName; if no data, None returned
X = cursor.fetchall()
```