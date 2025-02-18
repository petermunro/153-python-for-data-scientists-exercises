# Python Exceptions Exercises

## Exercise 1: Basic Exception Handling
Practice basic exception handling patterns.

1. Create functions to handle common exceptions:
   - Division by zero
   - Index out of range
   - Key error in dictionary
   - Type error
2. Implement proper error messages
3. Return appropriate default values

<details>
<summary>Solution</summary>

```python
def safe_divide(a, b):
    """
    Safely divide two numbers.
    
    Args:
        a (number): Numerator
        b (number): Denominator
        
    Returns:
        float or None: Result of division or None if error
    """
    try:
        return a / b
    except ZeroDivisionError:
        print("Error: Division by zero!")
        return None
    except TypeError:
        print("Error: Invalid types for division!")
        return None

def safe_list_get(lst, index, default=None):
    """
    Safely get item from list.
    
    Args:
        lst (list): List to get item from
        index (int): Index to access
        default: Value to return if error occurs
        
    Returns:
        Item from list or default value
    """
    try:
        return lst[index]
    except IndexError:
        print(f"Error: Index {index} is out of range!")
        return default
    except TypeError:
        print("Error: Invalid index type!")
        return default

def safe_dict_get(dct, key, default=None):
    """
    Safely get value from dictionary.
    
    Args:
        dct (dict): Dictionary to get value from
        key: Key to access
        default: Value to return if error occurs
        
    Returns:
        Value from dictionary or default value
    """
    try:
        return dct[key]
    except KeyError:
        print(f"Error: Key '{key}' not found!")
        return default
    except TypeError:
        print("Error: Invalid key type!")
        return default

# Test the functions
def test_exception_handlers():
    # Test safe_divide
    print("\nTesting safe_divide:")
    print(safe_divide(10, 2))      # Should work
    print(safe_divide(10, 0))      # Division by zero
    print(safe_divide(10, "2"))    # Type error
    
    # Test safe_list_get
    print("\nTesting safe_list_get:")
    lst = [1, 2, 3]
    print(safe_list_get(lst, 1))    # Should work
    print(safe_list_get(lst, 5))    # Index error
    print(safe_list_get(lst, "1"))  # Type error
    
    # Test safe_dict_get
    print("\nTesting safe_dict_get:")
    dct = {"a": 1, "b": 2}
    print(safe_dict_get(dct, "a"))     # Should work
    print(safe_dict_get(dct, "c"))     # Key error
    print(safe_dict_get(dct, ["a"]))   # Type error

if __name__ == "__main__":
    test_exception_handlers()
```
</details>

## Exercise 2: Multiple Exception Handlers
Create a data processing function that handles multiple types of errors.

1. Create a function that processes a CSV file:
   - Handle file not found
   - Handle permission errors
   - Handle invalid data format
   - Handle empty file
2. Use multiple except blocks
3. Implement proper error recovery
4. Add logging

<details>
<summary>Solution</summary>

```python
import csv
import logging
from pathlib import Path

# Setup logging
logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s - %(levelname)s - %(message)s'
)

class DataProcessingError(Exception):
    """Custom exception for data processing errors."""
    pass

def process_csv_file(filepath, required_columns=None):
    """
    Process a CSV file with multiple error handlers.
    
    Args:
        filepath (str): Path to CSV file
        required_columns (list): List of required column names
        
    Returns:
        list: Processed data or empty list if error occurs
    """
    if required_columns is None:
        required_columns = []
    
    try:
        # Check if file exists
        if not Path(filepath).exists():
            raise FileNotFoundError(f"File not found: {filepath}")
        
        # Try to open and read file
        with open(filepath, 'r') as file:
            # Check if file is empty
            first_char = file.read(1)
            if not first_char:
                raise DataProcessingError("File is empty")
            
            # Reset file pointer
            file.seek(0)
            
            # Read CSV
            reader = csv.DictReader(file)
            
            # Validate columns
            if required_columns:
                missing_cols = [col for col in required_columns 
                              if col not in reader.fieldnames]
                if missing_cols:
                    raise DataProcessingError(
                        f"Missing required columns: {missing_cols}"
                    )
            
            # Process data
            data = []
            for row_num, row in enumerate(reader, start=1):
                try:
                    # Validate row data
                    processed_row = {
                        key: value.strip() if value else None
                        for key, value in row.items()
                    }
                    data.append(processed_row)
                except Exception as e:
                    logging.warning(f"Error processing row {row_num}: {e}")
                    continue
            
            return data
            
    except FileNotFoundError as e:
        logging.error(f"File error: {e}")
        return []
        
    except PermissionError:
        logging.error(f"Permission denied: {filepath}")
        return []
        
    except csv.Error as e:
        logging.error(f"CSV format error: {e}")
        return []
        
    except DataProcessingError as e:
        logging.error(f"Processing error: {e}")
        return []
        
    except Exception as e:
        logging.error(f"Unexpected error: {e}")
        return []
        
    finally:
        logging.info("File processing completed")

# Test the function
def test_csv_processing():
    # Test with non-existent file
    print("\nTesting non-existent file:")
    process_csv_file("nonexistent.csv")
    
    # Test with empty file
    print("\nTesting empty file:")
    with open("empty.csv", "w") as f:
        pass
    process_csv_file("empty.csv")
    
    # Test with invalid format
    print("\nTesting invalid format:")
    with open("invalid.csv", "w") as f:
        f.write("invalid,csv,format\nwith,wrong,number,of,columns\n")
    process_csv_file("invalid.csv", required_columns=["id", "name"])
    
    # Test with valid file
    print("\nTesting valid file:")
    with open("valid.csv", "w") as f:
        f.write("id,name,age\n1,Alice,30\n2,Bob,25\n")
    result = process_csv_file("valid.csv", required_columns=["id", "name"])
    print(f"Processed data: {result}")

if __name__ == "__main__":
    test_csv_processing()
```
</details>

## Exercise 3: Context Managers and Resource Cleanup
Practice using context managers and cleanup patterns.

1. Create a context manager for:
   - Database connection
   - File handling
   - Resource locking
2. Implement proper cleanup in finally blocks
3. Handle nested resources
4. Compare with and without context managers

<details>
<summary>Solution</summary>

```python
import contextlib
import threading
import logging
from typing import Optional

class DatabaseConnection:
    """Simulate a database connection."""
    def __init__(self, host: str, port: int):
        self.host = host
        self.port = port
        self.connected = False
    
    def connect(self):
        """Simulate connecting to database."""
        logging.info(f"Connecting to database at {self.host}:{self.port}")
        self.connected = True
    
    def disconnect(self):
        """Simulate disconnecting from database."""
        logging.info(f"Disconnecting from database at {self.host}:{self.port}")
        self.connected = False
    
    def query(self, sql: str) -> list:
        """Simulate database query."""
        if not self.connected:
            raise RuntimeError("Not connected to database")
        logging.info(f"Executing query: {sql}")
        return []  # Simulate empty result set

class ResourceLock:
    """Simulate a resource lock."""
    def __init__(self, name: str):
        self.name = name
        self.lock = threading.Lock()
    
    def acquire(self):
        """Acquire the lock."""
        logging.info(f"Acquiring lock: {self.name}")
        return self.lock.acquire()
    
    def release(self):
        """Release the lock."""
        logging.info(f"Releasing lock: {self.name}")
        self.lock.release()

@contextlib.contextmanager
def database_connection(host: str, port: int) -> DatabaseConnection:
    """Context manager for database connection."""
    conn = DatabaseConnection(host, port)
    try:
        conn.connect()
        yield conn
    finally:
        conn.disconnect()

@contextlib.contextmanager
def resource_lock(name: str) -> ResourceLock:
    """Context manager for resource lock."""
    lock = ResourceLock(name)
    try:
        lock.acquire()
        yield lock
    finally:
        lock.release()

class FileProcessor:
    """Process files with proper resource management."""
    
    def __init__(self, input_path: str, output_path: str):
        self.input_path = input_path
        self.output_path = output_path
        self.input_file: Optional[file] = None
        self.output_file: Optional[file] = None
    
    def __enter__(self):
        """Enter context manager."""
        try:
            self.input_file = open(self.input_path, 'r')
            self.output_file = open(self.output_path, 'w')
            return self
        except Exception:
            # Clean up if either file fails to open
            if self.input_file:
                self.input_file.close()
            if self.output_file:
                self.output_file.close()
            raise
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        """Exit context manager."""
        try:
            if self.input_file:
                self.input_file.close()
        finally:
            if self.output_file:
                self.output_file.close()
    
    def process(self):
        """Process input file and write to output file."""
        for line in self.input_file:
            processed = line.strip().upper()
            self.output_file.write(processed + '\n')

def demonstrate_resource_management():
    """Demonstrate different resource management approaches."""
    
    # 1. Without context manager
    print("\nWithout context manager:")
    db = DatabaseConnection("localhost", 5432)
    try:
        db.connect()
        db.query("SELECT * FROM users")
    finally:
        db.disconnect()
    
    # 2. With context manager
    print("\nWith context manager:")
    with database_connection("localhost", 5432) as db:
        db.query("SELECT * FROM users")
    
    # 3. Nested context managers
    print("\nNested context managers:")
    with database_connection("localhost", 5432) as db:
        with resource_lock("database") as lock:
            db.query("SELECT * FROM users")
    
    # 4. File processing
    print("\nFile processing:")
    # Create test file
    with open("input.txt", "w") as f:
        f.write("line 1\nline 2\nline 3\n")
    
    # Process file
    with FileProcessor("input.txt", "output.txt") as processor:
        processor.process()
    
    # Show results
    with open("output.txt", "r") as f:
        print("Processed contents:")
        print(f.read())

if __name__ == "__main__":
    logging.basicConfig(level=logging.INFO)
    demonstrate_resource_management()
```
</details>

## Exercise 4: Custom Exceptions
Create custom exceptions for a data validation system.

1. Create a hierarchy of custom exceptions:
   - Base validation exception
   - Type-specific exceptions
   - Domain-specific exceptions
2. Implement validation functions
3. Use exception chaining
4. Add proper exception messages and attributes

<details>
<summary>Solution</summary>

```python
class ValidationError(Exception):
    """Base class for validation errors."""
    def __init__(self, message, field=None):
        self.field = field
        self.message = message
        super().__init__(f"{field}: {message}" if field else message)

class TypeValidationError(ValidationError):
    """Raised when value is of wrong type."""
    def __init__(self, field, expected_type, actual_type):
        self.expected_type = expected_type
        self.actual_type = actual_type
        message = f"Expected type {expected_type.__name__}, got {actual_type.__name__}"
        super().__init__(message, field)

class RangeValidationError(ValidationError):
    """Raised when value is outside allowed range."""
    def __init__(self, field, min_value, max_value, actual_value):
        self.min_value = min_value
        self.max_value = max_value
        self.actual_value = actual_value
        message = f"Value {actual_value} outside range [{min_value}, {max_value}]"
        super().__init__(message, field)

class FormatValidationError(ValidationError):
    """Raised when value format is invalid."""
    def __init__(self, field, pattern, value):
        self.pattern = pattern
        self.value = value
        message = f"Value '{value}' doesn't match pattern '{pattern}'"
        super().__init__(message, field)

class DataValidator:
    """Validate data against schema."""
    
    def __init__(self, schema):
        self.schema = schema
    
    def validate(self, data):
        """Validate data against schema."""
        errors = []
        
        for field, rules in self.schema.items():
            try:
                if field not in data:
                    if rules.get('required', False):
                        raise ValidationError("Field is required", field)
                    continue
                
                value = data[field]
                self._validate_field(field, value, rules)
                
            except ValidationError as e:
                errors.append(e)
        
        if errors:
            raise ValidationError(
                "Multiple validation errors occurred",
                errors=errors
            )
    
    def _validate_field(self, field, value, rules):
        """Validate a single field."""
        # Type validation
        expected_type = rules.get('type')
        if expected_type and not isinstance(value, expected_type):
            raise TypeValidationError(field, expected_type, type(value))
        
        # Range validation
        if 'min' in rules or 'max' in rules:
            min_val = rules.get('min', float('-inf'))
            max_val = rules.get('max', float('inf'))
            if not min_val <= value <= max_val:
                raise RangeValidationError(field, min_val, max_val, value)
        
        # Format validation
        if 'pattern' in rules and not rules['pattern'].match(str(value)):
            raise FormatValidationError(field, rules['pattern'], value)
        
        # Custom validation
        if 'validate' in rules:
            try:
                rules['validate'](value)
            except Exception as e:
                raise ValidationError(str(e), field) from e

def test_validation():
    """Test the validation system."""
    import re
    
    # Define schema
    schema = {
        'name': {
            'type': str,
            'required': True,
            'pattern': re.compile(r'^[A-Za-z\s]+$')
        },
        'age': {
            'type': int,
            'min': 0,
            'max': 120
        },
        'email': {
            'type': str,
            'pattern': re.compile(r'^[\w\.-]+@[\w\.-]+\.\w+$')
        },
        'score': {
            'type': float,
            'min': 0,
            'max': 100,
            'validate': lambda x: x != 50  # Custom validation
        }
    }
    
    validator = DataValidator(schema)
    
    # Test valid data
    valid_data = {
        'name': 'John Doe',
        'age': 30,
        'email': 'john@example.com',
        'score': 85.5
    }
    
    try:
        validator.validate(valid_data)
        print("Valid data passed validation")
    except ValidationError as e:
        print(f"Unexpected validation error: {e}")
    
    # Test invalid data
    invalid_data = {
        'name': 'John123',  # Invalid format
        'age': 150,         # Out of range
        'email': 'invalid', # Invalid email
        'score': 50         # Fails custom validation
    }
    
    try:
        validator.validate(invalid_data)
    except ValidationError as e:
        print("\nValidation errors:")
        if hasattr(e, 'errors'):
            for error in e.errors:
                print(f"- {error}")
        else:
            print(f"- {e}")

if __name__ == "__main__":
    test_validation()
```
</details>

## Exercise 5: LBYL vs EAFP
Compare LBYL and EAFP approaches in different scenarios.

1. Implement both approaches for:
   - Dictionary access
   - File operations
   - Type conversions
2. Compare performance
3. Analyze code readability
4. Handle race conditions

<details>
<summary>Solution</summary>

```python
import time
import json
from pathlib import Path
from typing import Any, Dict, Optional

class PerformanceTimer:
    """Context manager for timing code execution."""
    def __init__(self, description: str):
        self.description = description
        self.start_time: float = 0
        
    def __enter__(self):
        self.start_time = time.time()
        return self
        
    def __exit__(self, exc_type, exc_val, exc_tb):
        duration = time.time() - self.start_time
        print(f"{self.description}: {duration:.6f} seconds")

class DataProcessor:
    """Compare LBYL and EAFP approaches."""
    
    def __init__(self, data: Dict[str, Any]):
        self.data = data
    
    # Dictionary Access
    def get_value_lbyl(self, key: str, default: Any = None) -> Any:
        """Get value using LBYL approach."""
        if isinstance(self.data, dict):
            if key in self.data:
                if self.data[key] is not None:
                    return self.data[key]
        return default
    
    def get_value_eafp(self, key: str, default: Any = None) -> Any:
        """Get value using EAFP approach."""
        try:
            return self.data[key]
        except (TypeError, KeyError):
            return default
    
    # File Operations
    @staticmethod
    def read_json_lbyl(filepath: str) -> Optional[Dict]:
        """Read JSON file using LBYL approach."""
        path = Path(filepath)
        if not path.exists():
            print("File doesn't exist")
            return None
        
        if not path.is_file():
            print("Path is not a file")
            return None
            
        if path.suffix != '.json':
            print("Not a JSON file")
            return None
            
        with open(filepath, 'r') as f:
            content = f.read()
            if not content:
                print("File is empty")
                return None
                
            return json.loads(content)
    
    @staticmethod
    def read_json_eafp(filepath: str) -> Optional[Dict]:
        """Read JSON file using EAFP approach."""
        try:
            with open(filepath, 'r') as f:
                return json.load(f)
        except FileNotFoundError:
            print("File doesn't exist")
        except PermissionError:
            print("Permission denied")
        except json.JSONDecodeError:
            print("Invalid JSON format")
        except Exception as e:
            print(f"Unexpected error: {e}")
        return None
    
    # Type Conversions
    @staticmethod
    def convert_to_int_lbyl(value: Any) -> Optional[int]:
        """Convert value to int using LBYL approach."""
        if isinstance(value, int):
            return value
        if isinstance(value, str):
            if value.isdigit():
                return int(value)
        if isinstance(value, float):
            if value.is_integer():
                return int(value)
        return None
    
    @staticmethod
    def convert_to_int_eafp(value: Any) -> Optional[int]:
        """Convert value to int using EAFP approach."""
        try:
            return int(value)
        except (TypeError, ValueError):
            return None

def compare_approaches():
    """Compare LBYL and EAFP approaches."""
    # Setup test data
    data = {
        'name': 'Alice',
        'age': '25',
        'score': 95.0,
        'meta': None
    }
    processor = DataProcessor(data)
    
    # Test dictionary access
    print("\nDictionary Access:")
    test_keys = ['name', 'age', 'missing', 'meta']
    
    with PerformanceTimer("LBYL Dictionary"):
        for key in test_keys * 1000:
            processor.get_value_lbyl(key)
    
    with PerformanceTimer("EAFP Dictionary"):
        for key in test_keys * 1000:
            processor.get_value_eafp(key)
    
    # Test file operations
    print("\nFile Operations:")
    # Create test JSON file
    with open('test.json', 'w') as f:
        json.dump({'test': 'data'}, f)
    
    with PerformanceTimer("LBYL File"):
        for _ in range(100):
            processor.read_json_lbyl('test.json')
    
    with PerformanceTimer("EAFP File"):
        for _ in range(100):
            processor.read_json_eafp('test.json')
    
    # Test type conversions
    print("\nType Conversions:")
    test_values = [42, "123", 45.0, "abc", None]
    
    with PerformanceTimer("LBYL Conversion"):
        for value in test_values * 1000:
            processor.convert_to_int_lbyl(value)
    
    with PerformanceTimer("EAFP Conversion"):
        for value in test_values * 1000:
            processor.convert_to_int_eafp(value)
    
    # Demonstrate race condition handling
    print("\nRace Condition Example:")
    
    # LBYL - Vulnerable to race conditions
    if Path('test.json').exists():
        try:
            # File might be deleted here by another process
            with open('test.json') as f:
                data = json.load(f)
        except FileNotFoundError:
            print("LBYL: Race condition demonstrated!")
    
    # EAFP - Handles race conditions gracefully
    try:
        with open('test.json') as f:
            data = json.load(f)
    except FileNotFoundError:
        print("EAFP: Handled missing file gracefully")

if __name__ == "__main__":
    compare_approaches()
```
</details>