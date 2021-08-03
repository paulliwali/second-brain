# Python Basics

## Dictionary

Dictionary comprehension for filtering items in `some_dict` that exists in `some_list`

```python
some_list = [1, 2, 3]
new_dict = {k: v for k, v in some_dict.itmes() if k in some_list}

```

Nested dictionary comprehension

```python
some_list = [1, 2, 3]
new_dict = {
	outer_k: {
		inner_k: inner_v for inner_k, inner_v in outer_v.items() if inner_k in some_list
	} for outer_k, outer_v in some_dict.items() if outer_k in some_list
}
```

## Time and Dates

Don't initialize timezone when creating datetime objects, localize the naive datetime. [Source](http://pytz.sourceforge.net/#localized-times-and-date-arithmetic)

```python
import pytz
from datetime import datetime

pdt = pytz.timezone('America/Los_Angeles')
bad_tz_aware_datetime = datetime(2014, 1, 1, tzinfo=pdt)
naive_datetime = datetime(2014, 1, 1)
good_tz_aware_datetime = pdt.localize(naive_datetime)
```

## Logging

```python
# Setup logger for scripts
import logging
# Create a custom logger
logger = logging.getLogger(__name__)
# Create handlers
c_handler = logging.StreamHandler()
f_handler = logging.FileHandler('file.log')
c_handler.setLevel(logging.WARNING)
f_handler.setLevel(logging.ERROR)
# Create formatters and add it to handlers
c_format = logging.Formatter('%(name)s - %(levelname)s - %(message)s')
f_format = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')
c_handler.setFormatter(c_format)
f_handler.setFormatter(f_format)
# Add handlers to the logger
logger.addHandler(c_handler)
logger.addHandler(f_handler)
logger.warning('This is a warning')
logger.error('This is an error')
```

## Imports

```python
# Import modules from root 'src' folder
import sys
src_path = os.path.abspath("../../src")
if src_path not in sys.path:
    sys.path.append(src_path)
```

## Built-ins

`yield` - gets a generator object from the function. When Python calls a function with `yield` it will execute code up to it and pauses to deliver the object, when it is called again it will resume from that statement until the next `yield`

`generators` - a special kind of `iterator` that you can only iterator over once and doesn't store values in memory




