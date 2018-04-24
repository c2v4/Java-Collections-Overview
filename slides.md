
class: center, middle, inverse

# Java: Collections Overview

### by [Mateusz Duda](https://github.com/c2v4)

---
name: default
layout: true
---
class: center, middle, inverse

# Collection

---
count: false
class: center, middle, inverse

# ~~Collection~~
# Object

---
template: default
layout: true

### Object

---
```java
public class Object {
    
    public boolean equals(Object obj) {
        return (this == obj);
    }
    
    public native int hashCode();

    ...
    
}
```
---
## Equals
Indicates whether some other object is "equal to" this one.

**Compares values for equality**. When an Object is a composite it is good to compare all non cached properties when checking equality.

When not overridden it does reference comparison.
--

### Properties:

* Reflexive: `x.equals(x)`
* Symmetric: `x.equals(y)` == `y.equals(x)`
* Transitive: `x.equals(y)` and `y.equals(z)` then `x.equals(z)`
* Consistent: multiple invocations of `x.equals(y)` consistently return `true` or consistently return `false`, provided no information used in `equals` comparisons on the objects is modified
* For any non-null reference value `x`, `x.equals(null)` should return `false`

---
### HashCode

Calculates and returns a 'summary' of an object represented by `int` value.

* Whenever it is invoked on the same object more than once during an execution of a Java application, the `hashCode` method must consistently return the same integer, provided no information used in `equals` comparisons on the object is modified
* **If two objects are equal according to the `equals(Object)` method, then calling the `hashCode` method on each of the two objects must produce the same integer result**

This is typically implemented by converting the internal address of the object into an integer, but this implementation technique is not required by the _Java_ programming language.

---
class: center, middle, inverse

# Collection

---
template: default
layout: true

### Collection

---

# Oxford Dictionary

--
>A group of things or people.

--

>‘a rambling collection of houses’

---

# Java

    public interface Collection<E> extends Iterable<E> {
        int size();
        boolean isEmpty();
        boolean contains(Object o);
        boolean add(E e);
        boolean remove(Object o);
        ...
    }

???

- add returns true if the collection was modified
- remove returns true if element existed
- notice there's no way to get an element

Iterable - Implementing this interface allows an object to be the target of the "for-each loop" statement.
```java

public interface Iterable<T> {
    Iterator<T> iterator();
    ...
}
```
```java
public interface Iterator<E> {
        boolean hasNext();
        E next();
        default void remove() {
            throw new UnsupportedOperationException("remove");
        }
        ...
}
```
---
# Collections:
* Array
* List
* Set
* Queue
* Map
---
count: false
# Collections:
* ~~Array~~
* List
* Set
* Queue
* ~~Map~~
---
layout: false
class: center, middle, inverse

# Array

---
### Array

# Array


An array is a container **object** that holds a fixed number of values of a single type. The length of an array is established when the array is created. After creation, its length is fixed.

--


##<center>Why we need Collections API when we have such a great structure?</center>

???
- Additional requirements
- Fixed length
- Consistent API
---
layout: false
class: center, middle, inverse

# List

---
template: default
layout: true

### List

---
## Properties:
* Ordered

The _List_ interface provides four methods for positional (indexed) access to list elements.  _Lists_ (like Java arrays) are zero-based.
```java    
    public interface List<E> extends Collection<E> {
*       E get(int index);
        E set(int index, E element);
        void add(int index, E element);
*       E remove(int index);
        int indexOf(Object o);
        int lastIndexOf(Object o);
        List<E> subList(int fromIndex, int toIndex);
    ...
    }
```
* Allows duplicates
* Usually allows storing _null_

???
> It is not inconceivable that someone might wish to implement a list that prohibits duplicates, by throwing runtime exceptions when the user attempts to insert them, but we expect this usage to be rare.

* add shifts elements that are past provided index
* keep in mind that 'ordered' is not same as 'sorted'
---
## Types:
* ArrayList
* LinkedList
* Vector
* ...
--

>Vector is a holdover from early days of Java it works just as ArrayList but its methods are synchronized

---
## ArrayList

* Good all-around list implementation 
* Backed by an array
* Automatically resizes when needed
* Ideal for use-cases with frequent access
* Not performing well when Objects are inserted and removed frequently

---
## LinkedList

* Doubly Linked List implementation in java
* Implements Deque interface
* Consists of nodes that are pointing at next and previous element
* Pretty slow when it comes to access
* Ideal use-case: inserting, deleting, rearranging elements

---
## Computer Science behind Lists



|           | get   | add   | contains  | next  | remove(0) | iterator.remove   |
|-----------|-------|-------|-----------|-------|-----------|-------------------|
|ArrayList  | O(1)  | O(1)  | O(n)      | O(1)  | O(n)      | O(n)              |
|LinkedList | O(n)  | O(1)  | O(n)      | O(1)  | O(1)      | O(1)              |

---
layout: false
class: center, middle, inverse

# Set

---
template: default
layout: true

### Set

---
## Properties

* Models mathematical Set abstraction
* Does not allow duplicates

More formally, sets contain no pair of elements `e1` and `e2` such that `e1.equals(e2)`
* At most one _null_ element

???
* Does not allow null due to the fact that there is a need to call a method to determine `equals` property

---
## Types:
* HashSet
* LinkedHashSet
* EnumSet
* *SortedSet*
  * *NavigableSet*
      * TreeSet
* ...

---
## HashSet
* Unsorted, unordered
* Backed up by HashMap
* Basic operations take constant time
* Iteration order is not guaranteed
* Allows one _null_

???

|                      |add      |contains |next     |notes                  |
|----------------------|---------|---------|---------|-----------------------|
|HashSet               |O(1)     |O(1)     |O(h/n)   |h is the table capacity|
---
## LinkedHashSet
* Ordered version of HashSet
* Maintains doubly linked list across all elements
* Iteration order is consistent
* Backed by LinkedHashMap

???
LinkedHashSet         O(1)     O(1)     O(1) 

---
## EnumSet

* Specialized implementation to use with `Enum` types
* All elements must come from one enum type
* Extremely compact and efficient
* Enum Sets are represented by bit vectors
* Good typesafe alternative to `int` based bit flags

```java
public abstract class EnumSet<E extends Enum<E>> extends AbstractSet<E>
    implements Cloneable, java.io.Serializable
{
    ...
}
```

---
## SortedSet

* Abstraction
* Provides ordering
* All elements must implement `Comparable` or be accepted by provided `Comparator`
* Iteration takes place in ascending order

```java
public interface SortedSet<E> extends Set<E> {
        Comparator<? super E> comparator();
        E first();
        E last();
        SortedSet<E> subSet(E fromElement, E toElement);
        SortedSet<E> headSet(E toElement);
        SortedSet<E> tailSet(E fromElement);
}
```

???
* Subsets returned are backed by original one ao all operations will be visible in parent and vice versa

---
## NavigableSet
* Abstraction
* Extended SortedSet with navigation methods for finding closest elements

```java
public interface NavigableSet<E> extends SortedSet<E> {
    E lower(E e);
    E floor(E e);
    E ceiling(E e);
    E higher(E e);
    E pollFirst();
    E pollLast();
    ...
}
```

???
* Methods `lower`, `floor`, `ceiling`, and `higher` return elements respectively less than, less than or equal, greater than or equal, and greater than a given element, returning `null` if there is no such element.
* Poll methods are removing elements

---
## TreeSet
* Guaranteed to be in ascending order
* Does not accept `null`
* Uses Red-Black Tree
* Backed by TreeMap
* As mentioned in SortedSet elements must be `Comparable` or accepting given `Comparator`
* Ordering must be consistent with `equals`

---
## Computer Science

|                       | add      | contains |
|-----------------------|----------|----------|
| HashSet               | O(1)     | O(1)     |
| LinkedHashSet         | O(1)     | O(1)     |
| EnumSet               | O(1)     | O(1)     |
| TreeSet               | O(log n) | O(log n) |

---
layout: false
class: center, middle, inverse

# Queue

---
template: default
layout: true

### Queue

---
## Properties
* Design to hold elements prior to processing
* Provide additional insertion, extraction and inspection operations 
* Typically order elements in FIFO manner

```java
public interface Queue<E> extends Collection<E> {
*   boolean add(E e);
    boolean offer(E e);
*   E remove();
    E poll();
*   E element();
    E peek();
}
```


???
* Each of those operations has 2 forms: returning null when a negative case or throwing NoSuchElementException
* Highlighted method throw exception
* Add can only return true or throw IllegalStateException if cannot be added due to Queue restriction

---
## Types
* LinkedList
* PriorityQueue
  * Based on Priority Heap
  * Orders elements according to their `Comparable` implementation or by provided `Comparator`
* BlockingQueue
  * Additionally provides methods to wait for the queue to be empty to insert an element and a method to wait for the queue to have an element to be retrieved
* Deque
  * Supports inserting and removing elements from both ends
  * Name comes from Double Ended Queue 
---
layout: false
class: center, middle, inverse

# Map

---
template: default
layout: true

### Map

---
## Properties
* Maps keys to values
* Does not extend `Collection` but provides similar methods
```java
public interface Map<K,V> {
    int size();
    boolean isEmpty();
    boolean containsKey(Object key);
    boolean containsValue(Object value);
    V get(Object key);
    V put(K key, V value);
    V remove(Object key);
    Set<K> keySet();
    Collection<V> values();
    Set<Map.Entry<K, V>> entrySet();
    ...
}
```
???
* Methods that return key/value/entry collection are views and operations will be reflected

---
## Types
* HashMap
* LinkedHashMap
* TreeMap
* HashTable
* ConcurrentHashMap

---
## HashMap
* Implementation based on Hash Table
* Uses `hashCode` an `equals` method to determine key existence and to find value
* Consists of _Buckets_, `hashCode` determines it
* When a collision happens, keys are checked with equals
* Values are stored in LinkedList in _Buckets_ 
* In Java 8, when stored elements are comparable and there are 8 elements stored inside a bucket it gets transformed to Tree, when the number gets to 6 it is transformed to LinkedList back again
* Allows one `null` as a key and multiple `null` values
* Does not guarantee order

???
* Check `containsKey` in order to determine whether the value is there, get can return `null` not only when the value is not present, but also when the value is `null` itself

---
## LinkedHashMap
* Similar to HashMap
* Guarantees order
* Slower than HashMap for inserting, removing entries
* Faster iteration than HashMap
---
## TreeMap
* Implemented based on Red-Black Tree
* Keys are sorted
* Ordering based on `Comparable` implementation or provided `Comparator`
---
## HashTable
* 'Old HashMap' implementation
* Synchronized methods
---
## ConcurrentHashMap
* Similar to HashMap
* Threadsafe without synchronizing the whole map
* Reads happen fast, while write is done with a lock
* Uses multiple locks to ensure performance
---
## More Computer Science!
|                      |get      |containsKey| insert   | remove |
|----------------------|---------|-----------|----------|--------|
|HashMap               |O(1)     |O(1)       |O(1)      |O(1)    |
|LinkedHashMap         |O(1)     |O(1)       |O(1)      |O(1)    | 
|TreeMap               |O(log n) |O(log n)   |O(log n)  |O(log n)| 
|ConcurrentHashMap     |O(1)     |O(1)       |O(1)      |O(1)    |
---
## Map is not a Collection
If map is supposed to be a collection, it should be a collection of what?

--
```java
public interface Map<K,V> extends Collection<Map.Entry<K,V>>
```

--
```java
public boolean add(Map.Entry<K,V>);
```

--
Checking `hashCode` and `equals` of `Map.Entry<K,V>` should ignore Value?

--
--
```java
public boolean contains(Map.Entry<K,V>);
```
Should it check for key only or whole `Entry`?
???
* If we want to make Map a Collection we can do it but we come into pitfalls
---
layout: false
class: center, middle, inverse

# Recap
---
template: default
layout: true

### Rule of thumb

---

* Single Value: *Collection*
* Key-Value Pair: *Map*

---
count: false

* Single Value: *Collection*
  * Contains duplicates: *List*
     * ArrayList **ArrayList**
  * Does not contain duplicates
* Key-Value Pair: *Map*

---
count: false

* Single Value: *Collection*
  * Contains duplicates: *List*
     * ArrayList **ArrayList**
  * Does not contain duplicates *Set*
     * Ordered
     * Unordered: **HashSet**
* Key-Value Pair: *Map*
  * Ordered
  * Unordered: **HashMap**

---
count: false

* Single Value: *Collection*
  * Contains duplicates: *List*
     * ArrayList **ArrayList**
  * Does not contain duplicates *Set*
     * Ordered
         * Insertion **LinkedHashSet**
         * Sorted **TreeSet**
     * Unordered: **HashSet**
* Key-Value Pair: *Map*
      * Ordered
          * Insertion **LinkedHashMap**
          * Sorted **TreeMap**
      * Unordered: **HashMap**

---
layout: false
# Resources
* [Java Generics and Collections](http://shop.oreilly.com/product/9780596527754.do)
* [Collections Framework Overview](https://docs.oracle.com/javase/8/docs/technotes/guides/collections/overview.html)
* [Big-O Cheat Sheet](http://bigocheatsheet.com/)
* [JDK Javadocs](https://docs.oracle.com/javase/8/docs/api/)
* [Java Collections API Design FAQ](https://docs.oracle.com/javase/8/docs/technotes/guides/collections/designfaq.html#a14)
---
layout: false
class: center, middle, inverse

# That's all folks!
---
count:false
layout: false
class: center, middle, inverse

# ~~That's all folks!~~
# Almost!

---
count:false
>This was by design. We feel that mappings are not collections and collections are not mappings. Thus, it makes little sense for Map to extend the Collection interface (or vice versa).
 
>If a Map is a Collection, what are the elements? The only reasonable answer is "Key-value pairs", but this provides a very limited (and not particularly useful) Map abstraction. You can't ask what value a given key maps to, nor can you delete the entry for a given key without knowing what value it maps to.
 
>Collection could be made to extend Map, but this raises the question: what are the keys? There's no really satisfactory answer, and forcing one leads to an unnatural interface.
 
>Maps can be viewed as Collections (of keys, values, or pairs), and this fact is reflected in the three "Collection view operations" on Maps (keySet, entrySet, and values). While it is, in principle, possible to view a List as a Map mapping indices to elements, this has the nasty property that deleting an element from the List changes the Key associated with every element before the deleted element. That's why we don't have a map view operation on Lists. - [Java Collections API Design FAQ](https://docs.oracle.com/javase/8/docs/technotes/guides/collections/designfaq.html#a14)
