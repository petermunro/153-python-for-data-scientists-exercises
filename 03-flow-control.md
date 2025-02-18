# Flow Control Exercises

## Exercise 1: Temperature Classifier
Write a program that classifies temperatures into different categories. 

1. Create a variable `temperature` and assign it a value (you can use `input()` to get it from the user)
2. Write an if-elif-else statement that prints:
   - "Freezing" if temperature is below 0
   - "Cold" if temperature is between 0 and 10
   - "Moderate" if temperature is between 11 and 20
   - "Warm" if temperature is between 21 and 30
   - "Hot" if temperature is above 30

<details>
<summary>Click to see solution</summary>

```python
temperature = float(input("Enter temperature in Celsius: "))

if temperature < 0:
    print("Freezing")
elif temperature <= 10:
    print("Cold")
elif temperature <= 20:
    print("Moderate")
elif temperature <= 30:
    print("Warm")
else:
    print("Hot")
```
</details>

## Exercise 2: Conditional Expression Practice
Write a program that uses conditional expressions to solve the following problems:

1. Create a variable `number` and assign it a value
2. Use a conditional expression to assign "even" or "odd" to a variable `number_type`
3. Use a conditional expression to assign "positive", "negative", or "zero" to a variable `sign`
4. Print both results

<details>
<summary>Click to see solution</summary>

```python
number = int(input("Enter a number: "))

number_type = "even" if number % 2 == 0 else "odd"
sign = "positive" if number > 0 else ("negative" if number < 0 else "zero")

print(f"The number is {number_type}")
print(f"The number is {sign}")
```
</details>

## Exercise 3: Sum of Squares
Write a program that calculates the sum of squares for numbers in a given range:

1. Use a for loop and the range() function to calculate the sum of squares from 1 to n
2. Ask the user to input n
3. Print each number and its square as you go
4. Print the final sum

<details>
<summary>Click to see solution</summary>

```python
n = int(input("Enter a positive number: "))
sum_squares = 0

for i in range(1, n + 1):
    square = i * i
    sum_squares += square
    print(f"Number: {i}, Square: {square}")

print(f"Sum of squares from 1 to {n} is: {sum_squares}")
```
</details>

## Exercise 4: Number Guessing Game
Create a number guessing game using a while loop:

1. Import the random module (`import random`)
2. Generate a random number between 1 and 100 using `random.randint(1, 100)`
3. Use a while loop to repeatedly ask the user for guesses
4. For each guess, tell the user if their guess is:
   - Too high
   - Too low
   - Correct (and end the game)
5. Keep track of the number of guesses
6. Use break when the correct number is guessed

<details>
<summary>Click to see solution</summary>

```python
import random

target = random.randint(1, 100)
guesses = 0

while True:
    guess = int(input("Guess the number (1-100): "))
    guesses += 1
    
    if guess == target:
        print(f"Correct! You found it in {guesses} guesses!")
        break
    elif guess < target:
        print("Too low!")
    else:
        print("Too high!")
```
</details>

## Exercise 5: Prime Number Finder
Write a program that finds all prime numbers up to a given number:

1. Ask the user for a number n
2. Use nested loops to check each number from 2 to n
3. For each number, check if it's prime by testing for divisibility
4. Use continue to skip non-prime numbers
5. Print all prime numbers found
6. Use the else clause with your for loop to print a completion message

<details>
<summary>Click to see solution</summary>

```python
n = int(input("Enter a number: "))
print(f"Prime numbers up to {n}:")

for num in range(2, n + 1):
    for i in range(2, int(num ** 0.5) + 1):
        if num % i == 0:
            break
    else:
        print(num, end=" ")
else:
    print("\nSearch completed!")
```
</details>

## Bonus Challenge: FizzBuzz
Write a program that prints numbers from 1 to 100, but:

1. For multiples of 3, print "Fizz" instead of the number
2. For multiples of 5, print "Buzz" instead of the number
3. For numbers that are multiples of both 3 and 5, print "FizzBuzz"
4. Use continue to skip any numbers that don't meet these conditions

<details>
<summary>Click to see solution</summary>

```python
for i in range(1, 101):
    if i % 3 == 0 and i % 5 == 0:
        print("FizzBuzz")
        continue
    elif i % 3 == 0:
        print("Fizz")
        continue
    elif i % 5 == 0:
        print("Buzz")
        continue
    print(i)
```
</details>