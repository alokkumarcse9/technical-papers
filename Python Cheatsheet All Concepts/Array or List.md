# Python Cheatsheet — Array/List Methods

> In Python, we normally use a **list** when we talk about an array.

```python
arr = [10, 20, 30, 40, 50]
```

---

# 1. Creating an Array/List

```python
arr = [10, 20, 30, 40]
```

Empty list:

```python
arr = []
```

Using `list()`:

```python
arr = list()
```

---

# 2. Accessing Elements

### Using Index

```python
arr = [10, 20, 30, 40]

print(arr[0])    # 10
print(arr[2])    # 30
```

### Negative Index

```python
print(arr[-1])   # 40
print(arr[-2])   # 30
```

| Index | Value |
| ----- | ----- |
| `0`   | 10    |
| `1`   | 20    |
| `2`   | 30    |
| `3`   | 40    |
| `-1`  | 40    |
| `-2`  | 30    |

---

# 3. `append()`

Adds **one element at the end**.

```python
arr = [10, 20, 30]

arr.append(40)

print(arr)
```

Output:

```text
[10, 20, 30, 40]
```

### Remember

```python
arr.append(x)
```

➡️ Add `x` at the end.

---

# 4. `extend()`

Adds **multiple elements** to the end.

```python
arr = [10, 20]

arr.extend([30, 40, 50])

print(arr)
```

Output:

```text
[10, 20, 30, 40, 50]
```

### `append()` vs `extend()`

```python
arr = [1, 2]

arr.append([3, 4])

print(arr)
```

Output:

```text
[1, 2, [3, 4]]
```

But:

```python
arr = [1, 2]

arr.extend([3, 4])

print(arr)
```

Output:

```text
[1, 2, 3, 4]
```

### Easy rule

```text
append()  → one item
extend()  → multiple items
```

---

# 5. `insert()`

Adds an element at a specific index.

```python
arr = [10, 20, 40]

arr.insert(2, 30)

print(arr)
```

Output:

```text
[10, 20, 30, 40]
```

Syntax:

```python
arr.insert(index, value)
```

Example:

```python
arr.insert(0, 5)
```

Adds `5` at the beginning.

---

# 6. `remove()`

Removes the **first occurrence** of a value.

```python
arr = [10, 20, 30, 20]

arr.remove(20)

print(arr)
```

Output:

```text
[10, 30, 20]
```

Only the first `20` is removed.

### Important

`remove()` works with **value**, not index.

```python
arr.remove(20)
```

---

# 7. `pop()`

Removes and returns an element.

### Remove last element

```python
arr = [10, 20, 30]

x = arr.pop()

print(x)      # 30
print(arr)    # [10, 20]
```

### Remove using index

```python
arr = [10, 20, 30]

x = arr.pop(1)

print(x)      # 20
print(arr)    # [10, 30]
```

### Remember

```text
pop()        → last element
pop(index)   → element at index
```

---

# 8. `clear()`

Removes all elements.

```python
arr = [10, 20, 30]

arr.clear()

print(arr)
```

Output:

```text
[]
```

---

# 9. `index()`

Returns the index of the first occurrence.

```python
arr = [10, 20, 30, 40]

print(arr.index(30))
```

Output:

```text
2
```

Example:

```python
arr = [10, 20, 30, 20]

print(arr.index(20))
```

Output:

```text
1
```

---

# 10. `count()`

Counts how many times a value occurs.

```python
arr = [10, 20, 20, 30, 20]

print(arr.count(20))
```

Output:

```text
3
```

Very useful in DSA.

---

# 11. `sort()`

Sorts the list in ascending order.

```python
arr = [40, 10, 30, 20]

arr.sort()

print(arr)
```

Output:

```text
[10, 20, 30, 40]
```

### Descending order

```python
arr.sort(reverse=True)

print(arr)
```

Output:

```text
[40, 30, 20, 10]
```

### Important

`sort()` modifies the original list.

---

# 12. `reverse()`

Reverses the list.

```python
arr = [10, 20, 30, 40]

arr.reverse()

print(arr)
```

Output:

```text
[40, 30, 20, 10]
```

---

# 13. `copy()`

Creates a copy of a list.

```python
arr = [10, 20, 30]

new_arr = arr.copy()

print(new_arr)
```

Output:

```text
[10, 20, 30]
```

---

# 14. `len()`

Returns the number of elements.

```python
arr = [10, 20, 30, 40]

print(len(arr))
```

Output:

```text
4
```

> `len()` is a built-in function, not a list method.

---

# 15. `min()`

Returns the smallest value.

```python
arr = [40, 10, 30, 20]

print(min(arr))
```

Output:

```text
10
```

> `min()` is a built-in function.

---

# 16. `max()`

Returns the largest value.

```python
arr = [40, 10, 30, 20]

print(max(arr))
```

Output:

```text
40
```

> `max()` is a built-in function.

---

# 17. `sum()`

Returns the sum of all elements.

```python
arr = [10, 20, 30]

print(sum(arr))
```

Output:

```text
60
```

> `sum()` is a built-in function.

---

# 18. Checking if Element Exists

Use `in`.

```python
arr = [10, 20, 30]

print(20 in arr)
```

Output:

```text
True
```

Example:

```python
print(50 in arr)
```

Output:

```text
False
```

### DSA Pattern

```python
if target in arr:
    print("Found")
```

---

# 19. Slicing

Slicing is extremely important in DSA.

```python
arr = [10, 20, 30, 40, 50]
```

### First 3 elements

```python
print(arr[0:3])
```

Output:

```text
[10, 20, 30]
```

### From index 2

```python
print(arr[2:])
```

Output:

```text
[30, 40, 50]
```

### Until index 3

```python
print(arr[:3])
```

Output:

```text
[10, 20, 30]
```

### Entire list

```python
print(arr[:])
```

### Reverse

```python
print(arr[::-1])
```

Output:

```text
[50, 40, 30, 20, 10]
```

---

# 20. Traversing an Array

## Normal `for` loop

Use this when you need the **index**.

```python
arr = [10, 20, 30, 40]

for i in range(len(arr)):
    print(arr[i])
```

Output:

```text
10
20
30
40
```

### DSA pattern

```python
for i in range(len(arr)):
    # use i
    # use arr[i]
```

---

# 21. Enhanced `for` Loop

Use this when you only need the **values**.

```python
arr = [10, 20, 30, 40]

for value in arr:
    print(value)
```

### DSA pattern

```python
for value in arr:
    # work directly with value
```

### Easy Rule

```text
Need index?  → for i in range(len(arr))
Need value?  → for value in arr
```

---

# 22. `enumerate()`

Useful when you need **both index and value**.

```python
arr = [10, 20, 30, 40]

for i, value in enumerate(arr):
    print(i, value)
```

Output:

```text
0 10
1 20
2 30
3 40
```

### DSA Pattern

```python
for i, value in enumerate(arr):
    # use both i and value
```

---

# 23. `zip()`

Used to traverse two arrays together.

```python
arr1 = [10, 20, 30]
arr2 = [1, 2, 3]

for a, b in zip(arr1, arr2):
    print(a, b)
```

Output:

```text
10 1
20 2
30 3
```

Useful when comparing two arrays.

---

# 24. List Comprehension

A short way to create a new list.

### Normal approach

```python
arr = [1, 2, 3, 4]

squares = []

for x in arr:
    squares.append(x * x)

print(squares)
```

### List comprehension

```python
arr = [1, 2, 3, 4]

squares = [x * x for x in arr]

print(squares)
```

Output:

```text
[1, 4, 9, 16]
```

---

# 25. List Comprehension with Condition

Get only even numbers:

```python
arr = [1, 2, 3, 4, 5, 6]

even = [x for x in arr if x % 2 == 0]

print(even)
```

Output:

```text
[2, 4, 6]
```

---

# 26. Important Methods Quick Table

| Method               | Purpose                 |
| -------------------- | ----------------------- |
| `append(x)`          | Add one element         |
| `extend(iterable)`   | Add multiple elements   |
| `insert(i, x)`       | Add at index            |
| `remove(x)`          | Remove first occurrence |
| `pop()`              | Remove last element     |
| `pop(i)`             | Remove element at index |
| `clear()`            | Remove everything       |
| `index(x)`           | Find index              |
| `count(x)`           | Count occurrences       |
| `sort()`             | Sort ascending          |
| `sort(reverse=True)` | Sort descending         |
| `reverse()`          | Reverse list            |
| `copy()`             | Copy list               |

---

# 27. Important Built-in Functions

| Function         | Purpose                  |
| ---------------- | ------------------------ |
| `len(arr)`       | Length                   |
| `min(arr)`       | Minimum                  |
| `max(arr)`       | Maximum                  |
| `sum(arr)`       | Sum                      |
| `sorted(arr)`    | Returns sorted copy      |
| `reversed(arr)`  | Returns reverse iterator |
| `enumerate(arr)` | Index + value            |
| `zip(a, b)`      | Traverse arrays together |

---

# 28. `sort()` vs `sorted()`

This is very important.

### `sort()`

Changes the original list.

```python
arr = [30, 10, 20]

arr.sort()

print(arr)
```

Output:

```text
[10, 20, 30]
```

### `sorted()`

Creates and returns a new sorted list.

```python
arr = [30, 10, 20]

new_arr = sorted(arr)

print(arr)
print(new_arr)
```

Output:

```text
[30, 10, 20]
[10, 20, 30]
```

### Remember

```text
sort()    → modifies original list
sorted()  → returns new sorted list
```

---

# 29. `reverse()` vs `[::-1]`

### `reverse()`

Modifies the original list.

```python
arr = [1, 2, 3]

arr.reverse()

print(arr)
```

### `[::-1]`

Creates a reversed copy.

```python
arr = [1, 2, 3]

new_arr = arr[::-1]

print(arr)
print(new_arr)
```

Output:

```text
[1, 2, 3]
[3, 2, 1]
```

---

# 30. Most Important for DSA

You should become very comfortable with these:

```python
arr.append(x)
arr.pop()
arr.pop(i)
arr.remove(x)

arr.sort()
arr.reverse()

arr.index(x)
arr.count(x)

len(arr)
sum(arr)
min(arr)
max(arr)

x in arr

arr[i]
arr[start:end]
arr[::-1]

for x in arr:
for i in range(len(arr)):
for i, x in enumerate(arr):
```

---

# 31. DSA Decision Guide

When you see a problem, ask:

### "Do I need the index?"

```python
for i in range(len(arr)):
```

Use this when you need:

```python
arr[i]
```

Example:

```python
for i in range(len(arr)):
    if arr[i] > 10:
        print(i)
```

---

### "Do I only need the value?"

```python
for x in arr:
```

Example:

```python
for x in arr:
    if x > 10:
        print(x)
```

---

### "Do I need index + value?"

```python
for i, x in enumerate(arr):
```

Example:

```python
for i, x in enumerate(arr):
    if x == target:
        print(i)
```

---

### "Do I need to modify the array?"

Think about:

```python
append()
remove()
pop()
insert()
sort()
reverse()
```

---

### "Do I need a new array?"

Think about:

```python
arr[start:end]
arr[::-1]
sorted(arr)
[x * 2 for x in arr]
```

---

# 32. Mini Example — Common DSA Pattern

### Find the largest element

```python
arr = [10, 50, 20, 80, 30]

largest = arr[0]

for x in arr:
    if x > largest:
        largest = x

print(largest)
```

Output:

```text
80
```

---

### Find the index of largest element

```python
arr = [10, 50, 20, 80, 30]

largest = arr[0]
largest_index = 0

for i in range(len(arr)):
    if arr[i] > largest:
        largest = arr[i]
        largest_index = i

print(largest)
print(largest_index)
```

Output:

```text
80
3
```

---

# ⭐ Final Memory Trick

```text
ADD
    append()
    extend()
    insert()

REMOVE
    remove()
    pop()
    clear()

SEARCH
    index()
    count()
    in

ORDER
    sort()
    sorted()
    reverse()
    [::-1]

INFO
    len()
    min()
    max()
    sum()

TRAVERSAL
    for x in arr
    for i in range(len(arr))
    for i, x in enumerate(arr)
    zip()

CREATE NEW LIST
    slicing
    list comprehension
```

## The 5 Most Important to Learn First

If you are currently learning DSA, master these first:

```python
append()
pop()
sort()
len()
enumerate()
```

Then learn:

```python
remove()
insert()
index()
count()
extend()
```
