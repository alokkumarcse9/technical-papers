# Python Cheatsheet — Dictionary, Nested Dictionary & Dataset Concepts

> **Dictionary = Key → Value**
>
> Python dictionaries are one of the most important data structures for DSA, JSON/API data, configuration, and dataset processing.

---

# 1. What is a Dictionary?

A dictionary stores data in **key-value pairs**.

```python
student = {
    "name": "Alok",
    "age": 22,
    "course": "B.Tech"
}
```

Think:

```text
key       → value
-------------------
name      → Alok
age       → 22
course    → B.Tech
```

---

# 2. Creating a Dictionary

```python
student = {
    "name": "Alok",
    "age": 22,
    "city": "Bangalore"
}
```

Empty dictionary:

```python
data = {}
```

Or:

```python
data = dict()
```

---

# 3. Accessing Values

Use the key:

```python
student = {
    "name": "Alok",
    "age": 22
}

print(student["name"])
print(student["age"])
```

Output:

```text
Alok
22
```

---

# 4. `get()`

`get()` safely accesses a value.

```python
student = {
    "name": "Alok",
    "age": 22
}

print(student.get("name"))
```

Output:

```text
Alok
```

If key doesn't exist:

```python
print(student.get("city"))
```

Output:

```text
None
```

You can provide a default:

```python
print(student.get("city", "Not Found"))
```

Output:

```text
Not Found
```

### `[]` vs `get()`

```python
student["city"]
```

Can raise:

```text
KeyError
```

But:

```python
student.get("city")
```

returns:

```text
None
```

### DSA Tip

Use:

```python
dictionary.get(key, default)
```

when you're not sure whether the key exists.

---

# 5. Adding a New Key

```python
student = {
    "name": "Alok"
}

student["age"] = 22

print(student)
```

Output:

```text
{'name': 'Alok', 'age': 22}
```

---

# 6. Updating a Value

```python
student = {
    "name": "Alok",
    "age": 22
}

student["age"] = 23

print(student)
```

Output:

```text
{'name': 'Alok', 'age': 23}
```

Same syntax is used for:

```text
Add
+
Update
```

---

# 7. `update()`

Update multiple values.

```python
student = {
    "name": "Alok",
    "age": 22
}

student.update({
    "age": 23,
    "city": "Bangalore"
})
```

Result:

```python
{
    "name": "Alok",
    "age": 23,
    "city": "Bangalore"
}
```

---

# 8. `keys()`

Returns dictionary keys.

```python
student = {
    "name": "Alok",
    "age": 22
}

print(student.keys())
```

You can loop:

```python
for key in student.keys():
    print(key)
```

Output:

```text
name
age
```

---

# 9. `values()`

Returns all values.

```python
for value in student.values():
    print(value)
```

Output:

```text
Alok
22
```

---

# 10. `items()`

Returns key-value pairs.

```python
for key, value in student.items():
    print(key, value)
```

Output:

```text
name Alok
age 22
```

### Very Important

When you need both key and value:

```python
for key, value in dictionary.items():
```

---

# 11. Checking if Key Exists

Use:

```python
if "name" in student:
    print("Found")
```

Important:

```python
"name" in student
```

checks **keys**, not values.

---

# 12. Checking if Value Exists

Use:

```python
if "Alok" in student.values():
    print("Found")
```

---

# 13. `pop()`

Removes a key and returns its value.

```python
student = {
    "name": "Alok",
    "age": 22
}

age = student.pop("age")

print(age)
print(student)
```

Output:

```text
22
{'name': 'Alok'}
```

---

# 14. `popitem()`

Removes the last inserted key-value pair.

```python
student = {
    "name": "Alok",
    "age": 22
}

item = student.popitem()

print(item)
```

Output:

```text
('age', 22)
```

---

# 15. `clear()`

Removes everything.

```python
student.clear()

print(student)
```

Output:

```text
{}
```

---

# 16. `copy()`

Creates a shallow copy.

```python
student = {
    "name": "Alok",
    "age": 22
}

new_student = student.copy()
```

---

# 17. Dictionary Length

Use:

```python
len(student)
```

Example:

```python
student = {
    "name": "Alok",
    "age": 22,
    "city": "Bangalore"
}

print(len(student))
```

Output:

```text
3
```

---

# 18. Dictionary Quick Table

| Method      | Purpose                    |
| ----------- | -------------------------- |
| `get()`     | Safely get value           |
| `keys()`    | Get keys                   |
| `values()`  | Get values                 |
| `items()`   | Get key-value pairs        |
| `update()`  | Add/update multiple values |
| `pop()`     | Remove specific key        |
| `popitem()` | Remove last pair           |
| `clear()`   | Remove everything          |
| `copy()`    | Copy dictionary            |

---

# 19. Dictionary Traversal

## Only Keys

```python
for key in student:
    print(key)
```

Same as:

```python
for key in student.keys():
    print(key)
```

---

## Only Values

```python
for value in student.values():
    print(value)
```

---

## Key + Value

```python
for key, value in student.items():
    print(key, value)
```

### DSA Rule

```text
Need key?
→ for key in d

Need value?
→ for value in d.values()

Need key + value?
→ for key, value in d.items()
```

---

# 20. Dictionary as a HashMap

In DSA, Python's dictionary behaves like a **hash map**.

Example:

```python
frequency = {}

for ch in "banana":
    frequency[ch] = frequency.get(ch, 0) + 1

print(frequency)
```

Output:

```text
{'b': 1, 'a': 3, 'n': 2}
```

This is one of the most important DSA patterns.

---

# 21. Frequency Map Pattern

```python
frequency = {}

for item in arr:
    frequency[item] = frequency.get(item, 0) + 1
```

Example:

```python
arr = [1, 2, 2, 3, 3, 3]

frequency = {}

for num in arr:
    frequency[num] = frequency.get(num, 0) + 1

print(frequency)
```

Output:

```text
{1: 1, 2: 2, 3: 3}
```

---

# 22. Nested Dictionary

A dictionary can contain another dictionary.

```python
students = {
    "student1": {
        "name": "Alok",
        "age": 22
    },
    "student2": {
        "name": "Rahul",
        "age": 23
    }
}
```

Structure:

```text
students
│
├── student1
│     ├── name → Alok
│     └── age  → 22
│
└── student2
      ├── name → Rahul
      └── age  → 23
```

---

# 23. Accessing Nested Dictionary

```python
print(students["student1"]["name"])
```

Output:

```text
Alok
```

Access age:

```python
print(students["student1"]["age"])
```

Output:

```text
22
```

### Think step-by-step

```text
students
   ↓
student1
   ↓
name
   ↓
Alok
```

Code:

```python
students["student1"]["name"]
```

---

# 24. Updating Nested Dictionary

```python
students["student1"]["age"] = 23
```

Now:

```python
{
    "student1": {
        "name": "Alok",
        "age": 23
    }
}
```

---

# 25. Adding Data to Nested Dictionary

```python
students["student1"]["city"] = "Bangalore"
```

Now:

```python
{
    "student1": {
        "name": "Alok",
        "age": 22,
        "city": "Bangalore"
    }
}
```

---

# 26. Nested Dictionary Traversal

```python
for student_id, student_data in students.items():

    print(student_id)

    for key, value in student_data.items():
        print(key, value)
```

Output:

```text
student1
name Alok
age 22
student2
name Rahul
age 23
```

---

# 27. Dictionary → List → Dictionary

Dictionary values can be lists.

```python
students = {
    "Alok": ["Python", "SQL", "DSA"],
    "Rahul": ["Java", "Spring", "SQL"]
}
```

Access:

```python
print(students["Alok"][0])
```

Output:

```text
Python
```

Structure:

```text
students
│
├── Alok
│    ├── Python
│    ├── SQL
│    └── DSA
│
└── Rahul
     ├── Java
     ├── Spring
     └── SQL
```

---

# 28. List of Dictionaries

This structure is extremely common in datasets and APIs.

```python
students = [
    {
        "name": "Alok",
        "age": 22
    },
    {
        "name": "Rahul",
        "age": 23
    }
]
```

Structure:

```text
List
│
├── Dictionary
│     ├── name
│     └── age
│
└── Dictionary
      ├── name
      └── age
```

Access:

```python
print(students[0]["name"])
```

Output:

```text
Alok
```

---

# 29. Loop Through List of Dictionaries

```python
for student in students:
    print(student["name"])
```

Output:

```text
Alok
Rahul
```

This pattern is extremely important.

```python
for item in dataset:
    print(item["column"])
```

---

# 30. Nested List + Dictionary

You can combine lists and dictionaries.

```python
students = {
    "class_a": [
        {"name": "Alok", "marks": 90},
        {"name": "Rahul", "marks": 85}
    ],
    "class_b": [
        {"name": "Aman", "marks": 88},
        {"name": "Ravi", "marks": 92}
    ]
}
```

Structure:

```text
students
│
├── class_a
│     │
│     ├── Dictionary
│     │     ├── name
│     │     └── marks
│     │
│     └── Dictionary
│           ├── name
│           └── marks
│
└── class_b
      │
      ├── Dictionary
      └── Dictionary
```

Access:

```python
print(students["class_a"][0]["name"])
```

Output:

```text
Alok
```

Access marks:

```python
print(students["class_a"][0]["marks"])
```

Output:

```text
90
```

---

# 31. Nested → Nested → Nested

Python allows dictionaries inside dictionaries at any depth.

Example:

```python
company = {
    "engineering": {
        "backend": {
            "team1": {
                "lead": "Alok",
                "members": 5
            }
        }
    }
}
```

Access:

```python
print(
    company["engineering"]
           ["backend"]
           ["team1"]
           ["lead"]
)
```

Output:

```text
Alok
```

### Read the structure from outside → inside

```text
company
   ↓
engineering
   ↓
backend
   ↓
team1
   ↓
lead
   ↓
Alok
```

---

# 32. Very Deep Nested Data

Example:

```python
data = {
    "country": {
        "state": {
            "city": {
                "college": {
                    "department": {
                        "student": {
                            "name": "Alok",
                            "age": 22
                        }
                    }
                }
            }
        }
    }
}
```

Access:

```python
name = (
    data["country"]
        ["state"]
        ["city"]
        ["college"]
        ["department"]
        ["student"]
        ["name"]
)

print(name)
```

---

# 33. How to Understand Nested Data

Don't look at the entire structure at once.

Break it down.

Given:

```python
data = {
    "user": {
        "profile": {
            "address": {
                "city": "Bangalore"
            }
        }
    }
}
```

First:

```python
data["user"]
```

Then:

```python
data["user"]["profile"]
```

Then:

```python
data["user"]["profile"]["address"]
```

Finally:

```python
data["user"]["profile"]["address"]["city"]
```

Output:

```text
Bangalore
```

### Mental Rule

```text
Outer key
    ↓
Next key
    ↓
Next key
    ↓
Final key
    ↓
Value
```

---

# 34. Mixed Nested Structure

Real-world data is often mixed.

```python
data = {
    "company": "ABC",
    "employees": [
        {
            "name": "Alok",
            "skills": ["Python", "SQL"],
            "address": {
                "city": "Bangalore",
                "country": "India"
            }
        },
        {
            "name": "Rahul",
            "skills": ["Java", "Spring"],
            "address": {
                "city": "Delhi",
                "country": "India"
            }
        }
    ]
}
```

This contains:

```text
Dictionary
    ↓
List
    ↓
Dictionary
    ↓
List
    ↓
Dictionary
```

---

# 35. Access Mixed Nested Data

Get first employee:

```python
employee = data["employees"][0]
```

Get name:

```python
print(data["employees"][0]["name"])
```

Get first skill:

```python
print(data["employees"][0]["skills"][0])
```

Get city:

```python
print(data["employees"][0]["address"]["city"])
```

Output:

```text
Alok
Python
Bangalore
```

---

# 36. Mixed Nested Traversal

```python
for employee in data["employees"]:

    print(employee["name"])

    print("Skills:")

    for skill in employee["skills"]:
        print(skill)

    print("City:", employee["address"]["city"])
```

---

# 37. What is a Dataset?

A **dataset** is a collection of structured or unstructured data used for:

* Analysis
* Machine learning
* Reporting
* Testing
* Research
* Business decisions

Example dataset:

```text
Name     Age    City        Salary
-----------------------------------
Alok     22     Bangalore   50000
Rahul    23     Delhi       60000
Aman     24     Mumbai      55000
```

---

# 38. Dataset as a List of Dictionaries

A simple Python representation:

```python
dataset = [
    {
        "name": "Alok",
        "age": 22,
        "city": "Bangalore",
        "salary": 50000
    },
    {
        "name": "Rahul",
        "age": 23,
        "city": "Delhi",
        "salary": 60000
    },
    {
        "name": "Aman",
        "age": 24,
        "city": "Mumbai",
        "salary": 55000
    }
]
```

Think:

```text
Dataset
│
├── Row 1 → Dictionary
├── Row 2 → Dictionary
└── Row 3 → Dictionary
```

---

# 39. Dataset Row

Each dictionary can represent one row/record.

```python
dataset[0]
```

Output:

```python
{
    "name": "Alok",
    "age": 22,
    "city": "Bangalore",
    "salary": 50000
}
```

---

# 40. Dataset Column

A key represents a column.

Example:

```python
"name"
"age"
"city"
"salary"
```

Get all names:

```python
names = []

for row in dataset:
    names.append(row["name"])
```

Output:

```python
["Alok", "Rahul", "Aman"]
```

---

# 41. Dataset Traversal

```python
for row in dataset:
    print(row)
```

To access a column:

```python
for row in dataset:
    print(row["name"])
```

To access multiple columns:

```python
for row in dataset:
    print(row["name"], row["salary"])
```

---

# 42. Filter Dataset

Find employees with salary greater than 55000.

```python
for row in dataset:
    if row["salary"] > 55000:
        print(row["name"])
```

Output:

```text
Rahul
```

---

# 43. Create Filtered Dataset

```python
high_salary = []

for row in dataset:
    if row["salary"] > 55000:
        high_salary.append(row)
```

Now:

```python
print(high_salary)
```

---

# 44. Dataset `max()`

Find employee with highest salary.

```python
highest = max(dataset, key=lambda row: row["salary"])

print(highest)
```

Output:

```python
{
    "name": "Rahul",
    "age": 23,
    "city": "Delhi",
    "salary": 60000
}
```

---

# 45. Dataset `min()`

```python
lowest = min(dataset, key=lambda row: row["salary"])

print(lowest)
```

---

# 46. Dataset `sorted()`

Sort by salary:

```python
sorted_dataset = sorted(
    dataset,
    key=lambda row: row["salary"]
)
```

Descending:

```python
sorted_dataset = sorted(
    dataset,
    key=lambda row: row["salary"],
    reverse=True
)
```

---

# 47. Dataset Aggregation

Calculate total salary:

```python
total_salary = 0

for row in dataset:
    total_salary += row["salary"]

print(total_salary)
```

Calculate average:

```python
average = total_salary / len(dataset)

print(average)
```

---

# 48. Dataset Frequency

Suppose:

```python
dataset = [
    {"name": "Alok", "city": "Bangalore"},
    {"name": "Rahul", "city": "Delhi"},
    {"name": "Aman", "city": "Bangalore"},
    {"name": "Ravi", "city": "Delhi"}
]
```

Count employees by city:

```python
city_count = {}

for row in dataset:
    city = row["city"]

    city_count[city] = city_count.get(city, 0) + 1
```

Result:

```python
{
    "Bangalore": 2,
    "Delhi": 2
}
```

---

# 49. Group Dataset by Key

Example:

```python
dataset = [
    {"name": "Alok", "department": "IT"},
    {"name": "Rahul", "department": "HR"},
    {"name": "Aman", "department": "IT"}
]
```

Group by department:

```python
groups = {}

for row in dataset:

    department = row["department"]

    if department not in groups:
        groups[department] = []

    groups[department].append(row)
```

Result:

```python
{
    "IT": [
        {"name": "Alok", "department": "IT"},
        {"name": "Aman", "department": "IT"}
    ],
    "HR": [
        {"name": "Rahul", "department": "HR"}
    ]
}
```

---

# 50. Dictionary Comprehension

Create a dictionary quickly.

```python
squares = {
    x: x * x
    for x in range(1, 6)
}
```

Result:

```python
{
    1: 1,
    2: 4,
    3: 9,
    4: 16,
    5: 25
}
```

---

# 51. Dictionary Comprehension with Condition

```python
even_squares = {
    x: x * x
    for x in range(1, 10)
    if x % 2 == 0
}
```

Result:

```python
{
    2: 4,
    4: 16,
    6: 36,
    8: 64
}
```

---

# 52. `setdefault()`

Useful for grouping data.

```python
groups = {}

groups.setdefault("IT", []).append("Alok")
groups.setdefault("IT", []).append("Aman")
groups.setdefault("HR", []).append("Rahul")
```

Result:

```python
{
    "IT": ["Alok", "Aman"],
    "HR": ["Rahul"]
}
```

This pattern is useful in DSA.

---

# 53. `defaultdict`

For advanced dictionary usage:

```python
from collections import defaultdict
```

Example:

```python
groups = defaultdict(list)

groups["IT"].append("Alok")
groups["IT"].append("Aman")
groups["HR"].append("Rahul")
```

Result:

```python
{
    "IT": ["Alok", "Aman"],
    "HR": ["Rahul"]
}
```

---

# 54. `Counter`

Very useful for frequency counting.

```python
from collections import Counter

arr = [1, 2, 2, 3, 3, 3]

frequency = Counter(arr)

print(frequency)
```

Output:

```text
Counter({3: 3, 2: 2, 1: 1})
```

String:

```python
frequency = Counter("banana")

print(frequency)
```

---

# 55. Nested Dictionary + Loop

```python
data = {
    "student1": {
        "name": "Alok",
        "marks": {
            "math": 90,
            "python": 95
        }
    }
}
```

Access:

```python
print(data["student1"]["marks"]["python"])
```

Output:

```text
95
```

Traversal:

```python
for student_id, student in data.items():

    print(student["name"])

    for subject, marks in student["marks"].items():
        print(subject, marks)
```

---

# 56. Nested Dictionary — 4 Levels

```python
data = {
    "school": {
        "class_10": {
            "section_A": {
                "student1": {
                    "name": "Alok",
                    "marks": 95
                }
            }
        }
    }
}
```

Access:

```python
marks = (
    data["school"]
        ["class_10"]
        ["section_A"]
        ["student1"]
        ["marks"]
)

print(marks)
```

Output:

```text
95
```

---

# 57. How to Read Any Nested Structure

Given:

```python
data = {
    "A": {
        "B": {
            "C": {
                "D": 100
            }
        }
    }
}
```

Read it:

```text
data
 ↓
A
 ↓
B
 ↓
C
 ↓
D
 ↓
100
```

Code:

```python
data["A"]["B"]["C"]["D"]
```

---

# 58. Mixed Structure Reading Trick

Given:

```python
data = {
    "users": [
        {
            "name": "Alok",
            "skills": [
                "Python",
                "SQL"
            ]
        }
    ]
}
```

Identify each layer:

```text
data
 ↓
Dictionary
 ↓
"users"
 ↓
List
 ↓
index [0]
 ↓
Dictionary
 ↓
"skills"
 ↓
List
 ↓
index [0]
 ↓
"Python"
```

Therefore:

```python
data["users"][0]["skills"][0]
```

Output:

```text
Python
```

---

# 59. The Most Important Nested Pattern

When you see:

```python
data["users"][0]["profile"]["address"]["city"]
```

Don't memorize the entire line.

Break it:

```text
data
 ↓
users
 ↓
[0]
 ↓
profile
 ↓
address
 ↓
city
```

Every:

```text
["key"]
```

means:

> Go inside this dictionary using this key.

Every:

```text
[index]
```

means:

> Go inside this list using this index.

---

# 60. Dictionary + List + Dictionary Formula

This pattern appears constantly in APIs and datasets:

```python
data["users"][0]["name"]
```

Meaning:

```text
Dictionary
   ↓
List
   ↓
Dictionary
   ↓
Value
```

---

# 61. Nested Nested Nested Formula

Example:

```python
data["company"]["departments"][0]["teams"][1]["members"][2]["name"]
```

Read it:

```text
data
 ↓
company          → dictionary key
 ↓
departments      → list
 ↓
[0]              → first department
 ↓
teams            → list
 ↓
[1]              → second team
 ↓
members          → list
 ↓
[2]              → third member
 ↓
name             → dictionary key
 ↓
value
```

---

# 62. Dataset Operations Cheat Sheet

```text
READ
    row["name"]

ADD
    row["city"] = "Bangalore"

UPDATE
    row["age"] = 23

DELETE
    row.pop("age")

FILTER
    if row["salary"] > 50000

SORT
    sorted(data, key=lambda x: x["salary"])

MAX
    max(data, key=lambda x: x["salary"])

MIN
    min(data, key=lambda x: x["salary"])

COUNT
    frequency dictionary
    Counter

GROUP
    dictionary + list

TRAVERSE
    for row in data:
```

---

# 63. Dictionary vs List

| Feature          | List     | Dictionary     |
| ---------------- | -------- | -------------- |
| Access           | Index    | Key            |
| Example          | `arr[0]` | `data["name"]` |
| Ordered          | Yes      | Yes            |
| Key-value        | No       | Yes            |
| DSA use          | Sequence | HashMap        |
| Lookup by key    | No       | Yes            |
| Duplicate values | Yes      | Yes            |
| Duplicate keys   | N/A      | No             |

---

# 64. List of Dictionaries vs Dictionary of Lists

### List of dictionaries

Best when each item is a record/row:

```python
students = [
    {"name": "Alok", "age": 22},
    {"name": "Rahul", "age": 23}
]
```

Think:

```text
Rows
 ↓
Records
```

### Dictionary of lists

Useful for grouping:

```python
students = {
    "IT": ["Alok", "Aman"],
    "HR": ["Rahul"]
}
```

Think:

```text
Group
 ↓
Members
```

---

# 65. Real-World API/JSON Structure

A very common structure:

```python
response = {
    "status": "success",
    "data": {
        "users": [
            {
                "id": 1,
                "name": "Alok",
                "address": {
                    "city": "Bangalore",
                    "country": "India"
                }
            }
        ]
    }
}
```

Get user name:

```python
name = response["data"]["users"][0]["name"]
```

Get city:

```python
city = response["data"]["users"][0]["address"]["city"]
```

---

# 66. JSON Mental Model

JSON commonly maps to Python like this:

```text
JSON object      → Python dict
JSON array       → Python list
JSON string      → Python str
JSON number      → Python int/float
JSON true        → Python True
JSON false       → Python False
JSON null        → Python None
```

Example JSON:

```json
{
    "name": "Alok",
    "skills": ["Python", "SQL"]
}
```

Python:

```python
{
    "name": "Alok",
    "skills": ["Python", "SQL"]
}
```

---

# 67. Common DSA Dictionary Patterns

## Frequency

```python
freq = {}

for x in arr:
    freq[x] = freq.get(x, 0) + 1
```

---

## Check duplicate

```python
seen = set()

for x in arr:

    if x in seen:
        print("Duplicate")

    seen.add(x)
```

---

## Two Sum

```python
seen = {}

for i, num in enumerate(arr):

    target = required - num

    if target in seen:
        return [seen[target], i]

    seen[num] = i
```

---

## Grouping

```python
groups = {}

for item in data:

    key = item["category"]

    if key not in groups:
        groups[key] = []

    groups[key].append(item)
```

---

# ⭐ Final Dictionary Memory Map

```text
                    DICTIONARY
                         │
        ┌────────────────┼────────────────┐
        ↓                ↓                ↓
      ACCESS           MODIFY          TRAVERSE
        │                │                │
   d[key]              d[key] = x      for key in d
   d.get(key)          update()        d.values()
                                          d.items()
        │
        ↓
      METHODS
        │
   ┌────┼────┬─────┬──────┐
   ↓    ↓    ↓     ↓      ↓
 get  keys values items   pop
```

---

# ⭐ Nested Data Memory Map

```text
Dictionary
│
├── Dictionary
│     └── Dictionary
│           └── Value
│
├── List
│     └── Dictionary
│           └── Value
│
└── List
      └── Dictionary
            └── List
                  └── Dictionary
                        └── Value
```

---

# 🚀 Must-Memorize Dictionary Toolkit

```python
# Create
d = {}

# Access
d[key]
d.get(key)

# Add / Update
d[key] = value
d.update({...})

# Check
key in d
value in d.values()

# Traverse
for key in d:
for value in d.values():
for key, value in d.items():

# Delete
d.pop(key)
d.popitem()
d.clear()

# Length
len(d)

# Frequency
freq[x] = freq.get(x, 0) + 1

# Grouping
groups.setdefault(key, []).append(value)

# Advanced
from collections import Counter
from collections import defaultdict
```

# 🧠 The Golden Rule for Nested Dictionaries

Whenever you see complicated data like:

```python
data["users"][0]["profile"]["address"]["city"]
```

**Don't panic.**

Read it one operation at a time:

```text
data
 ↓
["users"]       → dictionary
 ↓
[0]             → list
 ↓
["profile"]     → dictionary
 ↓
["address"]     → dictionary
 ↓
["city"]        → final value
```

And remember:

```text
["key"]  → Dictionary access

[index]  → List access

.items() → Key + value

.values() → Values

.keys()  → Keys
```

This mental model is the key to understanding **nested dictionaries, JSON, API responses, datasets, and real-world Python data structures**.
