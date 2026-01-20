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

