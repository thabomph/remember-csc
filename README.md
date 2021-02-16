# Remember CompSci?

A quick recap of Computer Science fundamentals for those moments when you can't quite remember the basics and don't have enough time to look through multiple sources. This recap also includes some coding examples (in Java & JavaScript).

1. 📐 [Maths Recap](#maths-recap)
2. 📈 [Algorithmic Complexity](#algorithmic-complexity)
3. ​🛠️​ [Algorithms](#algorithms)
4. 🏗 [Data Structures](#data-structures)

⚠️ *This repository serves as a starting point for deeper research into each of the topics and is not an exhaustive overview of the topics discussed. Contributions are welcomed to address any mistakes or extend on the topics covered.*

---

## Maths Recap

### Exponents

An exponent refers to the number of times a number is multiplied by itself. It involving two numbers, the base $b$ and the exponent or power $n$. The operation is pronounced as "*$b$ raised to the power of $n$*", and is denoted as $b^n$.

**Example:** 2 raised to the power of 3:

```math
2^3 = 2 × 2 × 2 = 8
```

### Logarithms

A logarithm is the inverse function to an exponent. That means the logarithm of a given number $x$ is the exponent to which another fixed number, the base $b$, must be raised, in order to produce that number $x$. i.e. It answers the question *what do I need to raise the base $b$ by in order to get the value $x$?* i.e. $log_b(x) = z$ refers to the exponent in $b^z = x$

**Example:**

if

```math
2^3 = 8
```

then

```math
log_2(8) = 3
```

### Number Systems

#### Base-10 (Decimal)

* Decimal number system commonly used by people for counting.
* The unique digits used in this number system are defined by the set $N = \{0,1,2,3,4,5,6,7,8,9\}$

**Example:** The number 523 can be constructed and represented in base-10 as follows:

```matlab
= (5 × 10^2) + (2 × 10^1) + (3 × 10^0)
= (5 × 100) + (2 × 10) + (3 × 1)
= 523
```

#### Base-2 (Binary)

* Binary number system used as the primary system of computation by computers.
* The unique digits used in this number system are defined by the set $N = \{0,1\}$
* Defines values based on two digits 0 and 1, symbolizing the on and off states of electronic machines/systems
  
**Example:** The number 523 can be constructed and represented in base-2 as follows:

```matlab
= (1 × 2^9) + (0 × 2^8) + (0 × 2^7) + (0 × 2^6) + (0 × 2^5) + (0 × 2^4) + (1 × 2^3) + (0 × 2^2) + (1 × 2^1) + (1 × 2^0)
= (1 × 512) + (0 × 256) + (0 × 128) + (0 × 64) + (0 × 32) + (0 × 16) + (1 × 8) + (0 × 4) + (1 × 2) + (1 × 1)
= 1 0 0 0 0 0 1 0 1 1
```

##### Binary Arithmetic

Two binary numbers can be added similar to decimal number addition by carrying the sum numbers in related positions, however there are three rules that need to be followed when adding two binary numbers: 

1. Solve for Logical **XOR**

```matlab
  0 + 0 = 0
  1 + 0 = 1
```

2. Logical **AND** of two digits must be carried over with the current position resulting in 0.

```matlab
  1 + 1 = 10
```

3. Logical **AND** with 3 digits must be carried over with the current position resulting in 1.

```matlab
  1 + 1 + 1 = 11
```

**Example:** The sum of two binary numbers can be computed and represented as follows:

```matlab
  0101 0011
+ 0111 0110
  ---------
  1100 1001
```

**Example:** Binary sum of two binary numbers of the same size:

```java
/* Java */
static Integer[] binarySum(Integer[] a, Integer[] b) {
  LinkedList<Integer> sum = new LinkedList<Integer>();
  int maxSize = Math.max(a.length, b.length);
  boolean hasExtraBit = false;

  for(var i = maxSize - 1; i >= 0; i--) {
    if ((a[i] & b[i]) == 1) {
      if(hasExtraBit) {
        sum.addFirst(1);
      }else {
        hasExtraBit = true;
        sum.addFirst(0);
      }
    }else if ((a[i] ^ b[i]) == 1){
      if(hasExtraBit) {
        sum.addFirst(0);
      }else {
        sum.addFirst(1);
      }
    } else {
      if(hasExtraBit) {
        hasExtraBit = false;
        sum.addFirst(1);
      }else {
        sum.addFirst(0);
      }
    }
  }

  return sum.toArray(new Integer[sum.size()]);
}
```

### Probability

The likelihood that an event is to occur, or how likely it is that a proposition is true. i.e. The probability of an event is a number between 0 and 1, where, roughly speaking, 0 indicates impossibility of the event and 1 indicates certainty.

Probability can be defined as $x/n$ where $x$ is the number of successful events and $n$ is the total number of events

### Permutations

An arrangement of a set's members into a **sequence or linear order**, or if the set is already ordered, a rearrangement of its elements.  There are $n!$ permutations of length $n$ in a set of $n$ numbers, and $m × n!$ permutations of length $n$ in a set of $m$ numbers. Permutation operations have a [time complexity](#algorithmic-complexity) of $O(n!)$.

**Example:** Permutations of (1, 2, 3) of length 2:

```
(1, 2)
(2, 1)
(1, 3)
(3, 1)
(2, 3)
(3, 2)
```

### Combinations

An arrangement of a set's members into all possible arrangements, where the **order of the selection does not matter**. Combination operations have a time complexity of $$O(n)$$.

**Example:** Combinations of (1, 2, 3) of length 2:

```
(1, 2)
(1, 3)
(2, 3)
```

## Algorithmic Complexity

The time and space complexity of an algorithm are often evaluated the performance of algorithms

* BigO Notation helps us determine the performance of an algorithm (across OS and hardware) as input scales. i.e. it helps answer the question; ***"What will happen to an algorithm with an input of n, as n approaches infinity?"***
* It provides the upper bound on algorithms when input is assumed as the most difficult possible, and determines how many instructions an algorithm will perform as a function of an input of size *n*
* This is done based on the following rules:
  * Drop constants - i.e. $O(3n)$ becomes $O(n)$
  * Ignore non-dominant terms - i.e. $O(3n + n^2)$ becomes $O(n^2)$ and $O((f(n) + g(n))$ becomes $O(max(f(n)+ g(n))$
  * Chain of dominance is used: $O(1) < O(log(n)) < O(n) < O(n × log(n)) < O(n^2) < O(2^n) < O(n!)$
  * Use different variables for different inputs
  * Addition rule applies as $O(f(n)) + O(g(n)) = O(f(n) + g(n))$ - e.g. Sequence of For-Loops
  * Multiplication rule applies as $O(f(n)) × O(g(n)) = O(f(n) × g(n))$ - e.g. Nested For-Loop

### Time complexity

* The time complexity of an algorithm is the amount of time taken by the algorithm to complete its process as a function of its input length *n*. The different classes of time complexity are illustrated below:

![](https://victoria.dev/blog/a-coffee-break-introduction-to-time-complexity-of-algorithms/graph.png)
*Image source [blog post](https://victoria.dev/blog/a-coffee-break-introduction-to-time-complexity-of-algorithms/) by Victoria Drake*

#### Constant Time

Takes a constant time to run an algorithm regardless of the input size.

```math
O(1)
```

**Examples:**

* Mathematical operations
* Accessing Array element via index
* Accessing Map elements via key
* Pushing and popping on stack

#### Logarithmic Time

The running time of the algorithm grows in proportion to the logarithm of the input size $n$.

```math
O(log (n))
```

i.e. The operation is performed on an input source that is halved over time, therefore for an input source of size $n$, there can be at most $2^n$ or $O(log(n))$ operations (including the possibility of finding nothing).

**Examples:**

* Division of input
* Binary Search
* Balanced Search Tree

#### Linear Time

The run-time of an algorithm, increases as at the same pace as the size of the input $n$

```math
O(n)
```

**Examples:**

* Operation over an iteration of $n$ elements (e.g. Loop)
* Linear Search over n elements
* Tree traversals

#### Linearithmic Time

```math
O(n*log(n))
```

**Examples:**

* Merge Sort
* Quick Sort - optimal case

#### Quadratic Time

The run-time of an algorithm increases at the squared pace of the input data size $n$

```math
O(n^2)
```

**Examples:**

* Bubble Sort
* Insert Sort
* Select Sort
* Quick Sort - worst case
* Tree Sort
* Nested for loops e.g:

```js
/* JavaScript */
for (i in listA) {
  for (j in listA[i]) {
    ...
  }
}
```

#### Polynomial time

The run-time of an algorithm increases at the pace $n^k$ for the input data size *n*. Quadratic time algorithms are a type of polynomial-time algorithm where $k = 2$

```math
O(n^k)
```

#### Exponential Time

The runtime of the algorithm doubles with each addition to the input data set

```math
O(2^n)
```

**Example:**  Recursive function to determine the nth fibonacci number:

```js
/* JavaScript */
  function fib(n) {
    return (n <= 1) ? 1 : fib(n - 1) + fib(n  - 2)
  }
```

#### Factorial Time

The runtime of the algorithm is based on n factorial

```math
O(n!)
```

**Example:**

* Computing all possible combinations of values in an array of size $n$.

### Space Complexity

The space complexity of an algorithm is the amount of space (or memory) taken by the algorithm to run as a function of its input length, *n*. Space complexity includes **both auxiliary space and space used by the input**. i.e. it can be computed as $S = c + v$ where c is the space required by all constant inputs and v is the space required by all variable inputs.  The aim is to determine how much the space required with grow as the input size $n$ grows.

**Auxiliary space** is the temporary or extra space used by the algorithm while it is being executed. i.e. any change in memory needs while executing the function, in addition to the memory required for parameters of the function.

Many algorithms have inputs that can vary in size, e.g., an array. In such cases, the space complexity will depend on the size of the input and hence, cannot be less that *$O(n)$* for an input of size $n$. For fixed-size inputs, the complexity will be a constant *$O(1)$*.

Space complexity of an algorithm is commonly expressed using Big O notation.

**Examples:**

* A function with a constant time complexity such as

```js
/* JavaScript */
function greeting(name: string, age: number) {
  return `$Hello {name}, you are ${age} years old today`
}
```

has a space complexity of $O(8) + O(2) = O(1)$, by dropping constants on the JavaScript types:

| Type  | Bytes|
|-------|------|
|number | 8 b  |
|string | 2 b  |
|boolean| 1 b  |

This is because the amount of space the function needs will not change in addition to the the input.

* A function with a linear time complexity such as

```js
/* JavaScript */
function hasValue(list: string[], value: string) {
  for(let val of list) {
    return val === value
  }
}
```

has a space complexity of $O(n) + O(2) = O(n)$ by using the chain of dominance rule (i.e. $O(n) > O(1)$, and because the space required by the function itself does grow as the input grows (i.e. it only requires a temporary value for the current item in the list).

* A function with a quadratic time complexity such as

```js
/* JavaScript */
function hasValue(list: num[]) {
  for(let i = 0; i < list.length; i++) {
    for(let j = 0; j < list.length; j++) {
      console.log(i * j)
    }
  }
}
```

has a space complexity of $O(n^2)$ as the space required for the input of size *n* will grow exponentially at the rate $n×n$ when the function is being executed. The same applies to a function such as:

```js
/* JavaScript */
function hasValue(list: num[]) {
  for(let i = 0; i < list.length; i++) {
    for(let j = i; j < list.length; j++) {
      console.log(i * j)
    }
  }
}
```

even if *j* iterates from the point of *i*.

* A function with an exponential time complexity

```js
/* JavaScript */
function fib(int n) {
  return (n < 2) ? n : fib(n - 1) + fib(n - 2)
}
```

has a space complexity of $O(n)$, while the time complexity will be $O(2^n)$. This is because the recursive calls will be sequential and thus the addition rul will apply leading to an $O(n)$ space complexity.

Space complexity also depends on various things including the programming languages - e.g arrays are passed by reference in JS which would result in less space usage in some operations when compared to other languages.

---

### Searching Algorithms

#### Linear / Sequential Search

* Iterate through the entire list to search for the existence of the desired value
* Time complexity is $O(n)$ as the entire list would be iterated over in the worst case

**Example:**

```js
/* JavaScript */
  for(value in list) {
    if (vale = 'target') {
      return true
    }
  }
  return false
```

#### Binary Search

* Allows for searching for a target in a list of *n* elements by only looking at *n/2*  elements - ***assuming that the list is sorted***
* Involves repeatedly splitting the list in half and comparing the target to the item in the middle of the list to checking if the search should be done on the left or right sub-set of the list
* Time complexity of this is $O(log(n))$

**Example:** Binary Search with a loop

```js
/* JavaScript */
function binarySearchLoop(list, target) {
  let start = 0
  let end = list.length

  while(start <= end) {
    let mid = Math.floor((start + end) / 2)
    if(target === list[mid]) {
      return true
    } else if(target < list[mid]) {
      end = mid - 1      
    } else if(target > list[mid]) {
      start = mid + 1
    }
  }
  return false
}
```

**Example:** Recursive Binary Search

```js
/* JavaScript */
 /**
  * iLhs - initially set to index 0
  * iRhs - initially set to index list.length - 1
 */
 function binarySearchLoop(list, target, iLhs, iRhs) {
  let iMid = Math.floor((iRhs + iLhs) / 2)
  let mid = list[iMid]

  if(iRhs - iLhs < 1) {
    if(mid === target) {
      return `Found ${target} in ${list} at index ${iMid}`
    } else {
      return `Unable to find ${target} in ${list}`
    } 
  } else {    
    if(mid > target) {
      return binarySearchLoop(list, target, iLhs, iMid - 1)
    } else if(mid < target) {
      return binarySearchLoop(list,target, iMid + 1, iRhs)
    } 
  }
}
```

---

## Algorithms

### Sorting Algorithms

#### Bubble Sort

* Goes through a list (as many times as there are elements in the list) and keeps moving (bubbling) the largest value in the list (in that round) to the end of the list, by swapping adjacent pairs (next to each other) that are out of order.
* Called bubble sort because the biggest numbers keep bubbling up from the begging of the array.
* Has a time complexity of $O(n^2)$.
  
**Example:**
  
```js
/* JavaScript */
  function bubbleSort(list) {
    for(let i = 0; i < list.length; i++) {
      for(let j = 0; j < list.length - 1; j++) {
        if(list[j] > list[j + 1]) {
          let min = list[j]
          list[j] = list[j + 1]
          list[j + 1] = min
        }
      }
    }
  }
```

#### Insertion Sort

* Inserts each element in the array into it's correct position in a smaller subset of the array that has been sorted, which is initially only the first element but grows into the entire array.
* Has a time complexity of $O(n^2)$, but it sometimes outperform more advanced algorithms such as quick sort or merge sort on smaller lists as it could achieve a performance of $O(n)$ in the best case.

**Example:**

```js
/* JavaScript */
function insertionSort(list) {
  for(let i = 1; i < list.length; i++) {
    let j = i - 1
    const refValue = list[i]

    while(j >= 0 && list[j] > refValue) {
      list[j + 1] = list[j]
      j = j - 1
    }
    list[j + 1] = refValue
  }
}
```

#### Selection Sort

* Finds the minimum number in each iteration and puts it in the correct place
* Has a time complexity of $O(n^2)$
* Example:

```js
/* JavaScript */
function selectionSort(list) {
 for(let i = 0; i < list.length; i++) {
    let minIndex = i
    for(let j = i + 1; j < list.length; j++) {
      minIndex = (list[minIndex] < list[j]) ? minIndex : j
    }
    if(minIndex != i) {
      const minValue = list[minIndex]
      list[minIndex] = list[i]
      list[i] = minValue
    }
  } 
}  
```

#### Merge Sort

* Makes use of the Divide & Conquer to sort a list
  * **Divide** = Recursively dividing the input in halve, until the input is broken down into multiple lists of size 1, which are each sorted by default. This is an $O(log(n))$ operation.
  * **Merge** = Each pair of lists are scanned through linearly and combined in sorted order until you’re left with one final list, composing of the same elements from the input list but in sorted order. This is an $O(n)$ operation
* As a result the Merge sort has a $O(n*log(n))$ time complexity

**Example**
  
```js
/* JavaScript */
function msDivide(list) {
  const mid = Math.floor(list.length / 2)
  return (list.length < 2) ? list : msMerge(msDivide(list.slice(0, mid)), msDivide(list.slice(mid, list.length)))
}

const msMerge = (left, right) => {
  let iLeft = 0
  let iRight = 0
  let sortedList = []
  while(iLeft < left.length && iRight < right.length) {
    if(left[iLeft] < right[iRight]) {
      sortedList.push(left[iLeft])
      iLeft++
    }else {
      sortedList.push(right[iRight])
      iRight++
    }
  }
  return sortedList.concat(left.slice(iLeft)).concat(right.slice(iRight))
}
```

```java
/* Java */

static ArrayList<Integer> mergeSort(ArrayList<Integer> list) {
    int iMid = (int) Math.floor(list.size() / 2);
    ArrayList<Integer> left = new ArrayList<Integer>(list.subList(0, iMid));
    ArrayList<Integer> right = new ArrayList<Integer>(list.subList(iMid, list.size()));
    return list.size() < 2 ? list : mergeSortMerger(mergeSort(left), mergeSort(right));
  }

  static ArrayList<Integer> mergeSortMerger(ArrayList<Integer> left, ArrayList<Integer> right) {
    ArrayList<Integer> sorted = new ArrayList<Integer>();
    while((left.size() > 0) && (right.size()) > 0) {
      if(left.get(0) < right.get(0)) {
        sorted.add(left.remove(0));
      }else {
        sorted.add(right.remove(0));
      }
    }
    sorted.addAll(left);  
    sorted.addAll(right);
    return sorted;
  }
```

#### Quick Sort

* A pivot element (reference point) is chosen, and then the algorithm runs through the array using two pointers that move towards the pivot element, one at the left most position of the array and one at the right. Then, any values that are less than the pivot element are moved to the left side of the array and any values that are greater than or equal to the pivot element are moved to the right side of the array.
* After every iteration, the element that was used as the pivot item will end up in it’s final position in the sorted array.
* Typical time complexity of $O(n*log(n)$ - $O(n)$ for each sub-iteration of the list, $O(log (n))$ for each recursive call to sort the sub-list.
  * Average time complexity of the quicks sort algorithm is $O(n*log(n)$, however it could be $O(n^2)$ as a worst case - rare with median pivot selection. This can be avoided by using randomization to pick the pivot element which will minimize the odds of the pivot element having significantly more items that are greater or less than it throughout each iteration.
  * Quicksort is better to use with bigger collections as the time complexity is better in the long run.

**Example:**

```js
/* JavaScript */
function quickSort(list) {
  if(list.length < 2) return list

  let pivot = list[list.length - 1]
  let left = []
  let right = []

  for(let i = 0; i < list.length-1; i++) {
    if(list[i] < pivot) {
      left.push(list[i])
    }else {
      right.push(list[i])
    }
  }

  return [...quickSort(left), pivot, ...quickSort(right)]
}
```

---

## Data Structures

### Linked Lists

* Linear collection of data elements whose order is not given by their physical placement in memory. i.e. Elements (or Nodes) of a linked list are not stored sequentially in memory.
* This optimizes memory management as it allows for lists to be stored without a continuos block of memory (in cases where available memory positions are more dispersed).
* This also optimizes passing lists to functions as it eliminates the need for duplication of lists etc.
* The drawback of linked lists is that basic operations that are available through arrays in $O(1)$ time have to be built custom, which often involves them being $O(n)$. e.g. Indexing a linked list.

#### Singly Linked Lists

* Consists of Nodes, each with a pointer reference to the next Node in the list, adjacent to itself. The last node in the list points to Null
* Iteration through the list requires a knowledge of the head (or first) node in the list in order to traverse through the list

#### Doubly Linked Lists

* Consists of Nodes, each with a pointer reference to the next and previous Nodes in the list, adjacent to itself. The last node in the list points to Null
* Iteration through the list requires a knowledge of the Head (first), as well as the tail (last) nodes in the list in order to traverse through the list.

**Example:** Doubly Linked List

```js
/* JavaScript */
class ListNode {
  constructor(value) {
    this.value = value
    this.next = null
    this.previous = null
  }
}

class LinkedList {
  constructor() {
    this.head = null
    this.tail = null
  }
  ...
  push() {}
  pop() {}
  valueAt(index) {}
}
```

```java
  /* Java : java.util.LinkedList*/
  LinkedList<String> carLinkedList = new LinkedList<String>();
  carLinkedList.addFirst("Mazda");
  carLinkedList.getFirst();
  carLinkedList.removeLast();
```

---

### Hash Tables / Maps

A **hash function** is any function that can be used to map data of arbitrary size to a fixed-size values. The values returned by a hash function are called **hash values**, **hash codes**, **digests**, or simply **hashes**.

A **hash table** uses hash functions to store key value pairs (i.e. avoids the need of having two different arrays to manage keys and values)

* This works by using a hash function to associate a key with the memory position (or index) of a value. In so doing, hash functions avoid the need to store blocks of related data (...vs arrays) or having to iterate through different chains of data (...vs linked lists))
* This makes it easier and more efficient to check the existence of a value in a list (this is done in $O(1)$) in comparison to arrays and linked lists (who do this in $O(n)$)
* Two keys can have the same hash code (when a poor hash function is used) and thus be mapped to the same index. Various methods are in place for dealing with this (e.g. chaining using linked list)

**Example:**

```js
/* JavaScript */
const map = new Map
map.set('key', 'value')
map.get('key')
map.keys()
map.values()
map.clear('key')
map.size
console.log(map.has('key'))
```

```java
  HashMap<Integer, String> carsMap = new HashMap<Integer, String>();
    carsMap.put(102, "Volvo");
    carsMap.get(102);
    carsMap.size();
    carsMap.keySet(); // returns a set of keys
    carsMap.values(); // returns a set of values
    for(var carKey : carsMap.keySet()) {
      System.out.print(car);
    }
    for(String carValue : carsMap.values()) {
      System.out.print(car);
    }
    carsMap.remove(102);
    carsMap.clear();
```

---

### Graphs

Graphs are abstract data types that represent a collection of values and their relationship with one another

* The values are represented using **Nodes**
* The relationships between them are represented using **Edges** (connection maps)
* Graphs can be undirected or directed

#### Undirected Graphs

The order of the Nodes in the edge pairs don't matter

**Example:**

* Nodes: {A, B, C, D}
* Edges: {(A,B),(B,C),(B,D),(C,D)}

```
 .─.             
( A )            
 `─'             
  │              
(A,B)            
  │              
 .─.             
( B )──(B,C) ─┐  
 `─'          │  
  │          .─. 
  │         ( C )
(B,D)        `─' 
  │           │  
 .─.          │  
( D )──(C,D) ─┘  
 `─'             

```

#### Directed Graphs

The order of the Nodes in the edge pairs indicate the direction of the Graph

**Example:**

* Nodes: {A, B, C, D}
* Edges: {(A,B),(B,C),(B,D),(D,B),(C,D),(D,C)}

```
 .─.             
( A )            
 `─'             
  │              
(A,B)            
  │              
  ▼              
 .─.             
( B )──(B,C) ─┐  
 `─'          │  
  ▲           ▼
  │          .─. 
(B,D)       ( C )
  │          `─' 
  ▼           ▲  
 .─.    (C,D) │  
( D )◀──      ┘  
 `─'    (D,C)    
                 
```

#### Graph Search Algorithms

These determine how to search/traverse through a graph

##### Depth First Search (DFS)

* Involves traversing from the root node going down from left to right, deep into the graph from until hitting each leaf, then moving back up and down to the next branch
* Requires tracking all visited nodes in order to avoid an infinite loop where the graph is cyclical
* This has a runtime of $O(n)$

**Example:** Flow of DFS

```
         1  .─.           
    ┌──────( A )───────┐  
    │       `┬'        │  
    │        │         │  
    │        │         │  
    │        │         │  
 2 .─.    3 .─.     6 .─. 
  ( B )    ( C )     ( D )
   `─'      `┬'       `─' 
         ┌───┴───┐        
         │       │        
      4 .─.    5 ─.       
       ( E )   ( F )      
        `─'     `─'       
```

**Example:** Implementation of DFS

```js
/* JavaScript */
visited = new Map()
function dfs(visited, node) {
  if(!visited.has(node)) {
    print(node.value)
    visited.set(node, node.value)
    for(child in node.childred) {
      dfs(visited, child)
    }
  }
}
```

##### Breadth First Search (BFS)

* Involves traversing from the root node and evaluating each level in the graph from top to bottom first, then moving in the graph to cover all nodes
* Requires tracking all visited nodes in order to avoid an infinite loop where the graph is cyclical
* This has a runtime of $O(n)$
* Implementation makes use of a visit registry queue to control the order in which items are visited by first adding all children in each level before adding each of their own children into the queue

**Example:** Flow of BFS

```
         1  .─.           
    ┌──────( A )───────┐  
    │       `┬'        │  
    │        │         │  
    │        │         │  
    │        │         │  
 2 .─.    3 .─.     4 .─. 
  ( B )    ( C )     ( D )
   `─'      `┬'       `─' 
         ┌───┴───┐        
         │       │        
      5 .─.    6 ─.       
       ( E )   ( F )      
        `─'     `─'       
```

**Example:** Implementation of BFS

```js
/* JavaScript */
function bfs(root) {
  visited = new Map()
  toVisit = []

  toVisit.unshift(root)
  visited.set(root, root.value)

  while(toVisit !== []) {
    node = toVisit.shift() // dequeue
    print(node.val)

    for(child in node.children) {
      if(!visited.has(child)) {
        toVisit.unshift(child)
        visited.set(child, child.value)
      }
    }
  }
}
```

---

### Trees

Threes are types of Graphs with the following features:

* Trees have no cycles (i.e. Nodes do not reference themselves in a circular manner)
* Each node in a tree can have zero or more sub-nodes (child nodes), that it links to
* Nodes with no sub-nodes are called an **Edge node**
* Nodes with one or more sub-nodes are called a **Parent node**
* Trees have a single root node which is the parent node of all other nodes (directly or indirectly), and has no parent node itself

**Example:**

```
          .─.               
         ( A )              
          `┬'               
           │                
  ┌────────┼─────────┐      
  │        │         │      
 .─.      .─.       .─.     
( B )    ( C )     ( D )    
 `─'      `┬'       `┬'     
           │     ┌───┴───┐  
           │     │       │  
          .─.   .─.     .─. 
         ( E ) ( F )   ( G )
          `─'   `─'     `─' 
```

#### Tree Traversal Algorithms

* Determine the order in which nodes of a tree are visited - i.e. refers to when the node is visited.
* Makes use of a the depth first search algorithm
* The runtime for these is *$O(n)$* where there are n nodes in the tree

##### In-order Traversal

**Ordering of visits:**

1. Left Decedent
2. Root Node
3. Right Decedent

**Example:**

```js
/* JavaScript */
function inOrder(root) {
  inOrder(root.left)
  print(root.value)
  inOrder(root.right)
}
```

###### Pre-order Traversal

**Ordering of visits:**

1. Root Node
2. Left Decedent
3. Right Decedent

**Example:**

```js
/* JavaScript */
function preOrder(root) {
  print(root.value)
  preOrder(root.left)
  preOrder(root.right)
}
```

###### Post-order Traversal

**Ordering of visits:**

1. Left Decedent
2. Right Decedent
3. Root Node

**Example:**

```js
/* JavaScript */
function postOrder(root) {
  postOrder(root.left)
  postOrder(root.right)
  print(root.value)
}
```

#### Binary Trees

Trees where **each node can have at most 2 children**
The runtime for searching in a binary tree is $O(n)$

**Example:**

```
           .─.        
          ( A )       
           `─'        
            │         
      ┌─────┴──────┐  
      │            │  
     .─.          .─. 
    ( B )        ( C )
     `─'          `─' 
      │            │  
  ┌───┴───┐        │  
  │       │        │  
  │       │        │  
 .─.     .─.      .┴. 
( D )   ( E )    ( F )
 `─'     `─'      `─' 
```

```js
/* JavaScript */
// Node of a Binary Tree
class Node {
  constructor(value) {
    this.value = value
    this.left = null 
    this.right = null 
  }
}
```

#### Binary Search Trees (BSTs)

Binary trees with the following properties:

* All **left decedents of any node are less** than the node (and it's ancestors)
* All **right decedents of any node are greater** than the node (and it's ancestors)

The runtime of a Binary Search Tree is $O(n)$ in the worst case (e.g. where the tree only consists of values on one side) but can be $O(log(n))$ in a balanced tree - the algorithm for this is similar to that of a binary search algorithm i.e. look at the node and check if you should search for the value in the left or right depending on whether the value is greater or less than the node value

**Example:**

```
              .─.                 
             ( 5 )                
              `─'                 
               │                  
      ┌────────┴───────┐          
      │                │          
     .─.              .─.         
    ( 2 )            ( 8 )        
     `┬'              `┬'         
  ┌───┴───┐        ┌───┴───┐      
  │       │        │       │      
 .─.     .─.      .─.     .─.     
( 1 )   ( 4 )    ( 7 )   (10 )    
 `─'     `─'      `┬'     `┬'     
               ┌───┘   ┌───┴───┐  
               │       │       │  
              .─.     .─.     .─. 
             ( 6 )   ( 9 )   (12 )
              `─'     `─'     `─' 
```

#### Complete Binary Trees

Binary trees with the following properties:

* Every level of the binary tree (Except the edge/last level) has to be full / balanced
* The Edge/last level of the binary tree must be filled from left to right

The hight of a complete binary tree is logarithmic to the number of node in the tree i.e. $O(log(n))$ height

**Example:**

```
              .─.         
             ( 5 )        
              `─'         
               │          
      ┌────────┴───────┐  
      │                │  
     .─.              .─. 
    ( 2 )            ( 6 )
     `┬'              `─' 
  ┌───┴───┐               
  │       │               
 .─.     .─.              
( 1 )   ( 4 )             
 `─'     `─'              
```

#### Binary Heaps

* Binary trees where each node in the tree is smaller or larger than all of it's direct children
* In binary Heaps the root node is either the smallest or the largest value in the tree. This can thus be retrieved in *$O(1)$* time.
* **Inserting** an element at the correct place can be achieved in $O(log(n))$ time by adding a node at the edge of the tree and continuously swap the new node with it's parent based on whether or not it's larger than it's parent.
* **Deleting** the min or max can be achieved in $O(log(n))$ time by first swapping the root with the last element in the edge, then contiguously swapping the new root with it's children in order to put it in the correct positions

* **Min Heaps** - when a binary heap is smaller than all of it's direct decedents
* **Max Heap** - when the binary heap is larger than all of its direct decedents

---

## Tries

A tree-like data structure that implements the dictionary/map/key-value pairs. The word Trie is short for “re**trie**val tree”.

The nodes represent single characters that can be part of the keys. We can therefore start at the root of a trie and follow the edges through the nodes representing $s_0, s_1,...,s_{h1}$. When we reach the leaf, we will get the string $S = s_0,s_1,...,s_{h1}$. i.e. Tries are a tree whose leaf nodes are strings.

**Example:**

```
                      .─.                       
                    ┌─   ─┐                     
                  ┌─┘ `─' └─┐                   
                 ┌┘         └─┐                 
                 │            └┐                
                .─.            │.─.             
               ( C )           ( T )            
                `┬'             `┬'             
          ┌──────┴─────┐         └───┐          
          │            │             │          
         .─.          .─.           .─.         
        ( A )        ( O )         ( R )        
         `┬'          `┬'           `┬'         
      ┌───┘        ┌───┘         ┌───┴───┐      
      │            │             │       │      
     .─.          .─.           .─.     .─.     
    ( R )   *───── T )         ( I )   ( Y )    
     `┬┐          `┬'           `┬'     `┬'     
    ┌─┘└─┐         │         ┌───┴───┐   └───┐  
   ┌┘    └─.      .┴.        │       │       │  
   │  ┌─( D )    ( S )      .─.     .─.         
  * ┌─┘  `┬'      `┬'      ( E )   ( M )     *  
   ┌┘     │        │        `┬'     `┬'         
   │     .┴.             ┌───┴───┐   └───┐      
   *    ( S )      *     │       │       │      
         `┬'            .─.     .─.             
          │            ( D )   ( S )     *      
                        `┬'     `┬'             
          *              │       │              
                                                
                         *       *
```

```js
/* JavaScript */
class TrieNode {
  constructor(value) {
    this.value = value
    this.isComplete: false
    this.children = []
  }
}
```
