# Python Standard Library Exercises

## Exercise 1: Command Line Arguments with argparse
Create a command-line tool for data processing.

1. Create a script that processes data files with these arguments:
   - Input file (required)
   - Output file (required)
   - Verbose mode (optional flag)
   - Format type (optional, choices: 'json', 'csv', 'txt')
   - Batch size (optional, with default value)
2. Add appropriate help messages
3. Implement error handling for invalid arguments

<details>
<summary>Solution</summary>

```python
import argparse
import sys

def process_data(input_file, output_file, format_type, batch_size, verbose):
    """Simulate data processing."""
    if verbose:
        print(f"Processing {input_file} -> {output_file}")
        print(f"Format: {format_type}, Batch size: {batch_size}")
    # Actual processing would go here
    return True

def main():
    # Create argument parser
    parser = argparse.ArgumentParser(
        description='Process data files with various options.',
        formatter_class=argparse.ArgumentDefaultsHelpFormatter
    )
    
    # Add arguments
    parser.add_argument('input_file',
                       help='Input file to process')
    parser.add_argument('output_file',
                       help='Output file to create')
    parser.add_argument('-v', '--verbose',
                       action='store_true',
                       help='Increase output verbosity')
    parser.add_argument('-f', '--format',
                       choices=['json', 'csv', 'txt'],
                       default='csv',
                       help='Output format type')
    parser.add_argument('-b', '--batch-size',
                       type=int,
                       default=1000,
                       help='Processing batch size')
    
    # Parse arguments
    args = parser.parse_args()
    
    # Use the arguments
    try:
        success = process_data(
            args.input_file,
            args.output_file,
            args.format,
            args.batch_size,
            args.verbose
        )
        if success and args.verbose:
            print("Processing completed successfully")
    except Exception as e:
        print(f"Error: {e}", file=sys.stderr)
        return 1
    
    return 0

if __name__ == "__main__":
    sys.exit(main())
```

Usage examples:
```bash
# Basic usage
python script.py input.dat output.csv

# With options
python script.py -v -f json -b 500 input.dat output.json

# Show help
python script.py --help
```
</details>

## Exercise 2: System Operations
Create utility scripts using os and sys modules.

1. Create a script that:
   - Lists all Python files in the current directory and subdirectories
   - Shows their size and last modification time
   - Filters files modified in the last 24 hours
   - Prints system information
2. Handle different operating systems appropriately
3. Use proper path manipulation

<details>
<summary>Solution</summary>

```python
import os
import sys
import time
from datetime import datetime, timedelta

def get_system_info():
    """Get system information."""
    info = {
        'Platform': sys.platform,
        'Python Version': sys.version,
        'Current Directory': os.getcwd(),
        'Username': os.getenv('USER') or os.getenv('USERNAME'),
        'Path Separator': os.path.sep,
        'Line Separator': os.linesep,
        'Environment Variables': len(os.environ)
    }
    return info

def format_size(size):
    """Format file size in human-readable format."""
    for unit in ['B', 'KB', 'MB', 'GB']:
        if size < 1024:
            return f"{size:.2f} {unit}"
        size /= 1024
    return f"{size:.2f} TB"

def get_file_info(filepath):
    """Get file information."""
    stats = os.stat(filepath)
    return {
        'path': filepath,
        'size': stats.st_size,
        'modified': datetime.fromtimestamp(stats.st_mtime),
        'created': datetime.fromtimestamp(stats.st_ctime)
    }

def find_python_files(start_path='.', hours=24):
    """Find Python files modified within specified hours."""
    cutoff_time = datetime.now() - timedelta(hours=hours)
    python_files = []
    
    for root, _, files in os.walk(start_path):
        for file in files:
            if file.endswith('.py'):
                filepath = os.path.join(root, file)
                file_info = get_file_info(filepath)
                if file_info['modified'] > cutoff_time:
                    python_files.append(file_info)
    
    return python_files

def main():
    # Print system information
    print("System Information:")
    print("-" * 50)
    for key, value in get_system_info().items():
        print(f"{key}: {value}")
    
    print("\nRecent Python Files:")
    print("-" * 50)
    
    # Find and print Python files
    python_files = find_python_files()
    if not python_files:
        print("No Python files found modified in the last 24 hours")
        return
    
    # Sort by modification time
    python_files.sort(key=lambda x: x['modified'], reverse=True)
    
    # Print file information
    for file_info in python_files:
        print(f"\nFile: {file_info['path']}")
        print(f"Size: {format_size(file_info['size'])}")
        print(f"Modified: {file_info['modified'].strftime('%Y-%m-%d %H:%M:%S')}")

if __name__ == "__main__":
    main()
```
</details>

## Exercise 3: Process Management
Create scripts to manage external processes using subprocess.

1. Create a script that:
   - Runs multiple system commands
   - Captures their output
   - Handles errors
   - Sets timeouts
2. Implement both synchronous and asynchronous execution
3. Process the command output

<details>
<summary>Solution</summary>

```python
import subprocess
import time
import sys
from datetime import datetime

def run_command(command, timeout=None):
    """Run a command and return its output."""
    try:
        result = subprocess.run(
            command,
            capture_output=True,
            text=True,
            timeout=timeout,
            shell=True if sys.platform == 'win32' else False
        )
        return {
            'command': command,
            'returncode': result.returncode,
            'stdout': result.stdout.strip(),
            'stderr': result.stderr.strip(),
            'success': result.returncode == 0
        }
    except subprocess.TimeoutExpired:
        return {
            'command': command,
            'returncode': -1,
            'stdout': '',
            'stderr': f'Command timed out after {timeout} seconds',
            'success': False
        }
    except Exception as e:
        return {
            'command': command,
            'returncode': -1,
            'stdout': '',
            'stderr': str(e),
            'success': False
        }

def run_commands_parallel(commands):
    """Run multiple commands in parallel."""
    processes = []
    for cmd in commands:
        process = subprocess.Popen(
            cmd,
            stdout=subprocess.PIPE,
            stderr=subprocess.PIPE,
            text=True,
            shell=True if sys.platform == 'win32' else False
        )
        processes.append((cmd, process))
    
    results = []
    for cmd, process in processes:
        stdout, stderr = process.communicate()
        results.append({
            'command': cmd,
            'returncode': process.returncode,
            'stdout': stdout.strip(),
            'stderr': stderr.strip(),
            'success': process.returncode == 0
        })
    
    return results

def main():
    # Define commands to run
    commands = [
        ['echo', 'Hello, World!'],
        ['ls', '-l'] if sys.platform != 'win32' else ['dir'],
        ['python', '--version'],
        ['invalid_command'],  # This will fail
        ['sleep', '2'] if sys.platform != 'win32' else ['timeout', '2']
    ]
    
    # Run commands synchronously
    print("Running commands synchronously:")
    print("-" * 50)
    for cmd in commands:
        print(f"\nExecuting: {' '.join(cmd)}")
        result = run_command(cmd, timeout=3)
        print(f"Success: {result['success']}")
        if result['stdout']:
            print(f"Output: {result['stdout']}")
        if result['stderr']:
            print(f"Error: {result['stderr']}")
    
    # Run commands in parallel
    print("\nRunning commands in parallel:")
    print("-" * 50)
    results = run_commands_parallel(commands)
    for result in results:
        print(f"\nCommand: {result['command']}")
        print(f"Success: {result['success']}")
        if result['stdout']:
            print(f"Output: {result['stdout']}")
        if result['stderr']:
            print(f"Error: {result['stderr']}")

if __name__ == "__main__":
    main()
```
</details>

## Exercise 4: File Operations with shutil
Create a backup utility using shutil.

1. Create a script that:
   - Takes a source directory
   - Creates a timestamped backup
   - Compresses the backup
   - Maintains a limited number of backups
2. Add logging
3. Implement error handling
4. Add cleanup of old backups

<details>
<summary>Solution</summary>

```python
import os
import shutil
import logging
from datetime import datetime
import glob

class BackupManager:
    def __init__(self, source_dir, backup_dir, max_backups=5):
        self.source_dir = os.path.abspath(source_dir)
        self.backup_dir = os.path.abspath(backup_dir)
        self.max_backups = max_backups
        
        # Setup logging
        logging.basicConfig(
            level=logging.INFO,
            format='%(asctime)s - %(levelname)s - %(message)s',
            handlers=[
                logging.FileHandler('backup.log'),
                logging.StreamHandler()
            ]
        )
        self.logger = logging.getLogger(__name__)
    
    def create_backup(self):
        """Create a new backup."""
        try:
            # Create backup directory if it doesn't exist
            os.makedirs(self.backup_dir, exist_ok=True)
            
            # Generate backup name with timestamp
            timestamp = datetime.now().strftime('%Y%m%d_%H%M%S')
            backup_name = f"backup_{timestamp}"
            backup_path = os.path.join(self.backup_dir, backup_name)
            
            # Copy files
            self.logger.info(f"Creating backup: {backup_path}")
            shutil.copytree(self.source_dir, backup_path)
            
            # Create archive
            archive_name = f"{backup_path}.zip"
            self.logger.info(f"Creating archive: {archive_name}")
            shutil.make_archive(backup_path, 'zip', backup_path)
            
            # Remove the uncompressed backup
            shutil.rmtree(backup_path)
            
            # Cleanup old backups
            self.cleanup_old_backups()
            
            self.logger.info("Backup completed successfully")
            return True
            
        except Exception as e:
            self.logger.error(f"Backup failed: {str(e)}")
            return False
    
    def cleanup_old_backups(self):
        """Remove old backups if exceeding max_backups."""
        try:
            # Get list of backup archives
            backup_pattern = os.path.join(self.backup_dir, "backup_*.zip")
            backups = sorted(glob.glob(backup_pattern))
            
            # Remove oldest backups if exceeding max_backups
            while len(backups) > self.max_backups:
                oldest_backup = backups.pop(0)
                self.logger.info(f"Removing old backup: {oldest_backup}")
                os.remove(oldest_backup)
                
        except Exception as e:
            self.logger.error(f"Cleanup failed: {str(e)}")
    
    def list_backups(self):
        """List all available backups."""
        try:
            backup_pattern = os.path.join(self.backup_dir, "backup_*.zip")
            backups = sorted(glob.glob(backup_pattern))
            
            self.logger.info(f"Found {len(backups)} backups:")
            for backup in backups:
                size = os.path.getsize(backup)
                modified = datetime.fromtimestamp(os.path.getmtime(backup))
                self.logger.info(f"- {os.path.basename(backup)}")
                self.logger.info(f"  Size: {size/1024/1024:.2f} MB")
                self.logger.info(f"  Modified: {modified}")
                
        except Exception as e:
            self.logger.error(f"Failed to list backups: {str(e)}")

def main():
    # Create backup manager
    manager = BackupManager(
        source_dir="./source",
        backup_dir="./backups",
        max_backups=3
    )
    
    # Create backup
    manager.create_backup()
    
    # List backups
    manager.list_backups()

if __name__ == "__main__":
    main()
```
</details>

## Exercise 5: Temporary Files and Pattern Matching
Create a script that processes files using temporary storage and pattern matching.

1. Create a script that:
   - Finds all files matching a pattern
   - Creates temporary copies for processing
   - Processes the files
   - Saves results
2. Use both tempfile and glob modules
3. Implement proper cleanup
4. Handle different file types

<details>
<summary>Solution</summary>

```python
import os
import tempfile
import glob
import shutil
import json
import csv
from contextlib import contextmanager

class FileProcessor:
    def __init__(self, search_dir):
        self.search_dir = search_dir
    
    @contextmanager
    def temporary_directory(self):
        """Create a temporary directory and clean it up when done."""
        temp_dir = tempfile.mkdtemp()
        try:
            yield temp_dir
        finally:
            shutil.rmtree(temp_dir)
    
    def find_files(self, pattern):
        """Find files matching pattern."""
        return glob.glob(os.path.join(self.search_dir, pattern), recursive=True)
    
    def process_file(self, filepath, temp_dir):
        """Process a single file based on its type."""
        filename = os.path.basename(filepath)
        temp_path = os.path.join(temp_dir, f"temp_{filename}")
        
        # Copy file to temporary location
        shutil.copy2(filepath, temp_path)
        
        # Process based on file type
        if filepath.endswith('.json'):
            return self.process_json(temp_path)
        elif filepath.endswith('.csv'):
            return self.process_csv(temp_path)
        elif filepath.endswith('.txt'):
            return self.process_text(temp_path)
        else:
            return None
    
    def process_json(self, filepath):
        """Process JSON file."""
        with open(filepath, 'r') as f:
            data = json.load(f)
            # Example processing: count items
            return {'type': 'json', 'items': len(data)}
    
    def process_csv(self, filepath):
        """Process CSV file."""
        with open(filepath, 'r') as f:
            reader = csv.reader(f)
            rows = list(reader)
            # Example processing: count rows and columns
            return {
                'type': 'csv',
                'rows': len(rows),
                'columns': len(rows[0]) if rows else 0
            }
    
    def process_text(self, filepath):
        """Process text file."""
        with open(filepath, 'r') as f:
            lines = f.readlines()
            # Example processing: count lines and words
            words = sum(len(line.split()) for line in lines)
            return {
                'type': 'text',
                'lines': len(lines),
                'words': words
            }
    
    def process_files(self, patterns):
        """Process all files matching patterns."""
        results = []
        
        with self.temporary_directory() as temp_dir:
            for pattern in patterns:
                files = self.find_files(pattern)
                for filepath in files:
                    try:
                        result = self.process_file(filepath, temp_dir)
                        if result:
                            results.append({
                                'file': filepath,
                                'results': result
                            })
                    except Exception as e:
                        print(f"Error processing {filepath}: {e}")
        
        return results

def main():
    # Create processor
    processor = FileProcessor("./data")
    
    # Define patterns to search for
    patterns = [
        "**/*.json",
        "**/*.csv",
        "**/*.txt"
    ]
    
    # Process files
    results = processor.process_files(patterns)
    
    # Print results
    print("\nProcessing Results:")
    print("-" * 50)
    for result in results:
        print(f"\nFile: {result['file']}")
        print("Results:")
        for key, value in result['results'].items():
            print(f"  {key}: {value}")

if __name__ == "__main__":
    main()
```
</details>