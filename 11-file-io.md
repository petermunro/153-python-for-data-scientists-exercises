# File Input and Output Exercises

## Exercise 1: Basic File Operations
Practice basic file reading and writing operations.

1. Create a text file called 'sample.txt' with some sample content:
   - Write three lines of text to the file
   - Read the entire file content
   - Read the file line by line
   - Read all lines into a list
2. Demonstrate proper file handling using the `with` statement
3. Print the content using different reading methods

<details>
<summary>Solution</summary>

```python
# Writing to a file
with open('sample.txt', 'w') as file:
    file.write("First line\n")
    file.write("Second line\n")
    file.write("Third line\n")

# Reading entire file
with open('sample.txt', 'r') as file:
    content = file.read()
    print("Method 1 - Read entire file:")
    print(content)

# Reading line by line
with open('sample.txt', 'r') as file:
    print("\nMethod 2 - Read line by line:")
    for line in file:
        print(line.strip())

# Reading all lines into a list
with open('sample.txt', 'r') as file:
    lines = file.readlines()
    print("\nMethod 3 - Read lines into list:")
    for line in lines:
        print(line.strip())
```
</details>

## Exercise 2: Working with CSV Files
Practice reading and writing CSV files.

1. Create a CSV file containing student data:
   - Headers: Name, Age, Grade
   - At least 5 students
2. Read the CSV file and:
   - Print each row as a list
   - Calculate the average grade
   - Find the oldest and youngest students
3. Create a new CSV file with additional statistics:
   - Add a "Pass/Fail" column based on grade
   - Add an "Age Group" column

<details>
<summary>Solution</summary>

```python
import csv

# Create student data
students = [
    ['Name', 'Age', 'Grade'],
    ['Alice', 20, 85],
    ['Bob', 22, 92],
    ['Charlie', 21, 78],
    ['David', 23, 95],
    ['Eve', 19, 88]
]

# Write initial CSV file
with open('students.csv', 'w', newline='') as file:
    writer = csv.writer(file)
    writer.writerows(students)

# Read and process data
grades = []
ages = []
processed_data = [['Name', 'Age', 'Grade', 'Pass/Fail', 'Age Group']]

with open('students.csv', 'r') as file:
    reader = csv.reader(file)
    next(reader)  # Skip header
    for row in reader:
        name, age, grade = row
        age = int(age)
        grade = int(grade)
        
        # Collect data for statistics
        grades.append(grade)
        ages.append(age)
        
        # Process row
        pass_fail = 'Pass' if grade >= 80 else 'Fail'
        age_group = 'Young' if age < 21 else 'Mature'
        processed_data.append([name, age, grade, pass_fail, age_group])

# Calculate statistics
avg_grade = sum(grades) / len(grades)
youngest = min(ages)
oldest = max(ages)

print(f"Average grade: {avg_grade:.2f}")
print(f"Age range: {youngest} to {oldest}")

# Write processed data
with open('students_processed.csv', 'w', newline='') as file:
    writer = csv.writer(file)
    writer.writerows(processed_data)
```
</details>

## Exercise 3: File Path Operations
Practice working with file paths across different operating systems.

1. Create a directory structure for a data science project:
   ```
   project/
   ├── data/
   │   ├── raw/
   │   └── processed/
   ├── scripts/
   └── results/
   ```
2. Write functions to:
   - Create the directory structure
   - Check if specific files exist
   - List all files in a directory
   - Get absolute paths for files
3. Use proper path joining for cross-platform compatibility

<details>
<summary>Solution</summary>

```python
import os
import glob

def create_project_structure(base_dir):
    """Create a project directory structure."""
    # Create directories
    directories = [
        os.path.join(base_dir, 'data', 'raw'),
        os.path.join(base_dir, 'data', 'processed'),
        os.path.join(base_dir, 'scripts'),
        os.path.join(base_dir, 'results')
    ]
    
    for directory in directories:
        os.makedirs(directory, exist_ok=True)
        print(f"Created directory: {directory}")

def check_file_exists(filepath):
    """Check if a file exists and print its properties."""
    if os.path.exists(filepath):
        print(f"File exists: {filepath}")
        print(f"Directory: {os.path.dirname(filepath)}")
        print(f"Filename: {os.path.basename(filepath)}")
        print(f"Absolute path: {os.path.abspath(filepath)}")
    else:
        print(f"File does not exist: {filepath}")

def list_directory_contents(directory):
    """List all files in a directory and its subdirectories."""
    print(f"\nContents of {directory}:")
    for root, dirs, files in os.walk(directory):
        level = root.replace(directory, '').count(os.sep)
        indent = ' ' * 4 * level
        print(f"{indent}{os.path.basename(root)}/")
        subindent = ' ' * 4 * (level + 1)
        for file in files:
            print(f"{subindent}{file}")

# Test the functions
project_dir = 'project'
create_project_structure(project_dir)

# Create a sample file
sample_path = os.path.join(project_dir, 'data', 'raw', 'sample.txt')
with open(sample_path, 'w') as f:
    f.write("Sample data")

# Check file
check_file_exists(sample_path)

# List contents
list_directory_contents(project_dir)
```
</details>

## Exercise 4: Error Handling
Practice handling common file operation errors.

1. Write a function that demonstrates handling:
   - FileNotFoundError
   - PermissionError
   - IOError
2. Create test cases for each error type
3. Implement proper error recovery strategies
4. Use logging to record errors

<details>
<summary>Solution</summary>

```python
import os
import logging

# Setup logging
logging.basicConfig(
    filename='file_operations.log',
    level=logging.INFO,
    format='%(asctime)s - %(levelname)s - %(message)s'
)

def safe_file_operation(operation, filepath, content=None):
    """
    Safely perform file operations with error handling.
    
    Args:
        operation (str): 'read' or 'write'
        filepath (str): Path to the file
        content (str): Content to write (if operation is 'write')
    
    Returns:
        tuple: (success, result)
    """
    try:
        if operation == 'read':
            with open(filepath, 'r') as file:
                result = file.read()
                logging.info(f"Successfully read file: {filepath}")
                return True, result
                
        elif operation == 'write':
            with open(filepath, 'w') as file:
                file.write(content)
                logging.info(f"Successfully wrote to file: {filepath}")
                return True, "Write successful"
                
    except FileNotFoundError:
        error_msg = f"File not found: {filepath}"
        logging.error(error_msg)
        return False, error_msg
        
    except PermissionError:
        error_msg = f"Permission denied: {filepath}"
        logging.error(error_msg)
        return False, error_msg
        
    except IOError as e:
        error_msg = f"IO Error: {str(e)}"
        logging.error(error_msg)
        return False, error_msg
        
    except Exception as e:
        error_msg = f"Unexpected error: {str(e)}"
        logging.error(error_msg)
        return False, error_msg

# Test cases
def test_file_operations():
    # Test reading non-existent file
    success, result = safe_file_operation('read', 'nonexistent.txt')
    print(f"Reading non-existent file: {result}")
    
    # Test writing to read-only file
    with open('readonly.txt', 'w') as f:
        f.write("test")
    os.chmod('readonly.txt', 0o444)  # Make file read-only
    success, result = safe_file_operation('write', 'readonly.txt', 'new content')
    print(f"Writing to read-only file: {result}")
    
    # Test successful operations
    success, result = safe_file_operation('write', 'test.txt', 'Hello, World!')
    print(f"Writing to file: {result}")
    
    success, result = safe_file_operation('read', 'test.txt')
    print(f"Reading from file: {result}")

# Run tests
test_file_operations()
```
</details>

## Exercise 5: Working with Large Files
Practice efficient techniques for handling large files.

1. Create a large text file (1000+ lines)
2. Implement different reading strategies:
   - Read line by line
   - Use generator expressions
   - Use chunked reading
3. Compare memory usage and performance
4. Process the file while reading

<details>
<summary>Solution</summary>

```python
import os
import time
import memory_profiler

# Create a large file
def create_large_file(filename, num_lines=1000):
    with open(filename, 'w') as file:
        for i in range(num_lines):
            file.write(f"This is line {i+1} with some additional text for size.\n")

# Different reading strategies
def read_all_at_once(filename):
    """Read entire file into memory."""
    with open(filename, 'r') as file:
        return file.read()

def read_line_by_line(filename):
    """Read file line by line."""
    lines = []
    with open(filename, 'r') as file:
        for line in file:
            lines.append(line.strip())
    return lines

def read_with_generator(filename):
    """Read file using a generator."""
    with open(filename, 'r') as file:
        for line in file:
            yield line.strip()

def read_in_chunks(filename, chunk_size=1024):
    """Read file in chunks."""
    with open(filename, 'r') as file:
        while True:
            chunk = file.read(chunk_size)
            if not chunk:
                break
            yield chunk

# Test and compare strategies
def compare_strategies(filename):
    # Create test file
    create_large_file(filename)
    file_size = os.path.getsize(filename)
    print(f"File size: {file_size/1024:.2f} KB")
    
    # Test each strategy
    print("\nTesting read_all_at_once:")
    start = time.time()
    content = read_all_at_once(filename)
    print(f"Time: {time.time() - start:.2f} seconds")
    
    print("\nTesting read_line_by_line:")
    start = time.time()
    lines = read_line_by_line(filename)
    print(f"Time: {time.time() - start:.2f} seconds")
    
    print("\nTesting generator:")
    start = time.time()
    for line in read_with_generator(filename):
        pass  # Process line here
    print(f"Time: {time.time() - start:.2f} seconds")
    
    print("\nTesting chunked reading:")
    start = time.time()
    for chunk in read_in_chunks(filename):
        pass  # Process chunk here
    print(f"Time: {time.time() - start:.2f} seconds")

# Run comparison
compare_strategies('large_file.txt')
```
</details>

## Exercise 6: Project Organization
Create a data processing pipeline that demonstrates file I/O best practices.

1. Create a project that:
   - Reads data from multiple source files
   - Processes the data
   - Writes results to different output formats
2. Implement:
   - Proper directory structure
   - Error handling
   - Logging
   - Progress tracking
3. Follow Python file I/O best practices

<details>
<summary>Solution</summary>

```python
import os
import csv
import json
import logging
from datetime import datetime

class DataProcessor:
    def __init__(self, input_dir, output_dir):
        # Setup directories
        self.input_dir = input_dir
        self.output_dir = output_dir
        os.makedirs(output_dir, exist_ok=True)
        
        # Setup logging
        log_file = os.path.join(output_dir, 'processing.log')
        logging.basicConfig(
            filename=log_file,
            level=logging.INFO,
            format='%(asctime)s - %(levelname)s - %(message)s'
        )
        
    def process_files(self):
        """Process all files in input directory."""
        try:
            # Get list of input files
            input_files = [f for f in os.listdir(self.input_dir)
                         if f.endswith('.csv')]
            
            if not input_files:
                raise FileNotFoundError("No CSV files found in input directory")
            
            # Process each file
            all_data = []
            for filename in input_files:
                filepath = os.path.join(self.input_dir, filename)
                data = self.read_csv(filepath)
                processed = self.process_data(data)
                all_data.extend(processed)
                
            # Save results
            self.save_results(all_data)
            logging.info("Processing completed successfully")
            
        except Exception as e:
            logging.error(f"Error during processing: {str(e)}")
            raise
            
    def read_csv(self, filepath):
        """Read and validate CSV file."""
        try:
            with open(filepath, 'r') as file:
                reader = csv.DictReader(file)
                return list(reader)
        except Exception as e:
            logging.error(f"Error reading {filepath}: {str(e)}")
            raise
            
    def process_data(self, data):
        """Process the data (example processing)."""
        processed = []
        for row in data:
            try:
                # Example processing
                processed_row = {
                    'name': row['name'].upper(),
                    'value': float(row['value']),
                    'category': row['category'],
                    'processed_date': datetime.now().isoformat()
                }
                processed.append(processed_row)
            except Exception as e:
                logging.warning(f"Error processing row {row}: {str(e)}")
                continue
        return processed
        
    def save_results(self, data):
        """Save results in multiple formats."""
        # Save as JSON
        json_path = os.path.join(self.output_dir, 'results.json')
        with open(json_path, 'w') as file:
            json.dump(data, file, indent=2)
            
        # Save as CSV
        csv_path = os.path.join(self.output_dir, 'results.csv')
        if data:
            with open(csv_path, 'w', newline='') as file:
                writer = csv.DictWriter(file, fieldnames=data[0].keys())
                writer.writeheader()
                writer.writerows(data)
                
        # Save summary
        summary_path = os.path.join(self.output_dir, 'summary.txt')
        with open(summary_path, 'w') as file:
            file.write(f"Processing completed at: {datetime.now()}\n")
            file.write(f"Total records processed: {len(data)}\n")
            file.write(f"Output files created: results.json, results.csv\n")

# Usage example
if __name__ == "__main__":
    processor = DataProcessor('data/raw', 'data/processed')
    processor.process_files()
```
</details>