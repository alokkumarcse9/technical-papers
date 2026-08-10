# Python Cheatsheet — String Methods

> In Python, a string is a sequence of characters.

```python
s = "hello"
```

---

# 1. Creating a String

```python
s = "hello"
```

Single quotes:

```python
s = 'hello'
```

Double quotes:

```python
s = "hello"
```

Empty string:

```python
s = ""
```

---

# 2. Accessing Characters

Strings use **indexing**, just like arrays/lists.

```python
s = "hello"

print(s[0])    # h
print(s[1])    # e
print(s[4])    # o
```

### Negative indexing

```python
print(s[-1])   # o
print(s[-2])   # l
```

| Index | Character |
| ----- | --------- |
| `0`   | `h`       |
| `1`   | `e`       |
| `2`   | `l`       |
| `3`   | `l`       |
| `4`   | `o`       |
| `-1`  | `o`       |
| `-2`  | `l`       |

---

# 3. `len()`

Returns the length of a string.

```python
s = "hello"

print(len(s))
```

Output:

```text
5
```

> `len()` is a built-in function, not a string method.

---

# 4. `lower()`

Converts the string to lowercase.

```python
s = "Hello World"

print(s.lower())
```

Output:

```text
hello world
```

### DSA use

Useful when the problem says:

> Ignore uppercase/lowercase.

```python
s = s.lower()
```

---

# 5. `upper()`

Converts the string to uppercase.

```python
s = "hello"

print(s.upper())
```

Output:

```text
HELLO
```

---

# 6. `capitalize()`

Makes the first character uppercase.

```python
s = "hello world"

print(s.capitalize())
```

Output:

```text
Hello world
```

---

# 7. `title()`

Capitalizes the first character of every word.

```python
s = "hello world"

print(s.title())
```

Output:

```text
Hello World
```

---

# 8. `swapcase()`

Converts uppercase to lowercase and lowercase to uppercase.

```python
s = "Hello World"

print(s.swapcase())
```

Output:

```text
hELLO wORLD
```

---

# 9. `strip()`

Removes spaces from the beginning and end.

```python
s = "   hello   "

print(s.strip())
```

Output:

```text
hello
```

### Important

It does **not** remove spaces in the middle.

```python
s = "   hello world   "

print(s.strip())
```

Output:

```text
hello world
```

---

# 10. `lstrip()`

Removes spaces from the **left side**.

```python
s = "   hello"

print(s.lstrip())
```

Output:

```text
hello
```

---

# 11. `rstrip()`

Removes spaces from the **right side**.

```python
s = "hello   "

print(s.rstrip())
```

Output:

```text
hello
```

---

# 12. `replace()`

Replaces one substring with another.

```python
s = "hello world"

print(s.replace("world", "python"))
```

Output:

```text
hello python
```

### Replace characters

```python
s = "banana"

print(s.replace("a", "o"))
```

Output:

```text
bonono
```

### Replace only a specific number of times

```python
s = "banana"

print(s.replace("a", "o", 2))
```

Output:

```text
bonona
```

Syntax:

```python
s.replace(old, new, count)
```

---

# 13. `find()`

Returns the index of the first occurrence.

```python
s = "hello"

print(s.find("l"))
```

Output:

```text
2
```

If not found:

```python
print(s.find("x"))
```

Output:

```text
-1
```

### DSA Pattern

```python
index = s.find(target)

if index != -1:
    print("Found")
```

---

# 14. `rfind()`

Finds the **last occurrence**.

```python
s = "banana"

print(s.rfind("a"))
```

Output:

```text
5
```

---

# 15. `index()`

Returns the index of the first occurrence.

```python
s = "hello"

print(s.index("l"))
```

Output:

```text
2
```

### `find()` vs `index()`

If the substring doesn't exist:

```python
s.find("x")
```

Returns:

```text
-1
```

But:

```python
s.index("x")
```

Raises an error.

### DSA Tip

Usually prefer:

```python
s.find(target)
```

when you want a safe "not found" result.

---

# 16. `count()`

Counts occurrences of a substring.

```python
s = "banana"

print(s.count("a"))
```

Output:

```text
3
```

Example:

```python
s = "hello"

print(s.count("l"))
```

Output:

```text
2
```

### DSA Use

Very useful for:

* Frequency problems
* Character counting
* Duplicate detection

---

# 17. `startswith()`

Checks whether a string starts with a particular value.

```python
s = "hello world"

print(s.startswith("hello"))
```

Output:

```text
True
```

Example:

```python
print(s.startswith("world"))
```

Output:

```text
False
```

---

# 18. `endswith()`

Checks whether a string ends with a particular value.

```python
s = "hello.py"

print(s.endswith(".py"))
```

Output:

```text
True
```

---

# 19. `split()`

Splits a string into a **list**.

```python
s = "hello world python"

words = s.split()

print(words)
```

Output:

```text
['hello', 'world', 'python']
```

### Split by a specific character

```python
s = "apple,banana,mango"

fruits = s.split(",")

print(fruits)
```

Output:

```text
['apple', 'banana', 'mango']
```

### Very important DSA pattern

```python
s = "10 20 30 40"

arr = s.split()

print(arr)
```

Output:

```text
['10', '20', '30', '40']
```

If you need integers:

```python
arr = list(map(int, s.split()))
```

Output:

```text
[10, 20, 30, 40]
```

---

# 20. `join()`

Combines elements into a string.

```python
words = ["hello", "world"]

result = " ".join(words)

print(result)
```

Output:

```text
hello world
```

### Join without spaces

```python
words = ["a", "b", "c"]

result = "".join(words)

print(result)
```

Output:

```text
abc
```

### Join with comma

```python
words = ["apple", "banana", "mango"]

result = ",".join(words)

print(result)
```

Output:

```text
apple,banana,mango
```

### Remember

```text
split() → String → List
join()  → List → String
```

---

# 21. `isalpha()`

Checks whether all characters are alphabets.

```python
s = "hello"

print(s.isalpha())
```

Output:

```text
True
```

But:

```python
s = "hello123"

print(s.isalpha())
```

Output:

```text
False
```

---

# 22. `isdigit()`

Checks whether all characters are digits.

```python
s = "12345"

print(s.isdigit())
```

Output:

```text
True
```

But:

```python
s = "123a"

print(s.isdigit())
```

Output:

```text
False
```

### DSA use

```python
if ch.isdigit():
    print("Digit")
```

---

# 23. `isalnum()`

Checks whether all characters are letters or numbers.

```python
s = "hello123"

print(s.isalnum())
```

Output:

```text
True
```

But:

```python
s = "hello 123"

print(s.isalnum())
```

Output:

```text
False
```

Because of the space.

---

# 24. `isspace()`

Checks whether all characters are whitespace.

```python
s = "   "

print(s.isspace())
```

Output:

```text
True
```

Example:

```python
s = "hello"

print(s.isspace())
```

Output:

```text
False
```

---

# 25. `islower()`

Checks whether all alphabetic characters are lowercase.

```python
s = "hello"

print(s.islower())
```

Output:

```text
True
```

---

# 26. `isupper()`

Checks whether all alphabetic characters are uppercase.

```python
s = "HELLO"

print(s.isupper())
```

Output:

```text
True
```

---

# 27. `isspace()`, `isdigit()`, `isalpha()` Quick Table

| Method      | Checks             |
| ----------- | ------------------ |
| `isalpha()` | Only alphabets     |
| `isdigit()` | Only digits        |
| `isalnum()` | Alphabets + digits |
| `isspace()` | Only whitespace    |
| `islower()` | Lowercase          |
| `isupper()` | Uppercase          |

---

# 28. String Slicing

Strings support slicing.

```python
s = "abcdef"
```

### First 3 characters

```python
print(s[0:3])
```

Output:

```text
abc
```

### From index 2

```python
print(s[2:])
```

Output:

```text
cdef
```

### Until index 4

```python
print(s[:4])
```

Output:

```text
abcd
```

### Reverse

```python
print(s[::-1])
```

Output:

```text
fedcba
```

---

# 29. String Concatenation

Joining strings using `+`.

```python
first = "Hello"
second = "World"

result = first + " " + second

print(result)
```

Output:

```text
Hello World
```

---

# 30. String Repetition

Use `*`.

```python
s = "abc"

print(s * 3)
```

Output:

```text
abcabcabc
```

---

# 31. Checking Character/Substring

Use `in`.

```python
s = "hello world"

print("hello" in s)
```

Output:

```text
True
```

Example:

```python
print("python" in s)
```

Output:

```text
False
```

### DSA Pattern

```python
if target in s:
    print("Found")
```

---

# 32. Traversing a String

## Enhanced `for` loop

Use when you only need characters.

```python
s = "hello"

for ch in s:
    print(ch)
```

Output:

```text
h
e
l
l
o
```

---

# 33. Normal `for` Loop

Use when you need the index.

```python
s = "hello"

for i in range(len(s)):
    print(i, s[i])
```

Output:

```text
0 h
1 e
2 l
3 l
4 o
```

### DSA Rule

```text
Need character only?
→ for ch in s

Need index?
→ for i in range(len(s))
```

---

# 34. `enumerate()`

Use when you need **index + character**.

```python
s = "hello"

for i, ch in enumerate(s):
    print(i, ch)
```

Output:

```text
0 h
1 e
2 l
3 l
4 o
```

### DSA Pattern

```python
for i, ch in enumerate(s):
    # use i
    # use ch
```

---

# 35. Strings Are Immutable

This is VERY important.

You cannot directly modify a character.

❌ This doesn't work:

```python
s = "hello"

s[0] = "H"
```

Python gives an error.

Instead, create a new string:

```python
s = "hello"

s = "H" + s[1:]

print(s)
```

Output:

```text
Hello
```

---

# 36. Converting String to List

Use `list()`.

```python
s = "hello"

arr = list(s)

print(arr)
```

Output:

```text
['h', 'e', 'l', 'l', 'o']
```

This is useful when you need to modify individual characters.

---

# 37. Converting List to String

Use `join()`.

```python
arr = ['h', 'e', 'l', 'l', 'o']

s = "".join(arr)

print(s)
```

Output:

```text
hello
```

### Important Pattern

```text
String
  ↓
list()
  ↓
List
  ↓
join()
  ↓
String
```

---

# 38. `ord()`

Returns the Unicode/ASCII code of a character.

```python
print(ord("a"))
```

Output:

```text
97
```

```python
print(ord("A"))
```

Output:

```text
65
```

### DSA Use

Character arithmetic:

```python
difference = ord("d") - ord("a")

print(difference)
```

Output:

```text
3
```

---

# 39. `chr()`

Converts a Unicode/ASCII number into a character.

```python
print(chr(97))
```

Output:

```text
a
```

```python
print(chr(65))
```

Output:

```text
A
```

### `ord()` + `chr()`

```python
ch = "a"

next_char = chr(ord(ch) + 1)

print(next_char)
```

Output:

```text
b
```

---

# 40. `str()`

Converts a value into a string.

```python
num = 123

s = str(num)

print(s)
print(type(s))
```

Output:

```text
123
<class 'str'>
```

Very useful when building strings.

---

# 41. `int()`

Converts a numeric string into an integer.

```python
s = "123"

num = int(s)

print(num)
print(type(num))
```

Output:

```text
123
<class 'int'>
```

### DSA Example

```python
s = "12345"

total = 0

for ch in s:
    total += int(ch)

print(total)
```

Output:

```text
15
```

---

# 42. `split()` + `map()` + `int()`

Very common in DSA input handling.

```python
s = "10 20 30 40"

arr = list(map(int, s.split()))

print(arr)
```

Output:

```text
[10, 20, 30, 40]
```

Understand the flow:

```text
"10 20 30 40"
       ↓
    split()
       ↓
["10", "20", "30", "40"]
       ↓
     map(int)
       ↓
[10, 20, 30, 40]
```

---

# 43. Common String Methods

| Method         | Purpose                                |
| -------------- | -------------------------------------- |
| `lower()`      | Convert to lowercase                   |
| `upper()`      | Convert to uppercase                   |
| `capitalize()` | First character uppercase              |
| `title()`      | First character of each word uppercase |
| `swapcase()`   | Swap uppercase/lowercase               |
| `strip()`      | Remove surrounding whitespace          |
| `lstrip()`     | Remove left whitespace                 |
| `rstrip()`     | Remove right whitespace                |
| `replace()`    | Replace substring                      |
| `find()`       | Find first occurrence                  |
| `rfind()`      | Find last occurrence                   |
| `index()`      | Find index                             |
| `count()`      | Count occurrences                      |
| `startswith()` | Check beginning                        |
| `endswith()`   | Check ending                           |
| `split()`      | String → List                          |
| `join()`       | List → String                          |
| `isalpha()`    | Check alphabet                         |
| `isdigit()`    | Check digit                            |
| `isalnum()`    | Check alphabet/digit                   |
| `isspace()`    | Check whitespace                       |
| `islower()`    | Check lowercase                        |
| `isupper()`    | Check uppercase                        |

---

# 44. Important Built-in Functions

| Function  | Purpose            |
| --------- | ------------------ |
| `len(s)`  | Length             |
| `str(x)`  | Convert to string  |
| `int(s)`  | Convert to integer |
| `list(s)` | String → List      |
| `ord(ch)` | Character → number |
| `chr(n)`  | Number → character |

---

# 45. Most Important for String DSA

Master these first:

```python
len(s)

s.lower()
s.upper()

s.strip()
s.replace()

s.find()
s.count()

s.split()
"".join(arr)

ch.isalpha()
ch.isdigit()
ch.isalnum()

ch in s

s[i]
s[start:end]
s[::-1]

ord(ch)
chr(n)
```

---

# 46. String DSA Loop Patterns

## Pattern 1 — Value only

```python
for ch in s:
    print(ch)
```

Use when you don't need the index.

---

## Pattern 2 — Index + value

```python
for i in range(len(s)):
    print(i, s[i])
```

Use when you need the index.

---

## Pattern 3 — `enumerate()`

```python
for i, ch in enumerate(s):
    print(i, ch)
```

Use when you need both index and character.

---

# 47. Frequency Counting

One of the most important string DSA patterns.

```python
s = "banana"

frequency = {}

for ch in s:
    if ch in frequency:
        frequency[ch] += 1
    else:
        frequency[ch] = 1

print(frequency)
```

Output:

```text
{'b': 1, 'a': 3, 'n': 2}
```

### Shorter version

```python
frequency = {}

for ch in s:
    frequency[ch] = frequency.get(ch, 0) + 1
```

---

# 48. Check Palindrome

A palindrome reads the same forward and backward.

```python
s = "madam"

if s == s[::-1]:
    print("Palindrome")
else:
    print("Not Palindrome")
```

Output:

```text
Palindrome
```

---

# 49. Reverse a String

### Method 1 — Slicing

```python
s = "hello"

reverse = s[::-1]

print(reverse)
```

### Method 2 — Loop

```python
s = "hello"

reverse = ""

for ch in s:
    reverse = ch + reverse

print(reverse)
```

---

# 50. Count Digits in a String

```python
s = "abc12345"

count = 0

for ch in s:
    if ch.isdigit():
        count += 1

print(count)
```

Output:

```text
5
```

---

# 51. Count Vowels

```python
s = "hello world"

count = 0

for ch in s:
    if ch in "aeiou":
        count += 1

print(count)
```

Output:

```text
3
```

---

# 52. Remove Spaces

```python
s = "hello world python"

s = s.replace(" ", "")

print(s)
```

Output:

```text
helloworldpython
```

---

# 53. Convert String to Lowercase Before Comparing

Useful when comparison should be case-insensitive.

```python
s1 = "Hello"
s2 = "hello"

if s1.lower() == s2.lower():
    print("Same")
```

Output:

```text
Same
```

---

# 54. String Methods — DSA Decision Guide

### Need length?

```python
len(s)
```

### Need each character?

```python
for ch in s:
```

### Need index?

```python
for i in range(len(s)):
```

### Need index + character?

```python
for i, ch in enumerate(s):
```

### Need reverse?

```python
s[::-1]
```

### Need substring?

```python
s[start:end]
```

### Need to check if character exists?

```python
if ch in s:
```

### Need frequency?

```python
s.count(ch)
```

or:

```python
frequency = {}

for ch in s:
    frequency[ch] = frequency.get(ch, 0) + 1
```

### Need to split words?

```python
s.split()
```

### Need to combine strings?

```python
"".join(arr)
```

### Need character number?

```python
ord(ch)
```

### Need number to character?

```python
chr(n)
```

---

# ⭐ Final Memory Map

```text
STRING
│
├── ACCESS
│   ├── s[i]
│   ├── s[-1]
│   └── s[start:end]
│
├── TRAVERSAL
│   ├── for ch in s
│   ├── for i in range(len(s))
│   └── enumerate(s)
│
├── SEARCH
│   ├── in
│   ├── find()
│   ├── rfind()
│   └── index()
│
├── COUNT
│   └── count()
│
├── MODIFY / CREATE NEW STRING
│   ├── lower()
│   ├── upper()
│   ├── replace()
│   ├── strip()
│   └── slicing
│
├── CONVERT
│   ├── split()
│   ├── join()
│   ├── list()
│   ├── str()
│   └── int()
│
├── CHECK
│   ├── isalpha()
│   ├── isdigit()
│   ├── isalnum()
│   ├── isspace()
│   ├── islower()
│   └── isupper()
│
└── CHARACTER OPERATIONS
    ├── ord()
    └── chr()
```

# 🚀 Must-Memorize String DSA Toolkit

```python
len(s)

for ch in s:
for i in range(len(s)):
for i, ch in enumerate(s):

s[i]
s[start:end]
s[::-1]

s.lower()
s.upper()
s.strip()
s.replace()

s.find()
s.count()

s.split()
"".join(arr)

ch.isalpha()
ch.isdigit()
ch.isalnum()

ch in s

ord(ch)
chr(n)
```

> **DSA Rule:** Don't try to memorize every string method at once. First master **indexing → slicing → loops → `in` → `find()` → `count()` → `split()`/`join()` → `isdigit()`/`isalpha()` → `ord()`/`chr()`**. These will cover a large portion of beginner and intermediate string problems.
