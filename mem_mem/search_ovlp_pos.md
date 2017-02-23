

```python
import warnings
import pandas as pd
import numpy as np
import os
import operator # sorting
from read_trace import *

warnings.filterwarnings("ignore", category=np.VisibleDeprecationWarning)
```


```python
# ls all the trace files in the targeted folder
target_folder = './profile_results'
trace_list = []
for root, dirs, files in os.walk(target_folder):
    for file in files:
        if 'trace' in file:
            trace_list.append(file)
```


```python
# record the overlapping rate for different data size
ovlp_dict = {}

for item in trace_list:
    trace_file = target_folder + "/" + item
    current_ovlp = check_kernel_ovlprate(trace_file)
    # find out the data size
    N = item.replace("trace_", "").replace(".csv","")
    ovlp_dict[N] = current_ovlp
```


```python
# you don't need to do this step
sorted_ovlp_dict = sorted(ovlp_dict.items(), key=operator.itemgetter(1), reverse=True)
```


```python
# find out the data size for 2 overlapping rates: 0.25, 0.5 and 0.75
found_75 = found_50 = found_25 = 0

for key, value in sorted_ovlp_dict:
    if (0.75 <= value < 0.76) and found_75 == 0:
        print "overlapping rate : " + str(value) + " data size : " + str(key)
        found_75 = 1
        
    if (0.50 <= value < 0.51) and found_50 == 0:
        print "overlapping rate : " + str(value) + " data size : " + str(key)
        found_50 = 1
        
    if (0.25 <= value < 0.26) and found_25 == 0:
        print "overlapping rate : " + str(value) + " data size : " + str(key)
        found_25 = 1
```

    overlapping rate : 0.752251115039 data size : 11100
    overlapping rate : 0.509615384617 data size : 4300
    overlapping rate : 0.257055159634 data size : 47300

