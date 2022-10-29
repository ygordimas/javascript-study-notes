# javascript-interview-prep
Study notes on various JavaScript topics.

# Arrays

## Describing an Array from the JavaScript perspective
An array is an instance of the Array object that inherits methods from Array.prototype and can store a mix of different data types. They are resizable and you can access their values through indexes, which are represented by positive integers that start at 0. 

## What are array literals?
An array literal is a notation for directly declaring arrays by initializing them with one or more declarations of its elements' values surrounded by square brackets.
```
const arrayLiteral = ['element1', 'element2', 'element3']
```
It is not much different from declaring a new array through the *new Array()* method, from which you are able to pass a parameter that will alocate a specific amount of memory space in the newly created array, but with no actual values attached to it.
```
const newArray = new Array(3)
```
From this example, *newArray.length* will return a value of 3, while newArray[0] will return *undefined*.
It is important to note that the newly created array does NOT hold 3 items with a value of type undefined.
*['undefined', 'undefined', 'undefined'] == newArray* would return false.

In terms of performance, it might be important to keep in mind that:
> By being able to predefine the size of the stack, the new Array() method might be advantageous, performance-wise, in cases where you want to prevent stack overflow from happening (when the size of the array exceeds the size of the stack). ([source](https://stackoverflow.com/questions/931872/what-s-the-difference-between-array-and-while-declaring-a-javascript-ar/931875#931875))

## What is the data type of an array in JavaScript?
Arrays have the object data type. 

## What is an array-like object?
In JavaScript, an object consists of a mapping between keys and values that can be later accessed or reference through said keys (either by square brackets notation or dot notation). An array-like object has an indexed key/value combination of elements and a length property with a value holding the amount of the previous key/value combinations. In this sense it might look and behave like an array, but it doesn’t hold any of the methods inherited from Array.prototype that come with an array declaration.

The ‘arguments’ parameter of a function is an array-like object.
It is possible to directly convert an array-like into an array.

```
//using the spread operator
[...arrayLike]
```

```
//using Array.from()
const newArray = Array.from(arrayLike)
```

## Which array operations can be done on an Array-Like object?
Iterations with *for* loops and numbered indices.

## Indexes
Array elements can be accessed by their indexes. 
Positive indexes will go from 0 until the value of it’s length minus 1. Negative indexes start from the last element of the array with a -1 value, adding -1 to each index (second to last will have an index of -2, and the first element of the array will have a negative index with the value of it’s length).

## How to find the size of an array?
Through the method found on Array.prototype.length 

## How to create an array of a given size?
```
const newArray = new Array(size)
```

## Is the array in JavaScript static-sized or dynamic-sized?
Dynamic-sized. JavaScript arrays can be modified at runtime and after being initially declared.

## What happens when "length" is reduced?
When the length property is reduced, every property with an index value smaller than the new length will be deleted.

## What will happen when "length" is increased?
The value of the length property will increase and the end of the array will have nothing but spots with no stored value.

## Sparse vs. Dense arrays
Dense arrays are the most common array type. All of it’s elements are sequential and the length property is an accurate indicator of the amount of all of it’s stored values.

A sparse array’s set of defined elements is not garanteed to start at index 0 or to be sequential to each other. It’s length property will always be greater than the amount of defined elements that it has due to one or more gaps between some of it’s elements. The length of a sparse array is not a precise indicator of the amount of values that are stored.

## How to detect a sparse array?
You can detect a sparse array by checking if the length of the array is greater than the amount of elements.
To check for sparse arrays, verify that there are no falsy values:

```
let array = [,,,,,,]

//declare a condition for running on truthy values only
for (let i = 0; i < array.length; i++) {
	if (array[i]) {}
}

//declare a sum value of truthy elements of an array and compare to it's length
function checkSparse(arr) {
let count = 0;

	for (let i = 0; i < arr.length; i++) {
		return count++;
	}

return count !== arr.length ? true : false
```

## How is a sparse array formed?
There are a few different ways a sparse array can come into fruition:

```
//using the Array object
let array = new Array(3);
```
```
//inserting a key/value at a certain index
//length will be 1000
array[999] = 0;
```
```
//using the delete operator
//will result in [, 6, 9, 12, 15] and a length of 5
let array = [3, 6, 9, 12, 15]
delete array[0]
```
```
//initializing an array with holes in it
let falsyArray = [,,,,,,]
let mistypedArray = [3, 6,, 12]
```

## Describe the looping behavior of a sparse array.
Array methods like .map and .forEach will skip sparse values so the only way to properly loop through a sparse array is with a for loop.

## Convert a sparse array into a dense array.
You can convert by filling all of the vacant spots with an ‘undefined’ value through a for loop or use either a .map or .forEach method that will skip all of vacancies and return only the stored values.

## Which methods of Array.prototype accepts negative indexes?
- Array.prototype.slice()
- Array.prototype.at()
- Array.prototype.copyWithin()
- Array.prototype.fill()
- Array.prototype.slice()
- Array.prototype.splice()

## How can you do a deep-level flat operation on a given array?
```
const arr = [3, 6, [9, 12], 15, [18, 21, [24, 27, [30]], 33], 36, 39]

const flatten = (arr) = {

	someNewArray = arr.reduce((acc, item) => {
  
		//checks if item is array
		//if it is, do a recursive function call on the array item
		if(Array.isArray(item)) {
    
			acc = acc.concat(flatten(item))
      
		} else {
    
		//if item is not an array, push it directly to the array
		//acc.push(item) is an option here
		acc = [...acc, item]
    
		}
    
		return acc;
	}, [])
  
	return someNewArray;
}
```

## How to get an array of names from a given array of user objects?
```
const users = [{
    id: 1,
    name: 'Peter',
    isActive: true
  },
  {
    id: 2,
    name: 'Paul',
    isActive: true
  },
  {
    id: 3,
    name: 'Mary',
    isActive: false
  }
]

const getArrayOfNames = (arr) => {
  const arrayOfNames = arr.reduce((acc, item) => {
    acc = [...acc, item.name]
    return acc
  }, [])

  return arrayOfNames
}
```

## How to get an array of names of ONLY ACTIVE USERS from a given array of user objects?
```
const users = [{
    id: 1,
    name: 'Peter',
    isActive: true
  },
  {
    id: 2,
    name: 'Paul',
    isActive: true
  },
  {
    id: 3,
    name: 'Mary',
    isActive: false
  }
]

const getArrayOfNames = (arr) => {
  const arrayOfNames = arr.reduce((acc, item) => {
    return item.isActive ? acc = [...acc, item.name] : acc
  }, [])

  return arrayOfNames
}
```

## How to get an array of names of ONLY ACTIVE USERS from a given array of user objects, sorted by age (descending)?
```
const users = [{
    id: 1,
    name: 'Peter',
    isActive: true,
    age: 18,
  },
  {
    id: 2,
    name: 'Paul',
    isActive: true,
    age: 20,
  },
  {
    id: 3,
    name: 'Mary',
    isActive: false,
    age: 22,
  }
]

const getArrayOfNames = (arr) => {
  const arrayOfNames = arr.sort((a, b) => b.age - a.age).reduce((acc, item) => {
    return item.isActive ? acc = [...acc, item.name] : acc
  }, [])

  return arrayOfNames
}
```
