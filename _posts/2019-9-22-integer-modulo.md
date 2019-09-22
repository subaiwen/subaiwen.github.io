---
title: "Ring of integers modulo n"
layout: post
date: 2019-09-22 1:58
image: /assets/images/favicon/GitHub-Mark.png
headerImage: false
tag:
- Number Theory
- Python
category: blog
author: subaiwen
description: Ring of integers modulo n in python
---

## Overview
In the ring $Z/nZ$, “the integers modulo n”, the elements of $Z/nZ$ are equivalence classes of integers; two integers are considered equivalent in $Z/nZ$ if they differ by some number of copies of n.  
When we let n=4, there are 4 classes: 4k, 1 + 4k, 2 + 4k, and 3 + 4k. The arithmetic tables are then:  
<p align="center">
  <img src="https://tva1.sinaimg.cn/large/006y8mN6ly1g78a75v4gtj30yu09c750.jpg" width="500">
</p>
The task to implement a class `IntMod` which supports the relevant arithmetic operations in python: +, -, *, and /.

---
## Class
### Define IntMod
```python
class IntMod(object):
	""" Encapsulates an element of the Ring Z/nZ """

	def __init__(self, rep, modulus):
		if not isinstance(rep, int) or not isinstance(modulus, int):
			raise NotImplementedError("int argument expected")
		self._rep = rep
		self._modulus = modulus
		# make sure _rep in the right range
		while self._rep < 0:
			self._rep += modulus
		self._rep = self._rep % self._modulus

	def __str__(self):
		return str(self._rep) + " (mod " + str(self._modulus) + ")"

	def __repr__(self):
		return "IntMod(" + str(self._rep) + "," + str(self._modulus) + ")"

	def __int__(self):
		return self._rep
```
The class requires 2 integer input and displays like `3 (mod 7)`: a remainder with a modulus in parentheses. 

### Devision operation
#### 1. Multiplication Table
To define division of two `IntMod`, we need to confirm if the divisor is a unit of the ring. Before that, we first compute the multiplication table for latter convenient:

```python
def table(self):
	""" Multiplication table """
	table = []
	for row in range(self._modulus):
		for col in range(self._modulus):
			table.append(row * col % self._modulus)
	return table
```

#### 2. Unit
> In ring theory we say that an element $u$ of a ring $R$ is a unit if it has an inverse in $R$, that is, if there is another element $v\in R$ such that $uv=vu=1$. 

Find the list of units ($u$):  

```python
def unit(self):
	""" units of the ring """
	u = []
	for i in range(len(self.table())):
		if self.table()[i] == 1:
			u.append(i % self._modulus)
	return u
```

#### 3. Inverse
If the divisor is a unit, division can be defined. But we cannot directly compute $a/b$ under modulo $n$, since it may give us float numbers. We do a little trick here: $$ a/b \; \% \; n = (\:inverse(b) \; * \; a\:) \; \% \; m$$  
So we can ensure an integer output of the division. To find the inverse ($v$):  

```python
def inver(self):
	""" inverse of the unit in the ring """
	if self._rep in self.unit():
		for row in range(len(self.table())):
			if self.table()[row*self._modulus + self._rep - 1] == 1:
				return row
	else:
		raise NotImplementedError("Divisor is not a unit")
```

#### 4. Division
```python
def __truediv__(self, other):
	"""float devision of self over other as IntMod """
	if isinstance(other, int):
		other = IntMod(other, self._modulus)
	if isinstance(other, IntMod):
		return IntMod(other.inver() * self._rep, self._modulus)
```

### Example
#### Division
```python
if __name__ == "__main__": 
	pass # unit tests go here

from mod_arith import IntMod
a = IntMod(3, 7)
b = IntMod(4, 7)
print(a/b)
```
```
>> 1 (mod 7)
```

#### Show the multiplication table
```python
import numpy as np
import pandas as pd

table = a.table()[:]
table = np.array(table).reshape(36,36)
print(pd.DataFrame(
	data = table[:,:], 
	index = [i for i in range(36)], 
	columns = [i for i in range(36)]))
```
<p align="center">
  <img src="https://tva1.sinaimg.cn/large/006y8mN6ly1g78bfld5lwj30ve0sw7ke.jpg" width="500">
</p>

---
## Reference
[Modular arithmetic](https://www.khanacademy.org/computing/computer-science/cryptography/modarithmetic/a/what-is-modular-arithmetic)  
[Modular Division](https://www.geeksforgeeks.org/modular-division/)
