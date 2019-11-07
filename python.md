

## Methods
### Map
Example 1
```python
def addition(n):
    return n+n
numbers = (1,2,3,4,5,6,7,8)
result = map(addition, numbers)
```
Example 2
```python
f = lambda x: x + x
numbers = (1, 2, 3, 4) 
result = map(f, numbers) 
```
### Compile
```python
srcCode = 'x = 10\ny = 20\nmul = x * y\nprint("mul =", mul)'
execCode = compile(srcCode, 'mulstring', 'exec')
try:
    exec(execCode) 
except Exception as e:
    print(e)  
 ```
 ### String format
 ```python
 my_string = "{} is an {} of {}"
 my_string.format("this", "exmple", "String format")
 ```
## Class
### Property
Example 1 :  Using property() method
```python
class Name: 
    def __init__(self, name): 
        self._name = name 
    # Getter    
    def getName(self): 
        print('Getting value') 
        return self._name 
          
    # Setter 
    def setName(self, name): 
        print('Setting value to ' + name) 
        self._name = name 
          
    # Deleter
    def delName(self): 
        print('Deleting name') 
        del self._name 
      
    name = property(getName, setName, delName, ) 
   ```
   Example 2 : Using @property decorator
```python
   class Name: 
    def __init__(self, name): 
        self._name = name 
    @property    
    def name(self): 
        print('Getting value') 
        return self._name 
          
    # setting the values 
    @name.setter 
    def name(self, name): 
        print('Setting value to ' + name) 
        self._name = name 
          
    # deleting the values 
    @name.deleter
    def name(self): 
        print('Deleting name') 
        del self._name 
        ```
## Sorting
Quick sort in 5 Line

from heapq import merge
arr = [9,4,6,5,7,8,1,2,3,-1]
def mergeSort(A):
    return A if len(A)==1 else merge(mergeSort(A[:int(len(A)/2)]),mergeSort(A[int(len(A)/2):]))    
print(list(mergeSort(arr)))
