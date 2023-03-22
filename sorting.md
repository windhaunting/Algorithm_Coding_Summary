
There are many types of sorting algorithms. Here we show the important quickSort and merge sort related points.


## 1. Quicksort algorithm
It takes last element as pivot, places the pivot element at its correct position in sorted array, and places all smaller (smaller than pivot) to left of pivot and all greater elements to right of pivot 

Time complexity: O(NlogN)
Worst case time complexity: O(N^2) when ordered and pick the the most front/end as the pivot

```
#  quicksort quick select
def quickSelect(arr, l, r):  # partition
  
  pivot = arr[r]   # here we can use random pick a indx, then swap the indx with r 
  
  for j in range(l, r):   #  final goal:   l,   pivot,   r     3, 4, 4, 1 2, 9, 8 10, 5
                                                   #              i        j       i, j 
    if arr[j] <= pivot:
      # swap
      arr[l], arr[j] = arr[j], arr[l]
      l += 1

      #print("add: ", i+1)
  
  # swap with pivot
  arr[l], arr[r] = arr[r], arr[l]
  return l

# Quicksort in range 
def quickSort(arr, l, r):  # partition
  
    if len(arr) == 1:
      return arr
    
    if l < r:
      pivot = quickSelect(arr, l, r)
      
      quickSort(arr, l, pivot-1)
      quickSort(arr, pivot+1, r)
    
def QSort(arr):
  quickSort(arr, 0, len(arr)-1)
  
  return arr

arr = [3,7, 2, 5]  # [2, -7, -2, -2, 0]
QSort(arr)
print("QSort: ", arr) 

```




## 2. Merge sort algorithm
Merge sort is a Divide and Conquer algorithm. It repeatedly divides the array into two halves until we reach a stage where we try to perform MergeSort on a subarray of size 1.

```

# Python program for implementation of MergeSort
def mergeSort(arr):
    if len(arr) > 1:
 
         # Finding the mid of the array
        mid = len(arr)//2
 
        # Dividing the array elements
        L = arr[:mid]
 
        # into 2 halves
        R = arr[mid:]
 
        # Sorting the first half
        mergeSort(L)
 
        # Sorting the second half
        mergeSort(R)
 
        i = j = k = 0
 
        # Copy data to temp arrays L[] and R[]
        while i < len(L) and j < len(R):
            if L[i] < R[j]:
                arr[k] = L[i]
                i += 1
            else:
                arr[k] = R[j]
                j += 1
            k += 1
 
        # Checking if any element was left
        while i < len(L):
            arr[k] = L[i]
            i += 1
            k += 1
 
        while j < len(R):
            arr[k] = R[j]
            j += 1
            k += 1
 
# Code to print the list
 
def printList(arr):
    for i in range(len(arr)):
        print(arr[i], end=" ")
    print()
 
 
# Driver Code
if __name__ == '__main__':
    arr = [12, 11, 13, 5, 6, 7]
    print("Given array is", end="\n")
    printList(arr)
    mergeSort(arr)
    print("Sorted array is: ", end="\n")
    printList(arr)


```

## Sorting with custom sorting order

Write a function absSort(arr), that sorts the array according to the absolute values of the numbers in arr.
#If two numbers have the same absolute value, sort them according to sign, where the negative numbers come before the positive numbers.

```
# python custom compare sorting

from functools import cmp_to_key
def absSort(arr):
    def compare(a, b):
        if abs(a) < abs(b): return -1
        if abs(a) > abs(b): return 1
        if a < b: return -1
        if a > b: return 1
        return 0


    # Calling
    
    arr.sort(key= cmp_to_key(compare))

arr = [2, 5, 1]

arr=[2, -7, -2, -2, 0]
absSort(arr)
print('arr :', arr)

```

The second idea: 
or construct the tuple list and sort

```
arr_lst = [(e, abs(e), e//e) for e in arr]
arr_lst = arr_lst.sort(key = lambda ele: (ele[1], ele[2]))
arr_sorted = [ele[0] for ele in arr_lst]

```