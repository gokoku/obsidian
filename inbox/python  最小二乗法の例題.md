#python

---
2021-09-18

# 最小二乗法の例題

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.optimize import curve_fit

x_observed = [9, 28, 38, 58, 88, 98, 108, 118, 128, 138, 148, 158, 168, 178, 188, 198, 208, 218, 228, 238, 278, 288, 298]
y_observed = [51, 80, 112, 294, 286, 110, 59, 70, 56, 70, 104, 59, 59, 72, 87, 99, 64, 60, 74, 151, 157, 57, 83]

x = np.array(x_observed)
y = np.array(y_observed)

plt.plot(x, y, marker = 'o', linestyle = 'none') 

def func(x, a, b):
  y = a * x + b
  return y

popt, pcov = curve_fit(func, x, y)

y_opt = popt[0] * x + popt[1]

plt.plot(x, y_opt, marker = "x")
```

![[Pasted image 20210918174038.png]]
