---
title: "Recursion: edit distance"
layout: post
date: 2019-09-04 2:28
image: /assets/images/favicon/GitHub-Mark.png
headerImage: false
tag:
- python
- edit distance
- Levenstein distance
category: blog
author: subaiwen
description: Algorithm for recursion applied to edit distance
---

## 1. Recursion:
Recursive: the algorithm recurs. i.e. the solution is built up of solutions to smaller parts of the same problem.  
A key feature is the “base case” where you can solve the problem in a straightforward way, without further reductions.  
For example, applying recursion to define a function of factorial of n ($n!$):  

```python
def fact(n):
	if n == 0:
		return 1
	elif:
		return n * fact(n-1)
	else:
		return float('inf')
```

## 2. Edit distance (Levenstein distance)
### Definition
The edit distance or Levenstein distance, $d_{Edit}(s,t)$, between two strings $s$ and $t$ is the minimum number of edit operations required to transform $s$ into $t$.  

An edit operation:

* an insertion of a character  
* deletion of a character  
* a mutation (i.e. substitution) of a character

### Example: “PRETTY” --> “PROTEIN”
<p align="center">
  <img src="https://tva1.sinaimg.cn/large/006y8mN6ly1g6njia1gxcj314u0hydja.jpg" width="500">
</p>
So the edit distance for the strings “PRETTY” and “PROTEIN” is 4.

### Theorem
It is possible express the edit distance recursively:

* The base case is when either of $s$ or $t$ has zero length. In this case, the other string must have been formed from entirely from insertions. So the edit distance must be the length of the (possibly) non-empty string.
* For the recursive case, we have to consider 2 possibilities:  
(1) The first characters of $s$ and $t$ are a match. In this case, the edit distance $d_{Edit}(s,t)$ is the same as $d_{Edit}(s′,t′)$ where $s′$ and $t′$ are the strings $s$, $t$ with the first character removed.  
(2) The first characters $s$ and $t$ do not match. In this case, the edit distance $d_{Edit}(s,t)$ is the minimum of
$$1+d_{Edit}(s′,t),\quad 1+d_{Edit}(s,t′),\quad and \quad 1+d_{Edit}(s′,t′)$$
where $s′$ and $t′$ are the strings $s$, $t$ with the first character removed.

### Python
#### Define function
In Python, we can define a script `edit_distance.py` to implement `edit_dist` function.

```python
def edit_dist(s,t):
	if len(s) == 0:
		dist = len(t)
	elif len(t) == 0:
		dist = len(s)
	else:
		if s[0] == t[0]:
			dist = edit_dist(s[1:],t[1:])
		else:
			dist = min( edit_dist(s[1:],t) + 1, edit_dist(s,t[1:]) + 1, edit_dist(s[1:],t[1:]) + 1)
	return dist
if __name__ == "__main__":
	s = input()
	t = input()
	print( edit_dist(s,t) )
```
Automate the testing in bash:
```bash
echo -e 'PRETTY\nPROTEIN' | python3 edit_distance.py
```   
`|`: pipe. Passes the output (stdout) of a previous command to the input (stdin) of the next one, or to the shell. 

#### Randomization and visualization
```python
import string, time, matplotlib.pyplot as plt
from random import choice
from edit_distance import edit_dist

def random_string(len=3, alphabet=string.ascii_uppercase):
# string.ascii_uppercase : 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'
	"""generate a random string of a specified length
	from a specified alphabet"""
	return "".join([ choice(alphabet) for _ in range(len)])
# choice(alphabet): randomly choose a character from alphabet
# [ choice(alphabet) for _ in range(len)]: create a list of random characters with the same length as len
# https://stackoverflow.com/questions/5893163/what-is-the-purpose-of-the-single-underscore-variable-in-python
# "".join([ choice(alphabet) for _ in range(len)]) : make a string out of a list, seperated with ""

times, ed_dists = [], []
for n in range(1000):
	s = random_string(len=10,alphabet="ACGT")
	t = random_string(len=10,alphabet="ACGT")
	start = time.time_ns()
	dist = edit_dist(s,t)
	end = time.time_ns()
	times.append(end-start)
	ed_dists.append(dist)
	if n % 10 == 0:
		print(n) # some feedback so we know it is running

plt.scatter(ed_dists, times)
plt.show()

```  
- `string.ascii_uppercase`: 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'  
- `choice(alphabet)`: randomly choose a character from alphabet.  
- `[choice(alphabet) for _ in range(len)]`: create a list of random characters with the same length as `len`.  
`_`: [throwaway index](https://stackoverflow.com/questions/5893163/what-is-the-purpose-of-the-single-underscore-variable-in-python) 
- `"".join()` : make a string out of the list, seperated with `""`.

#### Speed up
Wrap `edit_dist` with the memoizer `lru_cache` from the `functools` library. We can just modify the `edit_distance.py` file by adding two lines at the beginning:

```python
from functools import lru_cache

@lru_cache(maxsize=None) 
def edit_dist(s, t):
	# your code, as it was before
```

---

## Reference
[Advanced Bash-Scripting Guide: Chapter 3. Special Characters](https://www.tldp.org/LDP/abs/html/special-chars.html)  
[the single underscore “_” variable in Python?](https://stackoverflow.com/questions/5893163/what-is-the-purpose-of-the-single-underscore-variable-in-python)


