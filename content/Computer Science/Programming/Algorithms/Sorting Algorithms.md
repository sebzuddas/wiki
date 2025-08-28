# Quicksort
```pseudocode
function quickSort(array, low, high) 
	if low < high 
		pivotIndex = partition(array, low, high) 
		quickSort(array, low, pivotIndex - 1) 
		quickSort(array, pivotIndex + 1, high) 

function partition(array, low, high) 
	pivot = array[high] 
	i = low - 1 
	for j = low to high - 1
		 if array[j] < pivot 
		 i = i + 1 
		 swap array[i] with array[j] 
	 swap array[i + 1] with array[high] 
	 return i + 1
	
```

# Binary Search
```pseudocode

function binarySearch(array, target) 
	low = 0 
	high = array.length - 1 
	
	while low <= high 
		mid = low + (high - low) / 2 
		if array[mid] == target 
			return mid 
		else if array[mid] < target 
			high = mid - 1 
		else low = mid + 1 
		
		return -1 // if the target is not found

```