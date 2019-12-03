
# Javascript Helpers
```python
var arr = [
	    { id: 1, username: 'fred' }, 
	    { id: 2, username: 'bill' }, 
            { id: 3, username: 'ted' },
	    { id: 5, username: 'john' }, 
	    { id: 6, username: 'doe' }, 
            { id: 7, username: 'philip' }            
	];
```
### Filter (filter records)
```javascript
var result = arr.filter(
   function(item){
      return item.id == 1
   }
)
or
var result = arr.filter(
    item => item.id === 1
)
result = [ { id: 1, username: 'pred' } ]
```
### Some (if object in array)
```javascript
var findById = 2

var found = arr.some(
   function(item){
      return item.id == 1
   }
)
or
var found = arr.some(
   item => item.id === findById
)
or 
const checkUserId= item => item.id === findById;
var found = arr.some(checkUserId)
```

```
```
### 

