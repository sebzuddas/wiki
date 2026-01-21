---
aliases:
  - search
  - Searching algorithms
  - searching algorithms
tags:
  - data
  - engineering
  - computing
---
# What are Searching Algorithms?

# Searching [[Data Structures]]

# Searching [[Graph Theory|Graphs]]

# [Binary Search](https://pseudoeditor.com/guides/binary-search)

```pseudocode
function binary_search(list, target):
    left = 0
    right = length(list) - 1
    while left <= right:
        mid = (left + right) // 2
        if list[mid] == target:
            return mid
        elif list[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1
```