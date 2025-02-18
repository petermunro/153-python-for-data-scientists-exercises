# Programming Pythonically Exercises

## Exercise 1: Sequence Operations
Transform non-Pythonic code into Pythonic code using sequence operations.

1. Rewrite these non-Pythonic examples in a Pythonic way:
   ```python
   # Check if element exists
   found = False
   for x in my_list:
       if x == target:
           found = True
           break
   
   # Get first and last elements
   first = my_list[0]
   last = my_list[len(my_list)-1]
   
   # Check if string is empty
   if len(string) == 0:
       print("Empty string")
   ```

<details>
<summary>Solution</summary>

```python
# Pythonic versions:

# Check if element exists
found = target in my_list

# Get first and last elements
first = my_list[0]  # or my_list[-0]
last = my_list[-1]

# Check if string is empty
if not string:
    print("Empty string")
```
</details>

## Exercise 2: Slicing Operations
Practice using Python's powerful slicing features.

1. Given a list of numbers `[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]`, use slicing to:
   - Get the first three numbers
   - Get the last three numbers
   - Get every second number
   - Reverse the list
   - Get all numbers except first and last
2. Given a string "Python Programming", use slicing to:
   - Get the first word
   - Get the last four characters
   - Reverse the string
   - Get every second character

<details>
<summary>Solution</summary>

```python
numbers = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

# List slicing
first_three = numbers[:3]  # [0, 1, 2]
last_three = numbers[-3:]  # [7, 8, 9]
every_second = numbers[::2]  # [0, 2, 4, 6, 8]
reversed_list = numbers[::-1]  # [9, 8, 7, 6, 5, 4, 3, 2, 1, 0]
middle = numbers[1:-1]  # [1, 2, 3, 4, 5, 6, 7, 8]

text = "Python Programming"

# String slicing
first_word = text[:6]  # "Python"
last_four = text[-4:]  # "ming"
reversed_text = text[::-1]  # "gnimmargorP nohtyP"
every_second_char = text[::2]  # "Pto rgamn"
```
</details>

## Exercise 3: String Operations
Practice Pythonic string operations using join() and split().

1. Convert these non-Pythonic string operations to Pythonic ones:
   ```python
   # Combine words
   result = ""
   for word in words:
       result += word + " "
   result = result.strip()
   
   # Split into words and convert to uppercase
   result = []
   for word in text.split():
       result.append(word.upper())
   
   # Create comma-separated string
   result = ""
   for i, item in enumerate(items):
       if i > 0:
           result += ", "
       result += str(item)
   ```

<details>
<summary>Solution</summary>

```python
# Pythonic versions:

# Combine words
result = " ".join(words)

# Split into words and convert to uppercase
result = [word.upper() for word in text.split()]
# Or even more Pythonic:
result = text.upper().split()

# Create comma-separated string
result = ", ".join(str(item) for item in items)
```
</details>

## Exercise 4: Sorting Operations
Practice Pythonic sorting techniques.

1. Given a list of dictionaries representing students:
   ```python
   students = [
       {'name': 'Alice', 'grade': 85, 'age': 20},
       {'name': 'Bob', 'grade': 92, 'age': 22},
       {'name': 'Charlie', 'grade': 78, 'age': 19}
   ]
   ```
2. Sort the students:
   - By name (case-insensitive)
   - By grade (descending)
   - By age, then by name
   - By length of name
3. Create a new sorted list vs. sorting in place when appropriate

<details>
<summary>Solution</summary>

```python
# Sort by name (case-insensitive)
by_name = sorted(students, key=lambda x: x['name'].lower())

# Sort by grade (descending)
by_grade = sorted(students, key=lambda x: x['grade'], reverse=True)

# Sort by age, then name
by_age_name = sorted(students, key=lambda x: (x['age'], x['name']))

# Sort by length of name
by_name_length = sorted(students, key=lambda x: len(x['name']))

# In-place sorting (modifies original list)
students.sort(key=lambda x: x['grade'], reverse=True)

print("By name:", by_name)
print("By grade:", by_grade)
print("By age and name:", by_age_name)
print("By name length:", by_name_length)
```
</details>

## Exercise 5: enumerate() and zip()
Practice using enumerate() and zip() for more Pythonic code.

1. Convert these non-Pythonic loops to use enumerate() or zip():
   ```python
   # Print items with index
   i = 0
   for item in items:
       print(f"Item {i}: {item}")
       i += 1
   
   # Combine two lists
   result = []
   for i in range(len(names)):
       result.append((names[i], scores[i]))
   
   # Create dictionary from lists
   result = {}
   for i in range(len(keys)):
       result[keys[i]] = values[i]
   ```

<details>
<summary>Solution</summary>

```python
# Using enumerate()
for i, item in enumerate(items):
    print(f"Item {i}: {item}")

# Using zip() for lists
result = list(zip(names, scores))

# Using zip() for dictionary
result = dict(zip(keys, values))

# More examples
names = ['Alice', 'Bob', 'Charlie']
scores = [85, 92, 78]
grades = ['B', 'A', 'C']

# Iterate over multiple sequences
for name, score, grade in zip(names, scores, grades):
    print(f"{name} got {score} ({grade})")

# Enumerate with start index
for i, name in enumerate(names, start=1):
    print(f"Student {i}: {name}")
```
</details>

## Exercise 6: Pythonic Data Processing
Combine multiple Pythonic concepts to process data efficiently.

Given this data:
```python
data = [
    "Alice,85,Python,23",
    "Bob,92,Java,21",
    "Charlie,78,Python,24",
    "David,95,Java,22",
    "Eve,88,Python,25"
]
```

1. Process the data to:
   - Split into fields
   - Create a dictionary of students by language
   - Calculate average score per language
   - Find the youngest and oldest student per language
2. Use Pythonic approaches:
   - List comprehensions
   - Dictionary comprehensions
   - Set operations
   - Sorting with lambda
   - String methods

<details>
<summary>Solution</summary>

```python
# Parse data
students = [line.split(',') for line in data]
students = [
    {'name': name, 'score': int(score), 
     'language': lang, 'age': int(age)}
    for name, score, lang, age in students
]

# Group by language using dictionary comprehension
by_language = {}
for student in students:
    lang = student['language']
    by_language.setdefault(lang, []).append(student)

# Calculate averages using comprehension
averages = {
    lang: sum(s['score'] for s in students) / len(students)
    for lang, students in by_language.items()
}

# Find age ranges using min/max with key function
age_ranges = {
    lang: {
        'youngest': min(students, key=lambda s: s['age']),
        'oldest': max(students, key=lambda s: s['age'])
    }
    for lang, students in by_language.items()
}

# Get unique languages using set
languages = {student['language'] for student in students}

# Print summary
for lang in sorted(languages):
    print(f"\n{lang} Summary:")
    print(f"Average score: {averages[lang]:.2f}")
    print(f"Youngest: {age_ranges[lang]['youngest']['name']} "
          f"({age_ranges[lang]['youngest']['age']})")
    print(f"Oldest: {age_ranges[lang]['oldest']['name']} "
          f"({age_ranges[lang]['oldest']['age']})")
```
</details>