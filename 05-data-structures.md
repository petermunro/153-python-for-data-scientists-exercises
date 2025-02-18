# Data Structures - Tuples and Lists Exercises

## Exercise 1: Basic Tuple Operations
Practice working with tuples and understanding their immutability.

1. Create a tuple containing three numbers: 10, 20, 30
2. Try to print:
   - The first element
   - The last element
   - The length of the tuple
3. Try to modify the first element (this should raise an error)
4. Create a new tuple by concatenating your first tuple with (40, 50)

<details>
<summary>Solution</summary>

```python
# Create the initial tuple
numbers = (10, 20, 30)

# Access elements and properties
print(numbers[0])      # 10
print(numbers[-1])     # 30
print(len(numbers))    # 3

# Try to modify (this will raise a TypeError)
try:
    numbers[0] = 100
except TypeError as e:
    print(f"Error: {e}")  # TypeError: 'tuple' object does not support item assignment

# Concatenate tuples
new_numbers = numbers + (40, 50)
print(new_numbers)     # (10, 20, 30, 40, 50)
```
</details>

## Exercise 2: Tuple Methods and Operators
Practice using tuple methods and operators.

1. Create a tuple of repeated values: (1, 2, 2, 3, 2, 4)
2. Use the tuple methods to:
   - Count how many times 2 appears
   - Find the index of the first occurrence of 2
   - Check if 5 exists in the tuple using the `in` operator
   - Check if 3 exists in the tuple using the `in` operator
3. Create two tuples of coordinates (x1, y1) and (x2, y2) and unpack them into separate variables

<details>
<summary>Solution</summary>

```python
# Create tuple with repeated values
numbers = (1, 2, 2, 3, 2, 4)

# Count occurrences
count_2 = numbers.count(2)
print(f"2 appears {count_2} times")  # 3 times

# Find first index of 2
index_2 = numbers.index(2)
print(f"First occurrence of 2 is at index {index_2}")  # index 1

# Check membership
print(5 in numbers)    # False
print(3 in numbers)    # True

# Tuple unpacking with coordinates
coord1 = (3, 4)
coord2 = (6, 8)
x1, y1 = coord1
x2, y2 = coord2
print(f"Point 1: ({x1}, {y1})")  # (3, 4)
print(f"Point 2: ({x2}, {y2})")  # (6, 8)
```
</details>

## Exercise 3: Working with Lists
Practice basic list operations that are commonly used in data science.

1. Create a list of temperatures: 25.2, 27.8, 23.5, 26.0, 24.8
2. Calculate and print:
   - The first three temperatures using slicing
   - The last two temperatures using negative indices
   - Every second temperature using step slicing
3. Add a new temperature of 26.5 to the end
4. Insert 24.0 at the beginning
5. Calculate the average temperature (sum divided by length)

<details>
<summary>Solution</summary>

```python
# Create the temperature list
temperatures = [25.2, 27.8, 23.5, 26.0, 24.8]

# Slicing
print(temperatures[:3])    # [25.2, 27.8, 23.5]
print(temperatures[-2:])   # [26.0, 24.8]
print(temperatures[::2])   # [25.2, 23.5, 24.8]

# Adding elements
temperatures.append(26.5)
temperatures.insert(0, 24.0)

# Calculate average
average = sum(temperatures) / len(temperatures)
print(f"Average temperature: {average:.2f}")
```
</details>

## Exercise 4: List Methods
Practice using common list methods for data manipulation.

1. Create a list of student scores: 85, 92, 78, 65, 95, 88
2. Perform the following operations:
   - Sort the scores in ascending order
   - Sort the scores in descending order
   - Find the highest and lowest scores
   - Remove the lowest score
   - Add a new score of 82
3. Print the list after each operation

<details>
<summary>Solution</summary>

```python
# Create the scores list
scores = [85, 92, 78, 65, 95, 88]

# Sort ascending
scores.sort()
print(f"Sorted scores: {scores}")  # [65, 78, 85, 88, 92, 95]

# Sort descending
scores.sort(reverse=True)
print(f"Descending scores: {scores}")  # [95, 92, 88, 85, 78, 65]

# Find highest and lowest
highest = max(scores)
lowest = min(scores)
print(f"Highest: {highest}, Lowest: {lowest}")  # 95, 65

# Remove lowest score
scores.remove(lowest)
print(f"After removing lowest: {scores}")  # [95, 92, 88, 85, 78]

# Add new score
scores.append(82)
print(f"After adding 82: {scores}")  # [95, 92, 88, 85, 78, 82]
```
</details>

## Exercise 5: Nested Data Structures
Practice working with nested lists and tuples.

1. Create a list of coordinate pairs (as tuples): [(1, 2), (3, 4), (5, 6)]
2. Add a new coordinate (7, 8) to the list
3. Access and print:
   - The coordinate pair at index 1 (the second pair)
   - The y-value of the first coordinate
   - All x-values using a loop
4. Calculate the distance from the origin (0, 0) for each point using: sqrt(x² + y²)

<details>
<summary>Solution</summary>

```python
import math

# Create list of coordinates
coordinates = [(1, 2), (3, 4), (5, 6)]

# Add new coordinate
coordinates.append((7, 8))

# Access elements
print(f"Second coordinate: {coordinates[1]}")  # (3, 4)
print(f"First y-value: {coordinates[0][1]}")  # 2

# Print all x-values
print("X values:", end=" ")
for coord in coordinates:
    print(coord[0], end=" ")  # 1 3 5 7

# Calculate distances from origin
print("\nDistances from origin:")
for x, y in coordinates:
    distance = math.sqrt(x**2 + y**2)
    print(f"Point ({x}, {y}): {distance:.2f}")
```
</details>

## Exercise 6: Slicing Challenge
Practice advanced slicing operations on lists.

1. Create a list of numbers from 0 to 9
2. Using slicing, perform these operations:
   - Extract the first half of the list
   - Extract the last half of the list
   - Reverse the entire list
   - Get every third number
   - Create a copy of the list
3. Modify the copy and verify the original is unchanged

<details>
<summary>Solution</summary>

```python
# Create list of numbers
numbers = list(range(10))  # [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

# First half
first_half = numbers[:5]
print(f"First half: {first_half}")  # [0, 1, 2, 3, 4]

# Last half
last_half = numbers[5:]
print(f"Last half: {last_half}")  # [5, 6, 7, 8, 9]

# Reverse
reversed_numbers = numbers[::-1]
print(f"Reversed: {reversed_numbers}")  # [9, 8, 7, 6, 5, 4, 3, 2, 1, 0]

# Every third number
every_third = numbers[::3]
print(f"Every third: {every_third}")  # [0, 3, 6, 9]

# Create and modify copy
numbers_copy = numbers[:]
numbers_copy[0] = 99
print(f"Modified copy: {numbers_copy}")  # [99, 1, 2, 3, 4, 5, 6, 7, 8, 9]
print(f"Original unchanged: {numbers}")  # [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```
</details>