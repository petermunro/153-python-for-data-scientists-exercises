# Jupyter Notebooks Tutorial

This tutorial will guide you through using Jupyter Notebooks step by step. Each step has a solution that you can reveal if needed.

## Setup
Before starting, you'll need to install some Python packages. Open your terminal and run:
```bash
conda install jupyter numpy pandas matplotlib
```

## Step 1: Creating Your First Notebook

1. Open Jupyter Notebook by typing `jupyter notebook` in your terminal
2. Create a new Python notebook by clicking "New" → "Python 3"
3. Rename the notebook to "My_First_Notebook" by clicking on "Untitled" at the top

<details>
<summary>Solution</summary>

After launching Jupyter Notebook, you should see:
- A new browser window with the Jupyter dashboard
- Click the "New" button in the top right
- Select "Python 3" from the dropdown
- The notebook name "Untitled" appears at the top - click it to rename

The notebook will have an empty cell ready for your input.
</details>

## Step 2: Understanding Cell Types

1. Click the cell type dropdown (it says "Code" by default)
2. Change it to "Markdown"
3. Enter this title and description in the Markdown cell:
   ```markdown
   # My First Jupyter Notebook

   This notebook demonstrates basic Jupyter functionality.
   ```
4. Create a new cell (click "+") and leave it as "Code"
5. Enter this Python code:
   ```python
   print("Hello, Jupyter!")
   ```

<details>
<summary>Solution</summary>

Your notebook should now have two cells:

Markdown Cell:
```markdown
# My First Jupyter Notebook

This notebook demonstrates basic Jupyter functionality.
```

Code Cell:
```python
print("Hello, Jupyter!")
```

To execute cells:
- Click the "Run" button (play icon), or
- Press Shift + Enter
</details>

## Step 3: Working with Data

1. Create a new code cell
2. Copy and paste this code to create and work with a numpy array:
   ```python
   import numpy as np

   # Create an array of numbers 1 through 5
   my_array = np.array([1, 2, 3, 4, 5])

   # Print the array
   print("Our array:", my_array)
   ```

3. Change the last line to just `my_array` and run the cell. What happens?

4. Create another new code cell.

   ```python
   # Calculate and print the mean
   mean_value = np.mean(my_array)
   print("Mean value:", mean_value)
   ```

5. Run this cell too.

<details>
<summary>Solution</summary>

After running the cells, you should see output like this:
```
Our array: [1 2 3 4 5]
Mean value: 3.0
```

The code:
- Creates a numpy array with numbers 1-5
- Prints the array
- Calculates the mean using np.mean()
- Prints the calculated mean
</details>

## Step 4: Creating a Simple Plot

We will create a simple sine wave plot.

1. Create a new code cell
2. Copy and paste this code to import the necessary libraries:
   ```python
   import matplotlib.pyplot as plt
   import numpy as np
   ```

3. Create a new code cell containing the following code:

   ```python
   # Create the data points
   x = np.linspace(0, 10, 100)
   y = np.sin(x)

   # Create the plot
   plt.figure(figsize=(8, 4))
   plt.plot(x, y, 'b-')  # 'b-' means blue solid line
   plt.title('Sine Wave')
   plt.xlabel('x')
   plt.ylabel('sin(x)')
   plt.grid(True)
   plt.show()
   ```

3. Run the cell.

<details>
<summary>Solution</summary>

The code will create a blue sine wave plot with:
- A figure size of 8x4 inches
- X-axis labeled 'x'
- Y-axis labeled 'sin(x)'
- Title 'Sine Wave'
- Grid lines

Key matplotlib commands explained:
- `plt.figure(figsize=(width, height))`: Sets the size of the plot
- `plt.plot(x, y, 'b-')`: Creates a blue line plot
- `plt.title('text')`: Adds a title
- `plt.xlabel('text')`: Labels the x-axis
- `plt.ylabel('text')`: Labels the y-axis
- `plt.grid(True)`: Adds a grid
- `plt.show()`: Displays the plot
</details>

## Step 5: Creating a Bar Plot

1. Create a new code cell
2. Copy and paste this code to create a bar plot:
   ```python
   import matplotlib.pyplot as plt

   # Create the data
   categories = ['A', 'B', 'C', 'D']
   values = [23, 45, 56, 78]

   # Create the bar plot
   plt.figure(figsize=(8, 4))
   plt.bar(categories, values, color='skyblue')
   plt.title('Sample Bar Plot')
   plt.xlabel('Categories')
   plt.ylabel('Values')
   
   # Add value labels on top of each bar
   for i, v in enumerate(values):
       plt.text(i, v + 1, str(v), ha='center')
       
   plt.show()
   ```

<details>
<summary>Solution</summary>

The code will create a bar plot with:
- Four blue bars representing categories A through D
- Value labels on top of each bar
- X-axis labeled 'Categories'
- Y-axis labeled 'Values'
- Title 'Sample Bar Plot'

Key matplotlib commands for bar plots:
- `plt.bar(x, height, color='color')`: Creates bars with specified color
- `plt.text(x, y, text, ha='center')`: Adds text at specified coordinates
</details>

## Step 6: Using Magic Commands

1. Create code cells containing these magic commands, one in each cell at a time:
   ```python
   # Measure execution time of a calculation
   %time sum(range(1000000))
   ```

   ```python
   # List all variables in memory
   %who
   ```

   ```python
   # Ensure plots display in the notebook
   %matplotlib inline
   ```

   ```python
   # Time a more complex operation
   %%time
   total = 0
   for i in range(1000000):
       total += i
   print("Final total:", total)
   ```

<details>
<summary>Solution</summary>

After running each command, you'll see:
- `%time`: Shows CPU and wall time for the operation
- `%who`: Lists all variables you've created
- `%matplotlib inline`: No visible output, but ensures plots display in notebook
- `%%time`: Shows timing for the entire cell

Common magic commands:
- `%time`: Measures execution time of a single line
- `%%time`: Measures execution time of an entire cell
- `%who`: Lists all variables in memory
- `%matplotlib inline`: Ensures plots display in the notebook
</details>

## Step 7: Saving and Exporting

1. Save your notebook (File → Save and Checkpoint)
2. Try exporting to different formats (File → Download as)

<details>
<summary>Solution</summary>

To save your work:
- Use Ctrl/Cmd + S
- Or click File → Save and Checkpoint

To export:
1. Click File → Download as
2. Choose format:
   - HTML (for viewing in a browser)
   - PDF (for printing)
   - Python (.py file)
</details>

Remember:
- Save your work frequently
- Each cell can be run independently
- Cells can be run in any order, but this might affect your results
- The number in brackets [1] shows the order in which cells were executed