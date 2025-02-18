# Data Structures - Dictionaries and Sets Exercises

## Exercise 1: Basic Dictionary Operations
Practice creating and manipulating dictionaries with student data.

1. Create a dictionary containing student information:
   - Name: "Alice"
   - Age: 20
   - Grades: [85, 92, 78]
   - Active: True
2. Perform the following operations:
   - Print the student's name using both bracket notation and get()
   - Add a new key 'email' with value 'alice@example.com'
   - Update the age to 21
   - Print all keys and all values separately
3. Try to access a non-existent key 'phone' with a default value of 'Not Available'

<details>
<summary>Solution</summary>

```python
# Create student dictionary
student = {
    'name': 'Alice',
    'age': 20,
    'grades': [85, 92, 78],
    'active': True
}

# Access values
print(student['name'])               # Using brackets
print(student.get('name'))          # Using get()

# Add and update values
student['email'] = 'alice@example.com'
student['age'] = 21

# Print keys and values
print("Keys:", list(student.keys()))
print("Values:", list(student.values()))

# Access non-existent key with default
phone = student.get('phone', 'Not Available')
print("Phone:", phone)              # Not Available
```
</details>

## Exercise 2: Dictionary Methods
Work with dictionary methods to manage a course roster.

1. Create a dictionary of student grades: {'Alice': 85, 'Bob': 92, 'Charlie': 78}
2. Perform these operations:
   - Add a new student 'David' with grade 95
   - Remove 'Charlie' and store their grade in a variable
   - Try to remove a non-existent student 'Eve' using pop() with a default value
   - Clear all entries from the dictionary
3. Print the dictionary after each operation

<details>
<summary>Solution</summary>

```python
# Create grades dictionary
grades = {'Alice': 85, 'Bob': 92, 'Charlie': 78}
print("Initial grades:", grades)

# Add new student
grades['David'] = 95
print("After adding David:", grades)

# Remove Charlie
charlie_grade = grades.pop('Charlie')
print(f"Charlie's grade was: {charlie_grade}")
print("After removing Charlie:", grades)

# Try to remove non-existent student
eve_grade = grades.pop('Eve', 'Not found')
print(f"Eve's grade: {eve_grade}")

# Clear dictionary
grades.clear()
print("After clearing:", grades)
```
</details>

## Exercise 3: Nested Dictionaries
Work with nested dictionaries to manage complex student records.

1. Create a nested dictionary for student records with this structure:
   ```python
   {
       'Alice': {
           'age': 20,
           'grades': {'math': 85, 'physics': 92}
       },
       'Bob': {
           'age': 21,
           'grades': {'math': 78, 'physics': 85}
       }
   }
   ```
2. Perform these operations:
   - Print Alice's physics grade
   - Update Bob's math grade to 82
   - Add a new subject 'chemistry' with grade 88 to Alice's grades
   - Calculate the average math grade across all students

<details>
<summary>Solution</summary>

```python
# Create nested dictionary
students = {
    'Alice': {
        'age': 20,
        'grades': {'math': 85, 'physics': 92}
    },
    'Bob': {
        'age': 21,
        'grades': {'math': 78, 'physics': 85}
    }
}

# Access nested values
print(f"Alice's physics grade: {students['Alice']['grades']['physics']}")

# Update nested value
students['Bob']['grades']['math'] = 82

# Add new nested value
students['Alice']['grades']['chemistry'] = 88

# Calculate average math grade
math_grades = [student['grades']['math'] for student in students.values()]
avg_math = sum(math_grades) / len(math_grades)
print(f"Average math grade: {avg_math}")
```
</details>

## Exercise 4: Basic Set Operations
Practice creating and manipulating sets.

1. Create two sets:
   - Set A: {1, 2, 3, 4, 5}
   - Set B: {4, 5, 6, 7, 8}
2. Perform and print the results of:
   - Union of A and B
   - Intersection of A and B
   - Elements in A but not in B
   - Symmetric difference of A and B
3. Check if:
   - 3 is in set A
   - {1, 2} is a subset of A
   - A is a superset of {4, 5}

<details>
<summary>Solution</summary>

```python
# Create sets
A = {1, 2, 3, 4, 5}
B = {4, 5, 6, 7, 8}

# Set operations
print(f"Union: {A | B}")
print(f"Intersection: {A & B}")
print(f"A - B: {A - B}")
print(f"Symmetric difference: {A ^ B}")

# Membership and subset operations
print(f"3 in A: {3 in A}")
print(f"{1, 2} is subset of A: {set([1, 2]).issubset(A)}")
print(f"A is superset of {4, 5}: {A.issuperset({4, 5})}")
```
</details>

## Exercise 5: Set Methods
Practice using set methods to manage unique data.

1. Create an empty set
2. Add these elements one at a time: 1, 2, 3
3. Try to add 2 again (notice it won't create a duplicate)
4. Add multiple elements at once: 4, 5, 6
5. Remove elements using different methods:
   - remove() for element 3
   - discard() for element 3 again (notice it won't raise an error)
   - pop() an element
6. Clear the set

<details>
<summary>Solution</summary>

```python
# Create empty set
numbers = set()

# Add elements
numbers.add(1)
numbers.add(2)
numbers.add(3)
print(f"After adding 1,2,3: {numbers}")

# Try adding duplicate
numbers.add(2)
print(f"After trying to add 2 again: {numbers}")

# Add multiple elements
numbers.update([4, 5, 6])
print(f"After updating: {numbers}")

# Remove elements
numbers.remove(3)
print(f"After removing 3: {numbers}")

numbers.discard(3)  # Won't raise error
print(f"After discarding 3: {numbers}")

popped = numbers.pop()
print(f"Popped value: {popped}")
print(f"After popping: {numbers}")

# Clear set
numbers.clear()
print(f"After clearing: {numbers}")
```
</details>

## Exercise 6: FrozenSet Challenge
Work with immutable sets (frozensets).

1. Create a frozenset from a list of student IDs: [101, 102, 103, 104]
2. Try to add a new ID (this should raise an error)
3. Create a dictionary using frozensets as keys:
   - Key: frozenset of prerequisites
   - Value: course name
4. Add at least three courses with their prerequisites
5. Print all courses that require a specific prerequisite

<details>
<summary>Solution</summary>

```python
# Create frozenset
student_ids = frozenset([101, 102, 103, 104])
print(f"Student IDs: {student_ids}")

# Try to modify (will raise error)
try:
    student_ids.add(105)
except AttributeError as e:
    print(f"Error: {e}")

# Create dictionary with frozenset keys
courses = {
    frozenset([101, 102]): "Advanced Python",
    frozenset([101]): "Basic Python",
    frozenset([101, 103]): "Data Science"
}

# Find courses requiring prerequisite 101
prereq = 101
required_courses = [
    course for prereqs, course in courses.items()
    if prereq in prereqs
]
print(f"Courses requiring {prereq}: {required_courses}")
```
</details>