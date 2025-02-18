# Functions Exercises

## Exercise 1: Basic Function Definition
Practice creating and using simple functions.

1. Create a function called `calculate_average` that:
   - Takes a list of numbers as input
   - Returns their average
   - Includes a proper docstring
2. Create a function called `format_name` that:
   - Takes first_name and last_name as parameters
   - Returns the full name in "Last, First" format
3. Test both functions with sample data

<details>
<summary>Solution</summary>

```python
def calculate_average(numbers):
    """
    Calculate the average of a list of numbers.
    
    Args:
        numbers (list): A list of numerical values
        
    Returns:
        float: The average of the numbers
    """
    if not numbers:
        return 0
    return sum(numbers) / len(numbers)

def format_name(first_name, last_name):
    """
    Format a name in 'Last, First' format.
    
    Args:
        first_name (str): The person's first name
        last_name (str): The person's last name
        
    Returns:
        str: Formatted name string
    """
    return f"{last_name}, {first_name}"

# Test the functions
test_numbers = [85, 92, 78, 90, 88]
print(f"Average: {calculate_average(test_numbers)}")  # 86.6

print(format_name("Alice", "Smith"))  # Smith, Alice
```
</details>

## Exercise 2: Default and Keyword Arguments
Practice working with different types of function arguments.

1. Create a function `create_dataset` that:
   - Takes parameters: size (required), mean (default=0), std_dev (default=1)
   - Generates a list of random numbers with these parameters
   - Uses the random module's gauss() function
2. Create a function `format_data` that:
   - Takes data and formatting parameters (precision, separator)
   - Has appropriate default values
   - Returns formatted string representation
3. Test the functions using different combinations of arguments

<details>
<summary>Solution</summary>

```python
import random

def create_dataset(size, mean=0, std_dev=1):
    """
    Create a dataset of random numbers with specified parameters.
    
    Args:
        size (int): Number of data points
        mean (float): Mean of the distribution (default=0)
        std_dev (float): Standard deviation (default=1)
        
    Returns:
        list: Generated dataset
    """
    return [random.gauss(mean, std_dev) for _ in range(size)]

def format_data(data, precision=2, separator=", "):
    """
    Format a list of numbers as a string.
    
    Args:
        data (list): List of numbers
        precision (int): Decimal places (default=2)
        separator (str): String to separate numbers (default=", ")
        
    Returns:
        str: Formatted string
    """
    formatted = [f"{x:.{precision}f}" for x in data]
    return separator.join(formatted)

# Test with different argument combinations
data1 = create_dataset(5)  # Use defaults
data2 = create_dataset(5, mean=10, std_dev=2)  # Specify all arguments
data3 = create_dataset(size=5, std_dev=0.5)  # Mix positional and keyword

print(format_data(data1))  # Default formatting
print(format_data(data2, precision=3))  # Custom precision
print(format_data(data3, separator=" | "))  # Custom separator
```
</details>

## Exercise 3: Variable Arguments
Practice using *args and **kwargs.

1. Create a function `calculate_statistics` that:
   - Accepts any number of numeric arguments
   - Returns a dictionary with min, max, and average
2. Create a function `create_report` that:
   - Accepts any number of keyword arguments
   - Creates a formatted report string
3. Create a function `plot_data` that:
   - Takes a title parameter
   - Accepts any number of datasets as positional args
   - Accepts any number of plotting options as keyword args

<details>
<summary>Solution</summary>

```python
def calculate_statistics(*numbers):
    """
    Calculate statistics for any number of values.
    
    Args:
        *numbers: Variable number of numeric values
        
    Returns:
        dict: Dictionary containing min, max, and average
    """
    if not numbers:
        return None
    return {
        'min': min(numbers),
        'max': max(numbers),
        'average': sum(numbers) / len(numbers)
    }

def create_report(**kwargs):
    """
    Create a report from keyword arguments.
    
    Args:
        **kwargs: Arbitrary keyword arguments
        
    Returns:
        str: Formatted report
    """
    report = "Report:\n"
    for key, value in kwargs.items():
        report += f"{key}: {value}\n"
    return report

def plot_data(title, *datasets, **options):
    """
    Simulate plotting multiple datasets with options.
    
    Args:
        title (str): Plot title
        *datasets: Variable number of datasets to plot
        **options: Plot configuration options
    """
    print(f"Creating plot: {title}")
    print(f"Number of datasets: {len(datasets)}")
    print("Plotting options:")
    for key, value in options.items():
        print(f"  {key}: {value}")

# Test the functions
stats = calculate_statistics(1, 2, 3, 4, 5)
print(stats)  # {'min': 1, 'max': 5, 'average': 3.0}

report = create_report(title="Monthly Report", 
                      author="Alice",
                      date="2024-02-17")
print(report)

plot_data("Sales Data",
          [1, 2, 3], [4, 5, 6],  # datasets
          color='blue',
          style='lines',
          grid=True)  # options
```
</details>

## Exercise 4: Return Values and Multiple Returns
Practice working with different return patterns.

1. Create a function `analyze_text` that returns multiple values:
   - Word count
   - Character count
   - Average word length
2. Create a function `process_data` that returns different values based on a mode parameter:
   - 'summary': returns a single summary value
   - 'detailed': returns a tuple of values
   - 'all': returns a dictionary of all calculations
3. Test both functions and demonstrate unpacking return values

<details>
<summary>Solution</summary>

```python
def analyze_text(text):
    """
    Analyze text and return multiple statistics.
    
    Args:
        text (str): Text to analyze
        
    Returns:
        tuple: (word_count, char_count, avg_word_length)
    """
    words = text.split()
    word_count = len(words)
    char_count = len(text)
    avg_length = char_count / word_count if word_count > 0 else 0
    
    return word_count, char_count, avg_length

def process_data(numbers, mode='summary'):
    """
    Process data with different levels of detail.
    
    Args:
        numbers (list): Numbers to process
        mode (str): Processing mode ('summary', 'detailed', or 'all')
        
    Returns:
        various: Different return types based on mode
    """
    if not numbers:
        return None
        
    total = sum(numbers)
    avg = total / len(numbers)
    maximum = max(numbers)
    minimum = min(numbers)
    
    if mode == 'summary':
        return avg
    elif mode == 'detailed':
        return minimum, maximum, avg
    elif mode == 'all':
        return {
            'total': total,
            'average': avg,
            'max': maximum,
            'min': minimum,
            'count': len(numbers)
        }

# Test analyze_text
text = "Python is a great programming language"
words, chars, avg_len = analyze_text(text)
print(f"Words: {words}, Chars: {chars}, Average length: {avg_len:.2f}")

# Test process_data
numbers = [1, 2, 3, 4, 5]
print(f"Summary: {process_data(numbers, 'summary')}")
min_val, max_val, avg_val = process_data(numbers, 'detailed')
print(f"Min: {min_val}, Max: {max_val}, Avg: {avg_val}")
all_stats = process_data(numbers, 'all')
print("All stats:", all_stats)
```
</details>

## Exercise 5: Lambda Functions
Practice using lambda functions in data processing scenarios.

1. Create a list of tuples containing (name, age, score)
2. Use lambda functions with sorted() to:
   - Sort by age
   - Sort by score
   - Sort by name length
3. Use lambda with filter() to:
   - Find all people over 20
   - Find all scores above average
4. Use lambda with map() to:
   - Create a list of formatted strings
   - Transform the scores

<details>
<summary>Solution</summary>

```python
# Create data
people = [
    ("Alice", 24, 85),
    ("Bob", 19, 92),
    ("Charlie", 21, 78),
    ("David", 25, 95),
    ("Eve", 20, 88)
]

# Sorting with lambda
by_age = sorted(people, key=lambda x: x[1])
by_score = sorted(people, key=lambda x: x[2], reverse=True)
by_name_length = sorted(people, key=lambda x: len(x[0]))

print("Sorted by age:", by_age)
print("Sorted by score:", by_score)
print("Sorted by name length:", by_name_length)

# Filtering with lambda
over_20 = list(filter(lambda x: x[1] > 20, people))
avg_score = sum(p[2] for p in people) / len(people)
above_avg = list(filter(lambda x: x[2] > avg_score, people))

print("People over 20:", over_20)
print("Above average scores:", above_avg)

# Mapping with lambda
formatted = list(map(lambda x: f"{x[0]}: {x[2]}%", people))
scaled_scores = list(map(lambda x: (x[0], x[1], x[2] * 1.1), people))

print("Formatted strings:", formatted)
print("Scaled scores:", scaled_scores)
```
</details>

## Exercise 6: Scope and Best Practices
Practice writing functions following Python best practices.

1. Create a module of utility functions for data processing that demonstrates:
   - Clear function names
   - Proper docstrings
   - Parameter validation
   - Appropriate use of default arguments
   - Error handling
2. Include functions for:
   - Data validation
   - Data transformation
   - Statistical calculations
3. Write example usage that demonstrates proper variable scope

<details>
<summary>Solution</summary>

```python
def validate_dataset(data, required_columns=None, allow_nulls=False):
    """
    Validate a dataset against specified criteria.
    
    Args:
        data (list): List of dictionaries representing dataset
        required_columns (list): List of required column names
        allow_nulls (bool): Whether null values are allowed
        
    Returns:
        tuple: (is_valid, error_message)
    """
    if not data:
        return False, "Empty dataset"
        
    if required_columns is None:
        required_columns = []
        
    # Check required columns
    for row in data:
        missing_cols = [col for col in required_columns if col not in row]
        if missing_cols:
            return False, f"Missing required columns: {missing_cols}"
            
        # Check for null values
        if not allow_nulls:
            null_cols = [k for k, v in row.items() if v is None]
            if null_cols:
                return False, f"Null values found in columns: {null_cols}"
                
    return True, "Data is valid"

def transform_dataset(data, transformations):
    """
    Apply transformations to a dataset.
    
    Args:
        data (list): List of dictionaries representing dataset
        transformations (dict): Dictionary of column:function pairs
        
    Returns:
        list: Transformed dataset
    """
    result = []
    for row in data:
        transformed_row = row.copy()
        for column, transform_func in transformations.items():
            if column in row:
                try:
                    transformed_row[column] = transform_func(row[column])
                except Exception as e:
                    print(f"Error transforming {column}: {e}")
        result.append(transformed_row)
    return result

def calculate_summary_stats(data, columns):
    """
    Calculate summary statistics for specified columns.
    
    Args:
        data (list): List of dictionaries representing dataset
        columns (list): Columns to analyze
        
    Returns:
        dict: Summary statistics per column
    """
    stats = {}
    for column in columns:
        values = [row[column] for row in data if column in row]
        if not values:
            continue
            
        stats[column] = {
            'min': min(values),
            'max': max(values),
            'avg': sum(values) / len(values),
            'count': len(values)
        }
    return stats

# Example usage
if __name__ == "__main__":
    # Sample dataset
    dataset = [
        {'id': 1, 'value': 10, 'category': 'A'},
        {'id': 2, 'value': 20, 'category': 'B'},
        {'id': 3, 'value': 30, 'category': 'A'}
    ]
    
    # Validate data
    is_valid, message = validate_dataset(
        dataset,
        required_columns=['id', 'value']
    )
    print(f"Validation: {message}")
    
    # Transform data
    transformations = {
        'value': lambda x: x * 2,
        'category': str.lower
    }
    transformed = transform_dataset(dataset, transformations)
    print("Transformed data:", transformed)
    
    # Calculate statistics
    stats = calculate_summary_stats(dataset, ['value'])
    print("Statistics:", stats)
```
</details>