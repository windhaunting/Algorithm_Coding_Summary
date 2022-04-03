

# Binary search

Search a number in a sorted sequence in O(logN). There are some common cases for searching.

### (1) Search the element directly in the sorted sequence
Example code:

```
# search a target in sorted nums array
 def search(nums: List[int], target: int) -> int:
        
        # sorted bs
        
        l = 0
        r = len(nums) - 1
        
        while(l <= r):
            
            mid = l + (r - l)//2
            
            if nums[mid] == target:
                return mid
            elif nums[mid] < target:
                l = mid + 1
            else:
                r = mid - 1
        return -1
```

### (2) Find the left position to insert, which is similar to bisect_left in Python's bisect

```
# find the left most index of the target inserting into a sorted nums array

```

### (3) Find the right position to insert, which is similar to bisect_right in Python's bisect

```
# find the right most index of the target inserting into a sorted nums array

```




