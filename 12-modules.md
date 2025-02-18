# Python Modules Exercises

## Exercise 1: Creating Basic Modules
Create and use your own Python modules.

1. Create a module `calculator.py` with basic math operations:
   - Addition
   - Subtraction
   - Multiplication
   - Division
   - A constant PI
2. Create a test script that imports and uses your module
3. Add proper documentation to your module

<details>
<summary>Solution</summary>

```python
# calculator.py
"""
A simple calculator module providing basic mathematical operations.

This module demonstrates basic module creation and documentation.
"""

PI = 3.14159

def add(a, b):
    """Add two numbers."""
    return a + b

def subtract(a, b):
    """Subtract b from a."""
    return a - b

def multiply(a, b):
    """Multiply two numbers."""
    return a * b

def divide(a, b):
    """
    Divide a by b.
    
    Args:
        a (number): Numerator
        b (number): Denominator
        
    Returns:
        float: Result of division
        
    Raises:
        ZeroDivisionError: If b is zero
    """
    if b == 0:
        raise ZeroDivisionError("Cannot divide by zero")
    return a / b

# Test code that runs only when module is run directly
if __name__ == "__main__":
    print(f"Testing {__name__}")
    print(f"2 + 3 = {add(2, 3)}")
    print(f"5 - 2 = {subtract(5, 2)}")
    print(f"4 * 6 = {multiply(4, 6)}")
    print(f"8 / 2 = {divide(8, 2)}")
```

Test script:
```python
# test_calculator.py
import calculator

# Test basic operations
print(f"PI = {calculator.PI}")
print(f"2 + 3 = {calculator.add(2, 3)}")
print(f"5 - 2 = {calculator.subtract(5, 2)}")
print(f"4 * 6 = {calculator.multiply(4, 6)}")
print(f"8 / 2 = {calculator.divide(8, 2)}")

# Test error handling
try:
    calculator.divide(5, 0)
except ZeroDivisionError as e:
    print(f"Caught error: {e}")
```
</details>

## Exercise 2: Module Import Techniques
Practice different ways of importing and using modules.

1. Create a module `data_processor.py` with functions for data processing:
   - calculate_mean
   - calculate_median
   - calculate_std
   - normalize_data
2. Create multiple test files that import the module using different techniques:
   - Import entire module
   - Import specific functions
   - Import with alias
   - Import all (demonstrate why this isn't recommended)

<details>
<summary>Solution</summary>

```python
# data_processor.py
import statistics
import math

def calculate_mean(data):
    """Calculate the mean of a list of numbers."""
    return sum(data) / len(data)

def calculate_median(data):
    """Calculate the median of a list of numbers."""
    sorted_data = sorted(data)
    n = len(sorted_data)
    mid = n // 2
    if n % 2 == 0:
        return (sorted_data[mid-1] + sorted_data[mid]) / 2
    return sorted_data[mid]

def calculate_std(data):
    """Calculate the standard deviation of a list of numbers."""
    mean = calculate_mean(data)
    squared_diff = [(x - mean) ** 2 for x in data]
    return math.sqrt(sum(squared_diff) / len(data))

def normalize_data(data):
    """Normalize data to have mean 0 and std 1."""
    mean = calculate_mean(data)
    std = calculate_std(data)
    return [(x - mean) / std for x in data]
```

Test files:

```python
# test_import_1.py - Import entire module
import data_processor

data = [1, 2, 3, 4, 5]
print(f"Mean: {data_processor.calculate_mean(data)}")
print(f"Median: {data_processor.calculate_median(data)}")
```

```python
# test_import_2.py - Import specific functions
from data_processor import calculate_mean, calculate_median

data = [1, 2, 3, 4, 5]
print(f"Mean: {calculate_mean(data)}")
print(f"Median: {calculate_median(data)}")
```

```python
# test_import_3.py - Import with alias
import data_processor as dp

data = [1, 2, 3, 4, 5]
print(f"Mean: {dp.calculate_mean(data)}")
print(f"Std: {dp.calculate_std(data)}")
```

```python
# test_import_4.py - Import all (not recommended)
from data_processor import *

# Problems this can cause:
# 1. Unclear where functions come from
# 2. Potential name conflicts
# 3. Importing unnecessary names
data = [1, 2, 3, 4, 5]
print(f"Mean: {calculate_mean(data)}")  # Where did calculate_mean come from?
```
</details>

## Exercise 3: Creating a Package
Create a package for data analysis with multiple modules.

1. Create a package structure:
   ```
   data_analysis/
   ├── __init__.py
   ├── statistics.py
   ├── visualization.py
   └── utils.py
   ```
2. Implement appropriate functionality in each module
3. Use `__init__.py` to expose main functionality
4. Create example usage scripts

<details>
<summary>Solution</summary>

```python
# data_analysis/__init__.py
"""
Data Analysis Package

This package provides tools for statistical analysis and data visualization.
"""

from .statistics import calculate_mean, calculate_median, calculate_std
from .visualization import plot_histogram, plot_scatter
from .utils import load_data, save_data

__version__ = '0.1.0'
```

```python
# data_analysis/statistics.py
"""Statistical analysis functions."""

def calculate_mean(data):
    """Calculate mean of a dataset."""
    return sum(data) / len(data)

def calculate_median(data):
    """Calculate median of a dataset."""
    sorted_data = sorted(data)
    n = len(sorted_data)
    mid = n // 2
    if n % 2 == 0:
        return (sorted_data[mid-1] + sorted_data[mid]) / 2
    return sorted_data[mid]

def calculate_std(data):
    """Calculate standard deviation of a dataset."""
    mean = calculate_mean(data)
    squared_diff = [(x - mean) ** 2 for x in data]
    return (sum(squared_diff) / len(data)) ** 0.5
```

```python
# data_analysis/visualization.py
"""Data visualization functions."""

import matplotlib.pyplot as plt

def plot_histogram(data, bins=10, title="Histogram"):
    """Plot histogram of data."""
    plt.figure()
    plt.hist(data, bins=bins)
    plt.title(title)
    plt.xlabel("Value")
    plt.ylabel("Frequency")
    plt.show()

def plot_scatter(x, y, title="Scatter Plot"):
    """Create scatter plot."""
    plt.figure()
    plt.scatter(x, y)
    plt.title(title)
    plt.xlabel("X")
    plt.ylabel("Y")
    plt.show()
```

```python
# data_analysis/utils.py
"""Utility functions for data handling."""

import json

def load_data(filepath):
    """Load data from JSON file."""
    with open(filepath, 'r') as f:
        return json.load(f)

def save_data(data, filepath):
    """Save data to JSON file."""
    with open(filepath, 'w') as f:
        json.dump(data, f, indent=2)
```

Usage example:
```python
# example_usage.py
from data_analysis import calculate_mean, plot_histogram, load_data

# Load and analyze data
data = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
mean = calculate_mean(data)
print(f"Mean: {mean}")

# Create visualization
plot_histogram(data, bins=5, title="Data Distribution")
```
</details>

## Exercise 4: Module Properties and Reloading
Practice working with module properties and reloading modules during development.

1. Create a module that:
   - Has proper documentation
   - Includes version information
   - Has author information
   - Contains example usage
2. Create a development script that:
   - Imports the module
   - Displays module properties
   - Demonstrates module reloading
   - Shows how to handle module changes during development

<details>
<summary>Solution</summary>

```python
# mymodule.py
"""
Example module demonstrating module properties and documentation.

This module shows how to properly document and structure a Python module,
including version information, author details, and example usage.

Example:
    >>> import mymodule
    >>> mymodule.greet("Alice")
    'Hello, Alice!'
"""

__version__ = '0.1.0'
__author__ = 'Your Name'
__email__ = 'your.email@example.com'

def greet(name):
    """
    Generate a greeting message.
    
    Args:
        name (str): Name to greet
        
    Returns:
        str: Greeting message
    """
    return f"Hello, {name}!"

def calculate(x, y):
    """Perform calculation."""
    return x * y

# Example usage
if __name__ == "__main__":
    print(greet("World"))
```

Development script:
```python
# development.py
import importlib
import mymodule

def explore_module(module):
    """Explore module properties."""
    print(f"\nExploring module: {module.__name__}")
    print(f"File location: {module.__file__}")
    print(f"Documentation:\n{module.__doc__}")
    print(f"Version: {module.__version__}")
    print(f"Author: {module.__author__}")
    
    # List all attributes
    print("\nModule attributes:")
    for attr in dir(module):
        if not attr.startswith('__'):
            print(f"  {attr}")

# Initial exploration
explore_module(mymodule)

# Use the module
print("\nTesting module:")
print(mymodule.greet("Alice"))

# Simulate development changes
input("\nNow modify mymodule.py and press Enter to reload...")

# Reload the module
importlib.reload(mymodule)
print("\nAfter reloading:")
print(mymodule.greet("Alice"))
```
</details>

## Exercise 5: Module Organization Challenge
Create a complete project that demonstrates proper module organization.

1. Create a data analysis project with the following structure:
   ```
   data_analysis_project/
   ├── data_analysis/
   │   ├── __init__.py
   │   ├── core/
   │   │   ├── __init__.py
   │   │   ├── statistics.py
   │   │   └── processing.py
   │   ├── visualization/
   │   │   ├── __init__.py
   │   │   ├── plots.py
   │   │   └── charts.py
   │   └── utils/
   │       ├── __init__.py
   │       ├── io.py
   │       └── validation.py
   ├── tests/
   │   ├── __init__.py
   │   ├── test_statistics.py
   │   └── test_processing.py
   ├── examples/
   │   ├── basic_analysis.py
   │   └── advanced_visualization.py
   └── setup.py
   ```
2. Implement core functionality
3. Create proper package initialization
4. Write example scripts
5. Include tests

<details>
<summary>Solution</summary>

```python
# data_analysis/core/statistics.py
"""Core statistical functions."""

import numpy as np

def describe_data(data):
    """Generate descriptive statistics."""
    return {
        'mean': np.mean(data),
        'median': np.median(data),
        'std': np.std(data),
        'min': np.min(data),
        'max': np.max(data)
    }

# data_analysis/core/processing.py
"""Data processing functions."""

def clean_data(data):
    """Remove invalid values and normalize data."""
    return [x for x in data if x is not None]

# data_analysis/visualization/plots.py
"""Plotting functions."""

import matplotlib.pyplot as plt

def create_histogram(data, title="Distribution"):
    plt.hist(data)
    plt.title(title)
    plt.show()

# data_analysis/utils/io.py
"""Input/output utilities."""

import json

def load_json(filepath):
    """Load data from JSON file."""
    with open(filepath) as f:
        return json.load(f)

# data_analysis/__init__.py
"""
Data Analysis Package

A comprehensive package for data analysis and visualization.
"""

from .core.statistics import describe_data
from .core.processing import clean_data
from .visualization.plots import create_histogram
from .utils.io import load_json

__version__ = '0.1.0'

# examples/basic_analysis.py
from data_analysis import describe_data, create_histogram

data = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
stats = describe_data(data)
print("Statistics:", stats)
create_histogram(data)

# tests/test_statistics.py
import unittest
from data_analysis.core.statistics import describe_data

class TestStatistics(unittest.TestCase):
    def test_describe_data(self):
        data = [1, 2, 3, 4, 5]
        stats = describe_data(data)
        self.assertEqual(stats['mean'], 3)
        self.assertEqual(stats['median'], 3)
```

Setup file:
```python
# setup.py
from setuptools import setup, find_packages

setup(
    name="data_analysis",
    version="0.1.0",
    packages=find_packages(),
    install_requires=[
        'numpy',
        'matplotlib'
    ],
    author="Your Name",
    author_email="your.email@example.com",
    description="A data analysis package",
    long_description=open('README.md').read(),
    long_description_content_type="text/markdown",
    url="https://github.com/yourusername/data_analysis",
    classifiers=[
        "Programming Language :: Python :: 3",
        "License :: OSI Approved :: MIT License",
        "Operating System :: OS Independent",
    ],
    python_requires='>=3.6',
)
```
</details>