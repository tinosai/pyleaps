## PYLEAPS

This package wants to be a simple porting of the regsubset function from R.
Unlike `leaps` in `R`, the package is not optimized yet, and requires extra work to improve code readability.
For now, I have implemented forward/backward/best subset selection for linear regression, building on top
of the excellent `statsmodels` package.

In addition, for now, the user needs to manually code the categorical variable contrasts. This will be fixed in the future.

The package can be easily installed through pip. Check out https://pypi.org/project/pyleaps/ for details

## TO INSTALL

`pip install pyleaps`

The relevant dependencies should automatically get installed, in case they are not present in the environment
