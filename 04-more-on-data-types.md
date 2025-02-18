# More on Data Types - Exercises

## Exercise 1: Understanding References and Objects

In this exercise, you'll explore how Python handles object references.

Open the Python REPL and follow these steps:

1. Create two variables `a` and `b` where `b` references the same object as `a`.
2. Create a third variable `c` that creates a new object with the same value.
3. Use the `id()` function to verify the object references.
4. Print the IDs and values to understand what's happening.

<details>
<summary>Solution</summary>

```python
# Create initial object and reference
a = [1, 2, 3]  # Create a list object
b = a          # Create a reference to the same object
c = [1, 2, 3]  # Create a new list object with same values

# Print IDs and values
print(f"a: id={id(a)}, value={a}")
print(f"b: id={id(b)}, value={b}")
print(f"c: id={id(c)}, value={c}")

# Verify references by modifying a
a.append(4)
print("\nAfter modifying a:")
print(f"a: value={a}")  # Will show [1, 2, 3, 4]
print(f"b: value={b}")  # Will also show [1, 2, 3, 4] because it references same object
print(f"c: value={c}")  # Will still show [1, 2, 3] because it's a different object
```

Expected output will show that:
- `a` and `b` have the same ID (they reference the same object)
- `c` has a different ID (it's a separate object)
- When `a` is modified, `b` changes too (because they reference the same object)
- `c` remains unchanged (because it's a separate object)
</details>

## Exercise 2: Exploring Mutability

Create a new Python script that demonstrates the differences between mutable and immutable types.

1. Create a tuple `coordinates` with x, y values (e.g., `(10, 20)`).
2. Print the tuple.
3. Create a list `points` with the same values.
4. Modify one of the values in the list.
5. Print both original and modified values.
6. Print a message explaining why you can modify the list but not the tuple.

<details>
<summary>Solution</summary>

```python
# Working with immutable tuple
coordinates = (10, 20)
print(f"Original coordinates: {coordinates}")

# Note: We can't modify the tuple because it's immutable
print("We can't modify the tuple because it's immutable")

# Working with mutable list
points = [10, 20]
print(f"\nOriginal points: {points}")
points[0] = 30  # This will work
print(f"Modified points: {points}")

# Explanation
print("\nWhy this happens:")
print("- Tuples are immutable, which means they cannot be changed after creation")
print("- Lists are mutable, which means we can modify their contents")
```
</details>

## Exercise 3: String Operations Challenge

Open the Python REPL and process a string using various string operations:

1. Create a string variable containing the text: "  Python Programming is Fun!  "
2. Perform the following operations and print the results:
   - Remove leading and trailing whitespace
   - Convert to uppercase
   - Replace "Fun" with "Awesome"
   - Split the string into words
   - Join the words back together with "-" between them
3. Count how many times the letter 'i' appears (case insensitive)
4. Create a slice that extracts just the word "Programming"

<details>
<summary>Solution</summary>

```python
# Initial string
text = "  Python Programming is Fun!  "
print(f"Original text: '{text}'")

# String operations
cleaned = text.strip()
print(f"\nCleaned: '{cleaned}'")

uppercase = cleaned.upper()
print(f"Uppercase: '{uppercase}'")

replaced = cleaned.replace("Fun", "Awesome")
print(f"Replaced: '{replaced}'")

words = cleaned.split()
print(f"Split words: {words}")

joined = "-".join(words)
print(f"Joined with hyphens: '{joined}'")

# Count 'i' (case insensitive)
i_count = cleaned.lower().count('i')
print(f"\nNumber of 'i' characters: {i_count}")

# Extract "Programming" using slice
programming = cleaned[7:18]  # Indexes based on cleaned string
print(f"Extracted 'Programming': '{programming}'")
```
</details>

## Exercise 4: String Formatting

Create a new Python script that demonstrates different string formatting techniques:

1. Create variables for:
   - A person's name
   - Their age
   - Their height in meters
   - Their favorite color
2. Create formatted strings using:
   - f-strings
   - str.format() method
   - % operator (old style)
3. Format the height to 2 decimal places
4. Create a table-like output showing the person's details

<details>
<summary>Solution</summary>

```python
# Setup variables
name = "Alice Johnson"
age = 28
height = 1.7523
favorite_color = "blue"

# 1. Using f-strings
print("=== Using f-strings ===")
print(f"Name: {name}")
print(f"Age: {age}")
print(f"Height: {height:.2f}m")
print(f"Favorite Color: {favorite_color}")

# 2. Using str.format()
print("\n=== Using str.format() ===")
print("Name: {}".format(name))
print("Age: {}".format(age))
print("Height: {:.2f}m".format(height))
print("Favorite Color: {}".format(favorite_color))

# 3. Using % operator
print("\n=== Using % operator ===")
print("Name: %s" % name)
print("Age: %d" % age)
print("Height: %.2fm" % height)
print("Favorite Color: %s" % favorite_color)

# 4. Creating a table-like output using f-strings
print("\n=== Person Details Table ===")
print("┌" + "─" * 30 + "┐")
print(f"│ {'Name:':<10} {name:<17} │")
print(f"│ {'Age:':<10} {age:<17} │")
print(f"│ {'Height:':<10} {height:.2f}m{' ' * 13} │")
print(f"│ {'Color:':<10} {favorite_color:<17} │")
print("└" + "─" * 30 + "┘")
```
</details>