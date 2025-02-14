---
layout: single
title:  "Helper functions for profiling"
date:   2025-02-12
category:
    coding
tags:
  - python
  - profiling
  - optimisation
---

This post is a bit of a holding place for code I find helpful for quickly profiling the CPU or RAM that isn't comprehensive enough to go in a github repo.

## Timing

### Printing time

```
import time

# start timing
start_time = time.time()

# place some function here
time.sleep(5)

# print elapsed time 
elapsed_time = time.time() - start_time
hours = int(elapsed_time // 3600)
minutes = int((elapsed_time % 3600) // 60)
seconds = int(elapsed_time % 60)
print(f"Execution Time: {hours}h {minutes}m {seconds}s")
```


### A simple timing decorator function

```
from functools import wraps

def timefn(fn):
    @wraps(fn)
    def measure_time(*args, **kwargs):
        t1 = time.time()
        result = fn(*args, **kwargs)
        t2 = time.time()
        print(f"@timefn: {fn.__name__} took {(t2 - t1):0.2f} seconds")
        return result
    return measure_time

@timefn
def some_function:
    pass
```