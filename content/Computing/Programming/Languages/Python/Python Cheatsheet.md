---
aliases:
  - python cheatsheet
  - Python cheatsheet
tags:
  - computing
  - data
  - maths
  - modeling
---
# Package Management
Package management in [[Python]] helps organise collections of downloadable software packages. There are a number of package managers available, each with their own unique advantages. However, [[#UV]] has become a personal favourite. 

## Pip
## Conda
## Mamba
## UV


# Lists
## Instantiation
```python
new_list = [value]*length
```

## List Comprehension

```python
new_list = [value for iteration in length]
```


# Arrays
Arrays in  [[Python]] use the `numpy` library. 
```python
import numpy as np
```


# Loops

## Breaking Out of Nested Loops


```python
list1 = [1, 2, 3]
list2 = [9, 8, 7]

for i in list1:
	for j in list2:
		print(i, j)
		
```

in [[Matrices|matrix]] format:

```python 
A = [[a11, a12, a13],
	[a21, a22, a23],
	[a31, a32, a33]]

for row in A:
	for column in row:
		print(row, column)

```

But what if we wanted to get a single element `a22` from this nested list? using `break` inside the inner loop only stops the inner loop, at the next iteration of the outer loop, the inner loop will continue. 

This is solved by

```python
A = [[a11, a12, a13],
	[a21, a22, a23],
	[a31, a32, a33]]
	
for row in A:
	for item in row:
		if item == 'a22':
			break #breaks the inner loop
	else:
		continue # finishes the inner loop without breaking
	break
	# otherwise break the outer loop

```

### Using Flags

```python
A = [[a11, a12, a13],
	[a21, a22, a23],
	[a31, a32, a33]]
	
flag = False
for row in A:
	for item in row:
		if item == 'a22':
			flag = True
			break #breaks the inner loop
	if flag
		break
		# breaks the outer loop
```

### Three +

```python 

A = [[[a111]], [a121], [a131]],
	[[a211], [a221], [a231]],
	[[a311], [a321], [a331]]]
	
flag = False
for row in A:
	for item in row:
		for cell in item
			if cell == 'a22':
				flag = True
				break #breaks the inner loop
		if flag
		break # breaks the middle loop
	if flag
		break
		# breaks the outer loop
		
```