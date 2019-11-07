## One line Code (Python)
### String Reverse
```python
s = "Python"
s[::-1]
> nohtyP
```
### Factorial
```python
fact = lambda x: 1 if x == 0 else x * fact(x-1)
fact(5)
> 120
```
### Palindrome
```python
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
