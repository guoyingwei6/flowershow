---
created: 2024-08-02 16:12
updated: 2025-06-10 15:00
title:
tags:
  - garden/🌱Seedling
---
```python
import os
import sys
import logging

logging.basicConfig(filename='{0}.log'.format(os.path.basename(__file__).replace('.py','')),
                    format='%(asctime)s: %(name)s: %(levelname)s: %(message)s',level=logging.DEBUG,filemode='a')
#logging.info(f"The command line is:\n\tpython3 {' '.join(sys.argv)}")
logging.info("The command line is:\n\tpython3 {}".format(' '.join(sys.argv)))
```

