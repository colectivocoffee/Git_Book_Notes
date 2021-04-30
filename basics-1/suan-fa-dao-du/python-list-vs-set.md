# \[Python\] List vs Set

| **List** | **Set** |
| :--- | :--- |
| 1. Can contain duplicate elements | 1. Elements must be unique |
| 2. Order of elements is important | 2. Order of elements is not important, but it depends on the implementation |
| 3. Elements are accessed using index | 3. Elements themselves are indices |
| 4. The interface used to implement the list is System.Collections.IList | 4. The interface used to implement the set is System.Collections.ISet |
| 5. The list is implemented as a static list \(using array\) and dynamic list \(linked list\) | 5. Sets are implemented as hashset \(hashtable\) and sorted set \(red-black tree-based\) |

### Set Methods

| Method | Description |
| :--- | :--- |
| [add\(\)](https://www.w3schools.com/python/ref_set_add.asp) | \*\*Adds an element to the set |
| [copy\(\)](https://www.w3schools.com/python/ref_set_copy.asp) | \*\*Returns a copy of the set |
| [clear\(\)](https://www.w3schools.com/python/ref_set_clear.asp) | \*Removes all the elements from the set |
| [pop\(\)](https://www.w3schools.com/python/ref_set_pop.asp) | \*\*Removes an element from the set |
| [discard\(\)](https://www.w3schools.com/python/ref_set_discard.asp) | \*\*Remove the specified item |
| [remove\(\)](https://www.w3schools.com/python/ref_set_remove.asp) | \*\*Removes the specified element |
| [difference\(\)](https://www.w3schools.com/python/ref_set_difference.asp) | \*\*Returns a set containing the difference between two or more sets |
| [difference\_update\(\)](https://www.w3schools.com/python/ref_set_difference_update.asp) | Removes the items in this set that are also included in another, specified set |
| [intersection\(\)](https://www.w3schools.com/python/ref_set_intersection.asp) | Returns a set, that is the intersection of two other sets |
| [intersection\_update\(\)](https://www.w3schools.com/python/ref_set_intersection_update.asp) | Removes the items in this set that are not present in other, specified set\(s\) |
| [isdisjoint\(\)](https://www.w3schools.com/python/ref_set_isdisjoint.asp) | Returns whether two sets have a intersection or not |
| [issubset\(\)](https://www.w3schools.com/python/ref_set_issubset.asp) | Returns whether another set contains this set or not |
| [issuperset\(\)](https://www.w3schools.com/python/ref_set_issuperset.asp) | Returns whether this set contains another set or not |
| [symmetric\_difference\(\)](https://www.w3schools.com/python/ref_set_symmetric_difference.asp) | Returns a set with the symmetric differences of two sets |
| [symmetric\_difference\_update\(\)](https://www.w3schools.com/python/ref_set_symmetric_difference_update.asp) | inserts the symmetric differences from this set and another |
| [union\(\)](https://www.w3schools.com/python/ref_set_union.asp) | Return a set containing the union of sets |
| [update\(\)](https://www.w3schools.com/python/ref_set_update.asp) | Update the set with another set, or any other iterable |

