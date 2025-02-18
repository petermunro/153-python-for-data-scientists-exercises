# Comprehensions Exercises

## Exercise 1: Basic List Comprehensions
Practice creating list comprehensions with simple transformations.

1. Create a list of squares for numbers 0 through 9 using a list comprehension
2. Create a list of even numbers from 0 to 20 using a list comprehension with a condition
3. Create a list of temperatures in Fahrenheit from a list of Celsius temperatures: [0, 10, 20, 30, 40]
   - Use the formula: F = (C * 9/5) + 32
4. Compare your comprehension solutions with traditional for loops

<details>
<summary>Solution</summary>

```python
# Squares
squares = [x**2 for x in range(10)]
print(f"Squares: {squares}")

# Even numbers
evens = [x for x in range(21) if x % 2 == 0]
print(f"Even numbers: {evens}")

# Temperature conversion
celsius = [0, 10, 20, 30, 40]
fahrenheit = [(c * 9/5) + 32 for c in celsius]
print(f"Fahrenheit: {fahrenheit}")

# Traditional for loop comparison
squares_traditional = []
for x in range(10):
    squares_traditional.append(x**2)
print(f"Squares (traditional): {squares_traditional}")
```
</details>

## Exercise 2: List Comprehensions with Strings
Work with string transformations using list comprehensions.

1. Given a list of names: ["alice", "bob", "charlie", "david", "eve"]
   - Create a list of capitalized names
   - Create a list of names longer than 4 characters
   - Create a list of names that start with 'a' or 'b' (case insensitive)
2. Given a list of email addresses, extract the usernames (part before @)
   - Example: ["alice@example.com", "bob@example.com", "charlie@example.com"]

<details>
<summary>Solution</summary>

```python
# Names list
names = ["alice", "bob", "charlie", "david", "eve"]

# Capitalize names
capitalized = [name.capitalize() for name in names]
print(f"Capitalized: {capitalized}")

# Names longer than 4 characters
long_names = [name for name in names if len(name) > 4]
print(f"Long names: {long_names}")

# Names starting with a or b
ab_names = [name for name in names if name.lower()[0] in 'ab']
print(f"Names starting with A or B: {ab_names}")

# Extract usernames from emails
emails = ["alice@example.com", "bob@example.com", "charlie@example.com"]
usernames = [email.split('@')[0] for email in emails]
print(f"Usernames: {usernames}")
```
</details>

## Exercise 3: Dictionary Comprehensions
Practice creating dictionaries using comprehensions.

1. Create a dictionary of number:square pairs for numbers 1-5
2. Create a dictionary of name:length pairs from a list of names
3. Create a dictionary of number:frequency pairs from a list of numbers
4. Filter a dictionary to keep only pairs where the value is above a threshold

<details>
<summary>Solution</summary>

```python
# Number:square pairs
squares = {x: x**2 for x in range(1, 6)}
print(f"Squares: {squares}")

# Name:length pairs
names = ["Alice", "Bob", "Charlie", "David"]
name_lengths = {name: len(name) for name in names}
print(f"Name lengths: {name_lengths}")

# Number frequency
numbers = [1, 2, 3, 2, 4, 1, 5, 2, 3]
frequency = {num: numbers.count(num) for num in set(numbers)}
print(f"Number frequency: {frequency}")

# Filter dictionary
scores = {"Alice": 85, "Bob": 92, "Charlie": 78, "David": 95}
high_scores = {name: score for name, score in scores.items() if score >= 90}
print(f"High scores: {high_scores}")
```
</details>

## Exercise 4: Set Comprehensions
Practice creating sets using comprehensions.

1. Create a set of squares for numbers 1-5
2. Create a set of vowels from a given string
3. Create a set of unique word lengths from a list of words
4. Create a set of all unique first letters from a list of names

<details>
<summary>Solution</summary>

```python
# Set of squares
squares = {x**2 for x in range(1, 6)}
print(f"Square numbers: {squares}")

# Vowels in string
text = "hello world how are you"
vowels = {char for char in text if char.lower() in 'aeiou'}
print(f"Vowels: {vowels}")

# Unique word lengths
words = ["hello", "world", "python", "programming", "code"]
lengths = {len(word) for word in words}
print(f"Unique lengths: {lengths}")

# First letters
names = ["Alice", "Bob", "Charlie", "David", "Bob", "Alice"]
first_letters = {name[0] for name in names}
print(f"First letters: {first_letters}")
```
</details>

## Exercise 5: Nested Comprehensions
Practice using comprehensions with nested structures.

1. Create a 3x3 multiplication table using a nested list comprehension
2. Flatten a list of lists using a list comprehension
3. Create a dictionary of character frequencies for each word in a list
4. Generate coordinate pairs for a grid from (0,0) to (2,2)

<details>
<summary>Solution</summary>

```python
# Multiplication table
mult_table = [[i * j for j in range(1, 4)] for i in range(1, 4)]
print("Multiplication table:")
for row in mult_table:
    print(row)

# Flatten list of lists
nested = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
flattened = [num for sublist in nested for num in sublist]
print(f"Flattened: {flattened}")

# Character frequencies per word
words = ["hello", "world"]
char_freq = {word: {char: word.count(char) for char in word} for word in words}
print(f"Character frequencies: {char_freq}")

# Coordinate pairs
coordinates = [(x, y) for x in range(3) for y in range(3)]
print(f"Coordinates: {coordinates}")
```
</details>

## Exercise 6: Data Processing Challenge
Combine different types of comprehensions to process data.

Given this data about students and their grades:
```python
students = [
    {"name": "Alice", "grades": [85, 92, 78]},
    {"name": "Bob", "grades": [88, 76, 95]},
    {"name": "Charlie", "grades": [90, 85, 88]}
]
```

1. Create a dictionary of student names and their average grades
2. Create a set of all unique grades
3. Create a list of students with averages above 85
4. Create a dictionary of highest grade per student

<details>
<summary>Solution</summary>

```python
# Student data
students = [
    {"name": "Alice", "grades": [85, 92, 78]},
    {"name": "Bob", "grades": [88, 76, 95]},
    {"name": "Charlie", "grades": [90, 85, 88]}
]

# Average grades
averages = {
    student["name"]: sum(student["grades"]) / len(student["grades"])
    for student in students
}
print(f"Averages: {averages}")

# Unique grades
all_grades = {
    grade
    for student in students
    for grade in student["grades"]
}
print(f"Unique grades: {all_grades}")

# High performing students
high_performers = [
    student["name"]
    for student in students
    if sum(student["grades"]) / len(student["grades"]) > 85
]
print(f"High performers: {high_performers}")

# Highest grades
highest_grades = {
    student["name"]: max(student["grades"])
    for student in students
}
print(f"Highest grades: {highest_grades}")
```
</details>