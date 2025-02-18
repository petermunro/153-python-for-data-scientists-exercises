# Python Getting Started - Exercises

## Exercise 1: Hello, Python!
Practice basic Python syntax and printing.

1. Create a Python script that prints "Hello, Python!" to the console
2. Add a comment above the print statement explaining what the code does
3. Print your name on a new line

<details>
<summary>Solution</summary>

```python
# Print a greeting message
print("Hello, Python!")
print("Your Name")
```
</details>

## Exercise 2: Variable Practice
Work with different variable types and printing.

1. Create variables of the following types:
   - An integer for your age
   - A float for your height in meters
   - A string for your name
   - A boolean for whether you like Python
2. Print each variable and its type using the `type()` function

<details>
<summary>Solution</summary>

```python
age = 25
height = 1.75
name = "Alice"
likes_python = True

print(f"Age: {age}, Type: {type(age)}")
print(f"Height: {height}, Type: {type(height)}")
print(f"Name: {name}, Type: {type(name)}")
print(f"Likes Python: {likes_python}, Type: {type(likes_python)}")
```
</details>

## Exercise 3: Type Conversion
Practice converting between different data types.

1. Create a string variable containing the number "42"
2. Convert it to an integer and store in a new variable
3. Convert the integer to a float and store in another variable
4. Print all three variables and their types

<details>
<summary>Solution</summary>

```python
number_string = "42"
number_int = int(number_string)
number_float = float(number_int)

print(f"String: {number_string}, Type: {type(number_string)}")
print(f"Integer: {number_int}, Type: {type(number_int)}")
print(f"Float: {number_float}, Type: {type(number_float)}")
```
</details>

## Exercise 4: Basic Calculator
Create a simple calculator using basic operations.

1. Create two integer variables with values of your choice
2. Perform and print the following operations:
   - Addition
   - Subtraction
   - Multiplication
   - Division
   - Integer division
   - Modulus
   - Exponentiation
3. Add comments explaining each operation

<details>
<summary>Solution</summary>

```python
num1 = 10
num2 = 3

# Addition
print(f"{num1} + {num2} = {num1 + num2}")

# Subtraction
print(f"{num1} - {num2} = {num1 - num2}")

# Multiplication
print(f"{num1} * {num2} = {num1 * num2}")

# Division
print(f"{num1} / {num2} = {num1 / num2}")

# Integer division
print(f"{num1} // {num2} = {num1 // num2}")

# Modulus (remainder)
print(f"{num1} % {num2} = {num1 % num2}")

# Exponentiation
print(f"{num1} ** {num2} = {num1 ** num2}")
```
</details>

## Exercise 5: String Operations
Practice working with strings and string methods.

1. Create a string variable containing your full name
2. Print the following:
   - The name in all uppercase
   - The name in all lowercase
   - The length of the name
   - The first character of the name
   - The last character of the name
3. Create a greeting using an f-string that includes your name

<details>
<summary>Solution</summary>

```python
full_name = "John Smith"

print(full_name.upper())
print(full_name.lower())
print(len(full_name))
print(full_name[0])
print(full_name[-1])
print(f"Hello, my name is {full_name}!")
```
</details>

## Exercise 6: User Input Calculator
Create an interactive calculator that takes user input.

1. Prompt the user to enter two numbers
2. Convert the input strings to floats
3. Calculate and print:
   - The sum of the numbers
   - The difference between them
   - Their product
   - Their quotient (first number divided by second)
4. Handle potential errors (like division by zero) using conditional statements

<details>
<summary>Solution</summary>

```python
# Get input from user
num1 = float(input("Enter the first number: "))
num2 = float(input("Enter the second number: "))

# Calculate and print results
print(f"Sum: {num1 + num2}")
print(f"Difference: {num1 - num2}")
print(f"Product: {num1 * num2}")

# Check for division by zero
if num2 != 0:
    print(f"Quotient: {num1 / num2}")
else:
    print("Cannot divide by zero!")
```
</details>

## Exercise 7: Temperature Converter
Create a program that converts temperatures between Celsius and Fahrenheit.

1. Prompt the user to enter a temperature in Celsius
2. Convert the input string to a float
3. Calculate the temperature in Fahrenheit using the formula: F = (C * 9/5) + 32
4. Print both temperatures using f-strings, formatted to one decimal place

<details>
<summary>Solution</summary>

```python
# Get temperature in Celsius
celsius = float(input("Enter temperature in Celsius: "))

# Convert to Fahrenheit
fahrenheit = (celsius * 9/5) + 32

# Print results
print(f"{celsius:.1f}°C is equal to {fahrenheit:.1f}°F")
```
</details>

## Exercise 8: Simple Login System
Create a basic login system using conditional statements.

1. Create variables for a username and password
2. Prompt the user to enter their username and password
3. Use conditional statements to check if:
   - Both username and password match
   - Username matches but password doesn't
   - Username doesn't match
4. Print appropriate messages for each case

<details>
<summary>Solution</summary>

```python
# Set correct credentials
correct_username = "python_user"
correct_password = "secure123"

# Get user input
username = input("Enter username: ")
password = input("Enter password: ")

# Check credentials
if username == correct_username:
    if password == correct_password:
        print("Login successful!")
    else:
        print("Incorrect password")
else:
    print("Username not found")
```
</details>