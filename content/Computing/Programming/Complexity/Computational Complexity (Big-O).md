---
aliases:
  - Big-O
  - big O
  - Big o
  - big-o
---

Understanding the space and time complexity of an [[algorithm]] is critical to good software design. Fundamentally, we want to understand _how a program slows as data grows_. 

To analyse an algorithm's complexity, we focus on understanding:
- The number of steps required to complete a task (time complexity)
- The amount of data each step requires to complete the task (space complexity)

This analysis is referred to as $O(n)$ or Big-O, and is used to analyse how, for an each increase in the inputted data, how much more time does it take to run the algorithm (time complexity) and how much memory (space complexity) does the algorithm use. Big-O is a worst-case analysis, and there are other methods such as $\Theta(n)$ or Big-theta, which calculates the average complexity of an algorithm; or $\Omega(n)$ or Big-omega, which calculates the best-case complexity. Big-O is the most useful method of analysis in most cases. 

## Calculating Big-O

Lets take an example function in [[Python]]:
```python
def print_details(arr):
    # 1. Print the first element
    print(arr[0])                 # O(1) - Constant time
    
    # 2. Loop through the entire array
    for item in arr:              # Runs 'n' times -> O(n)
        print(item)
        
    # 3. Nested loops over the array
    for i in arr:                 # Runs 'n' times
        for j in arr:             # Runs 'n' times for EACH 'i'
            print(i, j)           # Total = n * n -> O(n^2)
```

Total operations are then:
$$
Operations = 1 + n + n^2
$$
Remembering that Big-O is a worst-case analysis, we can drop the least dominant terms: $1 + n$, knowing that our algorithm is dominated by the $n^2$ term. We can then state that this algorithm has complexity $O(n^2)$.

### Reducing Big-O
Though calculating Big-O gives concrete numbers, the more important component is the growth trend. You can see in the below images the different growth orders. 

![orders-of-growth|271](https://media2.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F38mm2a5585gapgsgln6n.jpg)

![growth image|534](https://msgprogramator.sk/wp-content/uploads/2024/05/graf-big-o-notacie-700-600.webp)

## Time Complexity


## Space Complexity


## Cheatsheet

![big-o cheatsheet|700](https://www.bigocheatsheet.com/img/big-o-cheat-sheet-poster.png)