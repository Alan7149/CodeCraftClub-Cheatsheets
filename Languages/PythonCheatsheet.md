# 🐍 Python Cheatsheet

> Part of [CodeCraftClub-Cheatsheets](../README.md) · A quick, practical reference for everyday Python.

---

## Hello World & Running

```python
print("Hello, World!")
```

```bash
python script.py        # run a file
python -m venv .venv     # create a virtual environment
pip install requests     # install a package
```

## Variables & Types

```python
name = "Alan"          # str
age = 21                # int
pi = 3.14               # float
is_active = True        # bool
nothing = None          # NoneType

type(age)               # <class 'int'>
int("42"), str(42), float("3.5"), bool(0)   # casting
```

## Strings

```python
s = "Code Craft Club"
s.lower(); s.upper(); s.title()
s.strip(); s.replace("Club", "Crew")
s.split(" ")             # ['Code', 'Craft', 'Club']
"-".join(["a", "b"])    # 'a-b'
len(s); s[0]; s[-1]; s[0:4]   # indexing & slicing
f"{name} is {age}"       # f-string formatting
"abc".startswith("a"); "abc".find("b")
```

## Numbers & Math

```python
7 // 2     # 3  floor division
7 % 2      # 1  modulo
2 ** 10    # 1024  power
abs(-5); round(3.567, 2); min(3, 9); max(3, 9)
import math
math.sqrt(16); math.ceil(2.1); math.floor(2.9); math.pi
```

## Collections

```python
# List (ordered, mutable)
nums = [1, 2, 3]
nums.append(4); nums.insert(0, 0); nums.pop(); nums.remove(2)
nums[::-1]               # reverse
sorted(nums); nums.sort(reverse=True)

# Tuple (immutable)
point = (4, 5)

# Set (unique, unordered)
s = {1, 2, 2, 3}        # {1, 2, 3}
s.add(4); s & {2, 4}; s | {5}

# Dict (key-value)
user = {"name": "Alan", "age": 21}
user["name"]; user.get("email", "n/a")
user.keys(); user.values(); user.items()
user["email"] = "a@b.com"
```

## Comprehensions

```python
squares = [x*x for x in range(5)]            # [0,1,4,9,16]
evens   = [x for x in range(10) if x % 2 == 0]
mapping = {k: v for k, v in [("a", 1)]}
unique  = {c for c in "banana"}
```

## Control Flow

```python
if age >= 18:
    print("adult")
elif age > 12:
    print("teen")
else:
    print("kid")

for i in range(3):       # 0, 1, 2
    print(i)

for k, v in user.items():
    print(k, v)

while age < 30:
    age += 1
    if age == 25: continue
    if age == 28: break

status = "on" if is_active else "off"   # ternary
```

## Functions

```python
def greet(name, greeting="Hi"):
    return f"{greeting}, {name}!"

def total(*args, **kwargs):     # variadic + keyword args
    return sum(args)

double = lambda x: x * 2         # anonymous function

def add(a: int, b: int) -> int:  # type hints
    return a + b
```

## Error Handling

```python
try:
    risky()
except ValueError as e:
    print("bad value", e)
except (TypeError, KeyError):
    print("type/key error")
else:
    print("no error")
finally:
    print("always runs")

raise ValueError("custom message")
```

## Classes & OOP

```python
class Animal:
    species = "unknown"          # class attribute

    def __init__(self, name):
        self.name = name          # instance attribute

    def speak(self):
        return f"{self.name} makes a sound"

    def __str__(self):
        return f"Animal({self.name})"

class Dog(Animal):               # inheritance
    species = "dog"
    def speak(self):
        return f"{self.name} barks"

d = Dog("Rex")
d.speak(); isinstance(d, Animal)   # True
```

## Files & JSON

```python
with open("data.txt", "r", encoding="utf-8") as f:
    content = f.read()           # or f.readlines()

with open("out.txt", "w") as f:
    f.write("hello")

import json
data = json.loads('{"a": 1}')
text = json.dumps({"a": 1}, indent=2)
```

## Handy Built-ins

```python
enumerate(["a", "b"])      # (0,'a'), (1,'b')
zip([1, 2], ["a", "b"])    # (1,'a'), (2,'b')
map(str, [1, 2, 3]); filter(lambda x: x > 0, [-1, 2])
any([False, True]); all([True, True])
range(0, 10, 2)             # 0,2,4,6,8
```

## Useful Standard Library

```python
from collections import Counter, defaultdict, deque
from itertools import combinations, permutations, product
from datetime import datetime, timedelta
import os, sys, random, re

Counter("banana")           # {'a':3,'n':2,'b':1}
datetime.now().isoformat()
random.choice([1, 2, 3]); random.randint(1, 6)
re.findall(r"\d+", "a1b22")  # ['1', '22']
```

---

[🔝 Back to README](../README.md)
