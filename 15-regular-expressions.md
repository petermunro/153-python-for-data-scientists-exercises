# Regular Expressions Exercises

## Exercise 1: Basic Pattern Matching
Practice basic regular expression patterns.

1. Create patterns to match:
   - All digits in a string
   - Words that start with 'data'
   - Email addresses
   - Phone numbers in format: XXX-XXX-XXXX
2. Test patterns with sample text
3. Use different regex methods (search, match, findall)

<details>
<summary>Solution</summary>

```python
import re

def test_patterns():
    # Test text
    text = """
    Contact info:
    - Phone: 123-456-7890 and 987-654-3210
    - Email: john.doe@example.com
    - Data points: data123, database, data_science
    - Temperature: 23.5 degrees
    """
    
    # 1. Match digits
    digit_pattern = r'\d+'
    digits = re.findall(digit_pattern, text)
    print("Digits found:", digits)
    
    # 2. Words starting with 'data'
    data_pattern = r'data\w+'
    data_words = re.findall(data_pattern, text)
    print("Data words:", data_words)
    
    # 3. Email addresses
    email_pattern = r'\b[\w\.-]+@[\w\.-]+\.\w+\b'
    emails = re.findall(email_pattern, text)
    print("Email addresses:", emails)
    
    # 4. Phone numbers
    phone_pattern = r'\d{3}-\d{3}-\d{4}'
    phones = re.findall(phone_pattern, text)
    print("Phone numbers:", phones)
    
    # Demonstrate different methods
    # search() - finds first match
    search_result = re.search(phone_pattern, text)
    print("First phone number:", search_result.group() if search_result else "None")
    
    # match() - matches at start of string
    match_result = re.match(r'Contact', text.strip())
    print("Starts with 'Contact':", bool(match_result))

if __name__ == "__main__":
    test_patterns()
```
</details>

## Exercise 2: Data Extraction and Cleaning
Create functions to extract and clean data from text.

1. Create a function to extract all dates in various formats:
   - YYYY-MM-DD
   - MM/DD/YYYY
   - DD-MM-YYYY
2. Create a function to clean and standardize text:
   - Remove special characters
   - Normalize whitespace
   - Convert to lowercase
3. Create a function to extract numerical values with units

<details>
<summary>Solution</summary>

```python
import re
from typing import List, Tuple

def extract_dates(text: str) -> List[Tuple[str, str]]:
    """
    Extract dates in various formats.
    Returns list of tuples (date_string, format_found).
    """
    patterns = {
        r'\d{4}-\d{2}-\d{2}': 'YYYY-MM-DD',
        r'\d{2}/\d{2}/\d{4}': 'MM/DD/YYYY',
        r'\d{2}-\d{2}-\d{4}': 'DD-MM-YYYY'
    }
    
    dates = []
    for pattern, format_name in patterns.items():
        matches = re.finditer(pattern, text)
        dates.extend((match.group(), format_name) for match in matches)
    
    return dates

def clean_text(text: str) -> str:
    """Clean and standardize text."""
    # Remove special characters (keep alphanumeric and whitespace)
    text = re.sub(r'[^\w\s]', '', text)
    
    # Normalize whitespace
    text = re.sub(r'\s+', ' ', text)
    
    # Convert to lowercase and strip
    return text.lower().strip()

def extract_measurements(text: str) -> List[Tuple[float, str]]:
    """
    Extract numerical values with units.
    Returns list of tuples (value, unit).
    """
    # Pattern for number (including decimals) followed by unit
    pattern = r'(\d*\.?\d+)\s*(kg|km|cm|m|°C|mph|GB|MB)\b'
    
    matches = re.finditer(pattern, text)
    return [(float(match.group(1)), match.group(2)) 
            for match in matches]

def test_data_processing():
    # Test text
    text = """
    Important dates:
    - Meeting: 2024-01-15
    - Due date: 03/15/2024
    - Event: 25-12-2024
    
    Measurements:
    - Weight: 75.5 kg
    - Distance: 10.2 km
    - Temperature: 23.5 °C
    - Storage: 500 GB
    
    This is some @#$% messy text    with irregular    spacing!
    """
    
    # Test date extraction
    print("Dates found:")
    dates = extract_dates(text)
    for date, format_name in dates:
        print(f"- {date} ({format_name})")
    
    # Test text cleaning
    print("\nCleaned text:")
    cleaned = clean_text(text)
    print(cleaned)
    
    # Test measurement extraction
    print("\nMeasurements found:")
    measurements = extract_measurements(text)
    for value, unit in measurements:
        print(f"- {value} {unit}")

if __name__ == "__main__":
    test_data_processing()
```
</details>

## Exercise 3: Pattern Validation
Create validation functions using regular expressions.

1. Create validators for:
   - Email addresses (comprehensive)
   - URLs
   - Strong passwords
   - Credit card numbers
2. Include proper error messages
3. Use pattern compilation for efficiency
4. Add unit tests

<details>
<summary>Solution</summary>

```python
import re
from dataclasses import dataclass
from typing import Optional

@dataclass
class ValidationResult:
    is_valid: bool
    message: str

class Validator:
    """Validator class with compiled regex patterns."""
    
    # Email pattern with comprehensive validation
    EMAIL_PATTERN = re.compile(r'''
        ^                     # Start of string
        [\w\.-]+             # Username (letters, digits, dots, hyphens)
        @                     # @ symbol
        [\w\.-]+             # Domain name
        \.[a-zA-Z]{2,}       # Domain suffix (at least 2 chars)
        $                     # End of string
    ''', re.VERBOSE)
    
    # URL pattern
    URL_PATTERN = re.compile(r'''
        ^                     # Start of string
        https?://            # Protocol (http or https)
        (?:                  # Non-capturing group for domain
            [\w-]+           # Subdomain
            \.               # Dot
        )*                   # Zero or more subdomains
        [\w-]+              # Main domain name
        \.                  # Dot
        [a-zA-Z]{2,}        # TLD
        (?:                 # Non-capturing group for path
            /[\w/.-]*       # Path components
        )?                  # Path is optional
        $                   # End of string
    ''', re.VERBOSE)
    
    # Strong password pattern
    PASSWORD_PATTERN = re.compile(r'''
        ^                    # Start of string
        (?=.*[A-Z])         # At least one uppercase letter
        (?=.*[a-z])         # At least one lowercase letter
        (?=.*\d)            # At least one digit
        (?=.*[@$!%*?&])     # At least one special character
        [\w@$!%*?&]{8,}     # Total length at least 8
        $                   # End of string
    ''', re.VERBOSE)
    
    # Credit card pattern (simplified for common formats)
    CREDIT_CARD_PATTERN = re.compile(r'''
        ^                    # Start of string
        (?:
            4\d{15}         # Visa
            |               # OR
            5[1-5]\d{14}    # MasterCard
            |               # OR
            3[47]\d{13}     # American Express
            |               # OR
            6(?:011|5\d{2})\d{12}  # Discover
        )
        $                   # End of string
    ''', re.VERBOSE)
    
    @classmethod
    def validate_email(cls, email: str) -> ValidationResult:
        """Validate email address."""
        if not email:
            return ValidationResult(False, "Email cannot be empty")
        
        if len(email) > 254:  # Maximum length for email addresses
            return ValidationResult(False, "Email is too long")
        
        if cls.EMAIL_PATTERN.match(email):
            return ValidationResult(True, "Valid email address")
        
        return ValidationResult(False, "Invalid email format")
    
    @classmethod
    def validate_url(cls, url: str) -> ValidationResult:
        """Validate URL."""
        if not url:
            return ValidationResult(False, "URL cannot be empty")
        
        if cls.URL_PATTERN.match(url):
            return ValidationResult(True, "Valid URL")
        
        return ValidationResult(False, "Invalid URL format")
    
    @classmethod
    def validate_password(cls, password: str) -> ValidationResult:
        """Validate password strength."""
        if not password:
            return ValidationResult(False, "Password cannot be empty")
        
        if len(password) < 8:
            return ValidationResult(False, "Password must be at least 8 characters")
        
        if not cls.PASSWORD_PATTERN.match(password):
            return ValidationResult(False, 
                "Password must contain uppercase, lowercase, "
                "digit, and special character")
        
        return ValidationResult(True, "Valid password")
    
    @classmethod
    def validate_credit_card(cls, card_number: str) -> ValidationResult:
        """Validate credit card number."""
        # Remove any spaces or hyphens
        card_number = re.sub(r'[\s-]', '', card_number)
        
        if not card_number:
            return ValidationResult(False, "Card number cannot be empty")
        
        if not card_number.isdigit():
            return ValidationResult(False, "Card number must contain only digits")
        
        if cls.CREDIT_CARD_PATTERN.match(card_number):
            return ValidationResult(True, "Valid card number")
        
        return ValidationResult(False, "Invalid card number format")

def test_validators():
    """Test the validation functions."""
    # Test cases
    test_data = {
        'emails': [
            'user@example.com',           # Valid
            'invalid.email@',             # Invalid
            'user.name@domain.co.uk',     # Valid
            '@invalid.com'                # Invalid
        ],
        'urls': [
            'https://www.example.com',    # Valid
            'http://sub.domain.co.uk',    # Valid
            'invalid.com',                # Invalid
            'https://invalid'             # Invalid
        ],
        'passwords': [
            'Abcd123!@#',                # Valid
            'weakpass',                  # Invalid
            'NoDigits!',                 # Invalid
            'Ab1!defgh'                  # Valid
        ],
        'credit_cards': [
            '4111111111111111',          # Valid Visa
            '5555555555554444',          # Valid MasterCard
            '1234567890123456',          # Invalid
            '378282246310005'            # Valid American Express
        ]
    }
    
    # Test each validator
    print("Testing Email Validation:")
    for email in test_data['emails']:
        result = Validator.validate_email(email)
        print(f"Email: {email}")
        print(f"Result: {result}\n")
    
    print("\nTesting URL Validation:")
    for url in test_data['urls']:
        result = Validator.validate_url(url)
        print(f"URL: {url}")
        print(f"Result: {result}\n")
    
    print("\nTesting Password Validation:")
    for password in test_data['passwords']:
        result = Validator.validate_password(password)
        print(f"Password: {'*' * len(password)}")
        print(f"Result: {result}\n")
    
    print("\nTesting Credit Card Validation:")
    for card in test_data['credit_cards']:
        result = Validator.validate_credit_card(card)
        print(f"Card: {'*' * (len(card)-4)}{card[-4:]}")
        print(f"Result: {result}\n")

if __name__ == "__main__":
    test_validators()
```
</details>

## Exercise 4: Text Analysis
Create a text analysis tool using regular expressions.

1. Create functions to analyze text:
   - Word frequency (excluding common words)
   - Sentence structure patterns
   - Named entity recognition (simple)
   - Code snippet extraction
2. Generate statistics and patterns
3. Visualize results

<details>
<summary>Solution</summary>

```python
import re
from collections import Counter
from typing import Dict, List, Set, Tuple
import matplotlib.pyplot as plt

class TextAnalyzer:
    """Text analysis using regular expressions."""
    
    # Common English words to exclude
    COMMON_WORDS = set('''
        the and to a of in that is for it with as was be on at by this
        are or not but had have has from they were
    '''.split())
    
    # Patterns for analysis
    WORD_PATTERN = re.compile(r'\b\w+\b')
    SENTENCE_PATTERN = re.compile(r'[.!?]+\s+')
    NAME_PATTERN = re.compile(r'\b[A-Z][a-z]+(?:\s+[A-Z][a-z]+)*\b')
    CODE_PATTERN = re.compile(r'```python\s*(.*?)\s*```', re.DOTALL)
    
    def __init__(self, text: str):
        self.text = text
        self.words = self._extract_words()
        self.sentences = self._extract_sentences()
    
    def _extract_words(self) -> List[str]:
        """Extract all words from text."""
        return [word.lower() for word in self.WORD_PATTERN.findall(self.text)]
    
    def _extract_sentences(self) -> List[str]:
        """Split text into sentences."""
        return [s.strip() for s in self.SENTENCE_PATTERN.split(self.text)]
    
    def word_frequency(self, exclude_common: bool = True) -> Counter:
        """Calculate word frequency."""
        if exclude_common:
            words = [w for w in self.words if w not in self.COMMON_WORDS]
        else:
            words = self.words
        return Counter(words)
    
    def sentence_patterns(self) -> Dict[str, int]:
        """Analyze sentence structure patterns."""
        patterns = {}
        
        for sentence in self.sentences:
            # Simplified pattern: count words and presence of specific elements
            words = len(sentence.split())
            has_comma = ',' in sentence
            has_quotes = '"' in sentence or "'" in sentence
            
            pattern = f"{'long' if words > 10 else 'short'}"
            pattern += f"{'+comma' if has_comma else ''}"
            pattern += f"{'+quotes' if has_quotes else ''}"
            
            patterns[pattern] = patterns.get(pattern, 0) + 1
        
        return patterns
    
    def extract_names(self) -> Set[str]:
        """Extract potential named entities."""
        return set(self.NAME_PATTERN.findall(self.text))
    
    def extract_code(self) -> List[str]:
        """Extract Python code snippets."""
        return self.CODE_PATTERN.findall(self.text)
    
    def visualize_frequencies(self, top_n: int = 10):
        """Visualize word frequencies."""
        freq = self.word_frequency()
        most_common = freq.most_common(top_n)
        
        words, counts = zip(*most_common)
        
        plt.figure(figsize=(12, 6))
        plt.bar(words, counts)
        plt.xticks(rotation=45, ha='right')
        plt.title('Word Frequency Distribution')
        plt.xlabel('Words')
        plt.ylabel('Frequency')
        plt.tight_layout()
        plt.show()

def test_text_analysis():
    # Sample text for analysis
    text = """
    Python is a powerful programming language. Dr. Smith and Prof. Johnson
    are leading researchers in data science! They work at Tech University.
    
    Here's a simple code example:
    ```python
    def greet(name):
        return f"Hello, {name}!"
    ```
    
    The researchers published their findings in Nature. "This is a significant
    breakthrough," said Dr. Smith. The team's analysis showed promising results.
    """
    
    # Create analyzer
    analyzer = TextAnalyzer(text)
    
    # Word frequency
    print("Word Frequencies:")
    for word, count in analyzer.word_frequency().most_common(5):
        print(f"  {word}: {count}")
    
    # Sentence patterns
    print("\nSentence Patterns:")
    for pattern, count in analyzer.sentence_patterns().items():
        print(f"  {pattern}: {count}")
    
    # Named entities
    print("\nExtracted Names:")
    for name in analyzer.extract_names():
        print(f"  {name}")
    
    # Code snippets
    print("\nCode Snippets:")
    for i, snippet in enumerate(analyzer.extract_code(), 1):
        print(f"\nSnippet {i}:")
        print(snippet)
    
    # Visualize
    analyzer.visualize_frequencies()

if __name__ == "__main__":
    test_text_analysis()
```
</details>

## Exercise 5: Log File Parser
Create a log file parser using regular expressions.

1. Parse different types of log entries:
   - Error messages
   - Warning messages
   - Info messages
   - Timestamps
   - IP addresses
2. Generate statistics and reports
3. Identify patterns and anomalies

<details>
<summary>Solution</summary>

```python
import re
from datetime import datetime
from collections import defaultdict
from dataclasses import dataclass
from typing import List, Dict, Optional

@dataclass
class LogEntry:
    """Represents a parsed log entry."""
    timestamp: datetime
    level: str
    message: str
    ip: Optional[str] = None
    user: Optional[str] = None
    
    def __str__(self):
        parts = [
            f"[{self.timestamp.isoformat()}]",
            f"[{self.level}]"
        ]
        if self.ip:
            parts.append(f"[{self.ip}]")
        if self.user:
            parts.append(f"[{self.user}]")
        parts.append(self.message)
        return " ".join(parts)

class LogParser:
    """Parser for log files using regular expressions."""
    
    # Patterns for log components
    TIMESTAMP_PATTERN = re.compile(
        r'(\d{4}-\d{2}-\d{2}\s+\d{2}:\d{2}:\d{2}(?:\.\d{3})?)'
    )
    LEVEL_PATTERN = re.compile(r'\b(ERROR|WARNING|INFO|DEBUG)\b')
    IP_PATTERN = re.compile(r'\b(\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3})\b')
    USER_PATTERN = re.compile(r'user[=:]([^\s]+)')
    
    def __init__(self):
        self.entries: List[LogEntry] = []
        self.statistics: Dict[str, int] = defaultdict(int)
    
    def parse_file(self, filepath: str) -> None:
        """Parse a log file."""
        with open(filepath, 'r') as file:
            for line in file:
                if entry := self.parse_line(line):
                    self.entries.append(entry)
                    self.update_statistics(entry)
    
    def parse_line(self, line: str) -> Optional[LogEntry]:
        """Parse a single log line."""
        # Extract timestamp
        timestamp_match = self.TIMESTAMP_PATTERN.search(line)
        if not timestamp_match:
            return None
        
        timestamp = datetime.strptime(
            timestamp_match.group(1),
            '%Y-%m-%d %H:%M:%S.%f' if '.' in timestamp_match.group(1)
            else '%Y-%m-%d %H:%M:%S'
        )
        
        # Extract log level
        level_match = self.LEVEL_PATTERN.search(line)
        if not level_match:
            return None
        level = level_match.group(1)
        
        # Extract optional components
        ip_match = self.IP_PATTERN.search(line)
        ip = ip_match.group(1) if ip_match else None
        
        user_match = self.USER_PATTERN.search(line)
        user = user_match.group(1) if user_match else None
        
        # Extract message (everything after timestamp and level)
        message_start = max(
            m.end() for m in [timestamp_match, level_match]
            if m is not None
        )
        message = line[message_start:].strip()
        
        return LogEntry(timestamp, level, message, ip, user)
    
    def update_statistics(self, entry: LogEntry) -> None:
        """Update statistics for a log entry."""
        self.statistics['total_entries'] += 1
        self.statistics[f'level_{entry.level}'] += 1
        
        if entry.ip:
            self.statistics['entries_with_ip'] += 1
            self.statistics[f'ip_{entry.ip}'] += 1
        
        if entry.user:
            self.statistics['entries_with_user'] += 1
            self.statistics[f'user_{entry.user}'] += 1
        
        hour = entry.timestamp.hour
        self.statistics[f'hour_{hour:02d}'] += 1
    
    def generate_report(self) -> str:
        """Generate a report of log analysis."""
        report = ["Log Analysis Report", "=" * 20, ""]
        
        # Basic statistics
        report.extend([
            "Basic Statistics:",
            f"Total entries: {self.statistics['total_entries']}",
            "\nLog Levels:",
        ])
        
        for level in ['ERROR', 'WARNING', 'INFO', 'DEBUG']:
            count = self.statistics.get(f'level_{level}', 0)
            if count:
                report.append(f"- {level}: {count}")
        
        # IP address statistics
        ip_entries = {
            k: v for k, v in self.statistics.items()
            if k.startswith('ip_')
        }
        if ip_entries:
            report.extend([
                "\nIP Addresses:",
                f"Total IPs: {self.statistics['entries_with_ip']}"
            ])
            for ip, count in sorted(ip_entries.items(), key=lambda x: x[1], reverse=True)[:5]:
                report.append(f"- {ip[3:]}: {count}")
        
        # User statistics
        user_entries = {
            k: v for k, v in self.statistics.items()
            if k.startswith('user_')
        }
        if user_entries:
            report.extend([
                "\nUsers:",
                f"Total users: {self.statistics['entries_with_user']}"
            ])
            for user, count in sorted(user_entries.items(), key=lambda x: x[1], reverse=True)[:5]:
                report.append(f"- {user[5:]}: {count}")
        
        # Hourly distribution
        report.extend(["\nHourly Distribution:"])
        for hour in range(24):
            count = self.statistics.get(f'hour_{hour:02d}', 0)
            if count:
                report.append(f"- {hour:02d}:00: {count}")
        
        return "\n".join(report)

def test_log_parser():
    # Create sample log file
    sample_log = """
2024-01-15 10:30:45.123 INFO [192.168.1.1] User login successful user=alice
2024-01-15 10:31:12.456 WARNING [192.168.1.2] High memory usage
2024-01-15 10:32:00.789 ERROR [192.168.1.1] Database connection failed user=alice
2024-01-15 10:33:15.012 INFO [192.168.1.3] Cache cleared
2024-01-15 11:30:45.123 ERROR [192.168.1.2] Out of memory user=bob
    """.strip()
    
    with open('sample.log', 'w') as f:
        f.write(sample_log)
    
    # Parse log file
    parser = LogParser()
    parser.parse_file('sample.log')
    
    # Print parsed entries
    print("Parsed Log Entries:")
    for entry in parser.entries:
        print(entry)
    
    # Print report
    print("\n" + parser.generate_report())

if __name__ == "__main__":
    test_log_parser()
```
</details>