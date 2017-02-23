

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
sorted_ovlp_dict
```




    [('224', 0.20649556987318807),
     ('256', 0.17328397673168117),
     ('288', 0.14308609783913176),
     ('320', 0.11166053781587837),
     ('352', 0.09854080646822483),
     ('416', 0.0674471967456349),
     ('448', 0.05873466708126428),
     ('192', 0.05050963007965457),
     ('512', 0.047478858075688204),
     ('384', 0.04130575804550429),
     ('576', 0.03881904829477732),
     ('544', 0.0385088855681164),
     ('480', 0.032582899397945425),
     ('640', 0.030915722923657997),
     ('608', 0.029725247677242717),
     ('672', 0.027020430346626753),
     ('736', 0.022915245493799013),
     ('800', 0.017504214547269883),
     ('832', 0.01644883923366334),
     ('704', 0.016029373377999662),
     ('864', 0.016015168997401477),
     ('928', 0.013971795952551208),
     ('960', 0.013017181524699675),
     ('896', 0.01157878628606835),
     ('768', 0.009441963137046497)]




```python
print "max ovlp rate : " + str(sorted_ovlp_dict[0][1])
print "min ovlp rate : " + str(sorted_ovlp_dict[-1][1])
print "\n"


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

    max ovlp rate : 0.206495569873
    min ovlp rate : 0.00944196313705
    
    


### adjut


```python
# find out the data size for 2 overlapping rates: 0.25, 0.5 and 0.75
found_10 = found_20 = 0

for key, value in sorted_ovlp_dict:
    if (0.10 <= value < 0.12) and found_10 == 0:
        print "overlapping rate : " + str(value) + " data size : " + str(key)
        found_10 = 1
        
    if (0.20 <= value < 0.21) and found_20 == 0:
        print "overlapping rate : " + str(value) + " data size : " + str(key)
        found_20 = 1
```

    overlapping rate : 0.206495569873 data size : 224
    overlapping rate : 0.111660537816 data size : 320

