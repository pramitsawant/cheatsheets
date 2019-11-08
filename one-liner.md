## One line Code (Python)
### String Reverse
```python
s = "Python"
s[::-1]
> nohtyP
```
### Two nums
```python 
num[::2]
``` 
### Factorial
```python
fact = lambda x: 1 if x == 0 else x * fact(x-1)
fact(5)
> 120
```
### Fibonacci
```python
lambda x: x if x<=1 else fib(x-1) + fib(x-2)
```
### Palindrome
```python
phrase = 10101
str(phrase).find(str(phrase)[::-1])

isPalindrome = lambda n:True if str(n).lower()[::-1] == str(n).lower() else False

testcases = ["2002",2002,"MADAM","MALAYALAM","505050"]
result = [ isPalindrome(testcase) for testcase in testcases]
result
> [True, True, True, True, False]
```
### Merge Sort
```python
from heapq import merge
arr = [9,4,6,5,7,8,1,2,3,-1]
mergeSort = lambda A : A if len(A)==1 else merge(mergeSort(A[:int(len(A)/2)]),mergeSort(A[int(len(A)/2):]))
list(mergeSort(arr))
> [-1, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

### Quick Sort
```python 
lambda L: [] if L==[] else qsort([x for x in L[1:] if x< L[0]]) + L[0:1] + qsort([x for x in L[1:] if x>=L[0]])
```
### Binary Search
```python
def binarySearch(arr,x,l,r):
    if r >= l:
        mid = int(l + (r - l)/2)
        return mid if arr[mid] == x else binarySearch(arr,x, l, mid-1) if arr[mid] > x else binarySearch(arr,x, mid + 1, r) if arr[mid] < x else -1
    else:
        return -1
```


https://wiki.python.org/moin/Powerful%20Python%20One-Liners
