# Divide and Conquer

Divide and Conquer is an algorithm design paradigm that recursively breaks down a problem into two or more sub-problems of the same or related type, until these become simple enough to be solved directly.


Reference from: "https://www.geeksforgeeks.org/divide-and-conquer-algorithm-introduction/"

### Three parts of Divide and Conquer
This technique can be divided into the following three parts:

 1) Divide: This involves dividing the problem into smaller sub-problems.
 2) Conquer: Solve sub-problems by calling recursively until solved.
 3) Combine: Combine the sub-problems to get the final solution of the whole problem.


### Application of Divide and Conquer:

The following are some standard algorithms that follow Divide and Conquer algorithm.  

**Quicksort** is a sorting algorithm. The algorithm picks a pivot element and rearranges the array elements so that all elements smaller than the picked pivot element move to the left side of the pivot, and all greater elements move to the right side. Finally, the algorithm recursively sorts the subarrays on the left and right of the pivot element.

**Merge Sort** is also a sorting algorithm. The algorithm divides the array into two halves, recursively sorts them, and finally merges the two sorted halves.

**Closest Pair of Points** The problem is to find the closest pair of points in a set of points in the x-y plane. The problem can be solved in O(n^2) time by calculating the distances of every pair of points and comparing the distances to find the minimum. The Divide and Conquer algorithm solves the problem in O(N log N) time.

**Strassen’s Algorithm** is an efficient algorithm to multiply two matrices. A simple method to multiply two matrices needs 3 nested loops and is O(n^3). Strassen’s algorithm multiplies two matrices in O(n^2.8974) time.

**Cooley–Tukey Fast Fourier Transform (FFT)** algorithm is the most common algorithm for FFT. It is a divide and conquer algorithm which works in O(N log N) time.

**Karatsuba algorithm for fast multiplication** does the multiplication of two n-digit numbers in at most
3n^{log_{2}^{3}}\approx 3n^{1.585} single-digit multiplications in general (and exactly n^{\log_23}    when n is a power of 2). It is, therefore, faster than the classical algorithm, which requires n2 single-digit products. If n = 210 = 1024, in particular, the exact counts are 310 = 59, 049 and (210)2 = 1, 048, 576, respectively.

What does not qualifies as Divide and Conquer:

Binary Search is a searching algorithm. In each step, the algorithm compares the input element x with the value of the middle element in the array. If the values match, return the index of the middle. Otherwise, if x is less than the middle element, then the algorithm recurs for the left side of the middle element, else recurs for the right side of the middle element. Contrary to popular belief, this is not an example of Divide and Conquer because there is only one sub-problem in each step (Divide and conquer requires that there must be two or more sub-problems) and hence this is a case of Decrease and Conquer.


### Example of Divide and Conquer: Merge Sort:

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

Ref: https://www.geeksforgeeks.org/merge-sort/

```

### Example of Divide and Conquer: 53. Maximum Subarray
Given an integer array nums, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum. A subarray is a contiguous part of an array. The detailed description is [<span style="color:blue;"> here </span>](https://leetcode.com/problems/maximum-subarray/)


```

class Solution(object):
    def maxSubArray(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        
        
        #dp, maxRes[i] = max(maxRes[i-1] + nums[i], nums[i])
        #maxVal = max(maxRes[i], maxVal)
        
    
        
        # Except from Dynamic Programming solution
        #USE divide and conquer
        def maxCrossSum(nums, l, m, h):
            lsum = -1 * sys.maxsize
            summ = 0
            for i in range(m, l-1, -1):
                summ += nums[i]
                if summ > lsum:
                    lsum = summ
            summ = 0
            rsum = -1 * sys.maxsize
            for i in range(m+1, h+1):
                summ += nums[i]
                if (summ > rsum):
                    rsum = summ
            return lsum + rsum
                
        def maxSubArraySum(nums, l, h):
            if l == h:
                return nums[l]
            m = (l + h)/2
            return max(maxSubArraySum(nums, l, m), maxSubArraySum(nums, m+1, h), maxCrossSum(nums, l, m, h))
        
        
        if nums is None:
            return []
        l = 0
        h = len(nums)
        return maxSubArraySum(nums, l, h-1)

```