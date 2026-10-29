## 1. Linear Search
It's the simplest searching algorithm which checks every element in the list until it finds the target value or reaches the end of the list.

```python
def linear_search(arr, target):
	for i in range(len(arr)):
		if arr[i] == target:
			return i
	return -1 
	# when element not found
```

```java
public static int linearSearch(int[] arr, target){
	for (int i = 0; i < arr.length; i++){
		if(arr[i] == target){
			return i;
		}
	}
	return -1;
}
```

## 2. Binary Search
**Binary Search** is an efficient searching algorithm that works on sorted arrays. It repeatedly divides the search interval in half, comparing the target value to the middle element and narrowing the interval accordingly.

```python
def binary_search(arr, target):
	left = 0
	right = len(arr)-1
	
	while left <= right:
		mid = left + right // 2
		if arr[mid] == target:
			return mid
		elif arr[mid] < target:
			left = mid + 1
		elif arr[mid] > target
			right = mid - 1
	return -1
```

```java
public static int binarySearch(int[] arr, int target){
	int left = 0;
	int right = arr.length -1;
	while(left <= right){
		int mid = left + (right-left)/2;
		if(arr[mid]==target){
			return mid;
		}
		else if(arr[mid] > target){
			right = mid - 1;
		}
		else{
			left = mid + 1;
		}
	}
	return -1;
}
```

![|700x474](../../../attachments/searching%20algorithms-1755521660026.webp)

### binary search using recursions

```python
def binary_search(arr:list, left, right, target) -> int:
	if left > right:
		return -1;
	else:
		mid = left + (right-left) //2
		if arr[mid] == target:
			return mid
		
```