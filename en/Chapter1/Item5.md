##Item 5: Know How to Slice Sequences

Python includes syntax for slicing sequences into pieces. Slicing lets you access a subset of a sequence’s items with minimal effort. The simplest uses for slicing are the built-in types list, str, and bytes. Slicing can be extended to any Python class that implements the `__getitem__` and `__setitem__` special methods (see Item 28: “Inherit from collections.abc for Custom Container Types”).

The basic form of the slicing syntax is somelist[start:end], where start is inclusive and end is exclusive.

```python
a = [‘a’, ‘b’, ‘c’, ‘d’, ‘e’, ‘f’, ‘g’, ‘h’] 

print(‘First four:’, a[:4])
print(‘Last four: ‘, a[-4:]) 
print(‘Middle two:’, a[3:-3])

>>>

First four: [‘a’, ‘b’, ‘c’, ‘d’] 
Last four:	[‘e’, ‘f’, ‘g’, ‘h’] 
Middle two: [‘d’, ‘e’]
```

When slicing from the start of a list, you should leave out the zero index to reduce visual noise.

```python
assert a[:5] == a[0:5]
```

When slicing to the end of a list, you should leave out the final index because it’s redundant.

```python
assert a[5:] == a[5:len(a)]
```

Using negative numbers for slicing is helpful for doing offsets relative to the end of a list.
All of these forms of slicing would be clear to a new reader of your code. There are no surprises, and I encourage you to use these variations.

```python
a[:]	   # [‘a’, ‘b’, ‘c’, ‘d’, ‘e’, ‘f’, ‘g’, ‘h’] 
a[:5]	   # [‘a’, ‘b’, ‘c’, ‘d’, ‘e’]
a[:-1]	 # [‘a’, ‘b’, ‘c’, ‘d’, ‘e’, ‘f’, ‘g’]
a[4:]	   #  [‘e’, ‘f’, ‘g’, ‘h’]
a[-3:]	 #   [‘f’, ‘g’, ‘h’]
a[2:5]	 #   [‘c’, ‘d’, ‘e’]
a[2:-1]	 #   [‘c’, ‘d’, ‘e’, ‘f’, ‘g’] 
a[-3:-1] #   [‘f’, ‘g’]

```

Slicing deals properly with start and end indexes that are beyond the boundaries of the list. That makes it easy for your code to establish a maximum length to consider for an input sequence.

```python
first_twenty_items = a[:20] last_twenty_items = a[-20:]
```

In contrast, accessing the same index directly causes an exception. 

```python
a[20]

>>>

IndexError: list index out of range
```

**Note**

Beware that indexing a list by a negative variable is one of the few situations in which you can get surprising results from slicing. For example, the expression somelist[-n:] will work fine when n is greater than one (e.g.,
somelist[-3:]). However, when n is zero, the expression somelist[-0:] will result in a copy of the original list.


The result of slicing a list is a whole new list. References to the objects from the original list are maintained. Modifying the result of slicing won’t affect the original list.

```python
b = a[4:] 
print(‘Before:	’, b) 
b[1] = 99 
print(‘After:	’, b) 
print(‘No change:’, a)

>>>
Before: [‘e’, ‘f’, ‘g’, ‘h’]
After : [‘e’, 99, ‘g’, ‘h’]
No change: [‘a’, ‘b’, ‘c’, ‘d’, ‘e’, ‘f’, ‘g’, ‘h’]

```

When used in assignments, slices will replace the specified range in the original list. Unlike tuple assignments (like a, b = c[:2]), the length of slice assignments don’t need to be the same. The values before and after the assigned slice will be preserved. The list will grow or shrink to accommodate the new values.

```python
print(‘Before ‘, a) 
a[2:7] = [99, 22, 14] 
print(‘After	’, a) 
>>>
Before	[‘a’, ‘b’, ‘c’, ‘d’, ‘e’, ‘f’, ‘g’, ‘h’] 
After	[‘a’, ‘b’, 99, 22, 14, ‘h’]
```

If you leave out both the start and the end indexes when slicing, you’ll end up with a copy of the original list.

```python
b = a[:]
assert b == a and b is not a
```

If you assign a slice with no start or end indexes, you’ll replace its entire contents with a copy of what’s referenced (instead of allocating a new list).

```python
b = a 
print(‘Before’, a)
a[:] = [101, 102, 103] 
assert a is b      # Still the same list object
print(‘After ‘, a)  # Now has different contents


>>>

Before [‘a’, ‘b’, 99, 22, 14, ‘h’] After	[101, 102, 103]
```

##Things to Remember

* Avoid being verbose: Don’t supply 0 for the start index or the length of the sequence for the end index.

* Slicing is forgiving of start or end indexes that are out of bounds, making it easy to express slices on the front or back boundaries of a sequence (like a[:20] or
a[-20:]).

* Assigning to a list slice will replace that range in the original sequence with what’s referenced even if their lengths are different.
