# Python Best Practices: From Basics to Advanced 🐍

A comprehensive guide to help you build secure, maintainable, and scalable Python projects with clear examples!

## Table of Contents 📑

### Basic Practices 🔰
1. [Project Setup](#project-setup-)
   - [Virtual Environments](#virtual-environments-)
   - [Requirements Management](#requirements-management-)
   - [Environment Variables](#environment-variables-)
2. [Code Organization](#code-organization-)
   - [Project Structure](#project-structure-)
   - [Modules and Packages](#modules-and-packages-)
   - [Constants and Configuration](#constants-and-configuration-)
3. [Security Best Practices](#security-best-practices-)
   - [Password and Secret Management](#password-and-secret-management-)
   - [Input Validation](#input-validation-)
   - [SQL Injection Prevention](#sql-injection-prevention-)
   - [Dependency Security](#dependency-security-)
4. [Testing Your Code](#testing-your-code-)
   - [Unit Testing](#unit-testing-)
   - [Test Coverage](#test-coverage-)
   - [Mock and Patch](#mock-and-patch-)
5. [Documentation](#documentation-)
   - [Code Comments](#code-comments-)
   - [README File](#readme-file-)

### Advanced Practices 🚀
6. [Scalability Considerations](#scalability-considerations-)
   - [Data Structures and Algorithms](#data-structures-and-algorithms-)
   - [Avoid Global Variables](#avoid-global-variables-)
   - [Generator Functions](#generator-functions-)
   - [Caching Results](#caching-results-)
7. [Deployment Tips](#deployment-tips-)
   - [Containerization with Docker](#containerization-with-docker-)
   - [Continuous Integration (CI)](#continuous-integration-ci-)
8. [Advanced Topics](#advanced-topics-)
   - [Type Hints](#type-hints-)
   - [Asynchronous Programming](#asynchronous-programming-)
   - [Context Managers](#context-managers-)
   - [Decorators](#decorators-)
   - [Error Handling](#error-handling-)
   - [Performance Optimization](#performance-optimization-)
   - [Design Patterns](#design-patterns-)
   - [Packaging and Distribution](#packaging-and-distribution-)

## Project Setup 🚀

### Virtual Environments 🏝️

#### What is a Virtual Environment? 🤔
A virtual environment is like a separate box for each of your Python projects. Each box has its own Python and its own packages, so projects don't interfere with each other.

#### Do's ✅
- **DO** create a virtual environment for every project
- **DO** activate your virtual environment before installing packages
- **DO** keep your virtual environment folder in your project directory
- **DO** add your virtual environment folder to `.gitignore`

#### Don'ts ❌
- **DON'T** install packages globally when they're only needed for one project
- **DON'T** share virtual environments between projects
- **DON'T** include virtual environment folders in version control
- **DON'T** forget to activate your virtual environment when working on your project

#### Step-by-Step Example 👣

**Step 1: Install virtualenv if you don't have it**
```bash
pip install virtualenv
```

**Step 2: Create a virtual environment in your project folder**
```bash
# Navigate to your project folder
cd my_project

# Create a virtual environment named 'venv'
python -m venv venv
```

**Step 3: Activate the virtual environment**

On Windows:
```bash
venv\Scripts\activate
```

On Mac/Linux:
```bash
source venv/bin/activate
```

**Step 4: Verify activation**
```bash
# You should see (venv) at the beginning of your command line
# Check where Python is running from
which python  # On Mac/Linux
where python  # On Windows
```

**Step 5: Deactivate when done**
```bash
deactivate
```

### Requirements Management 📋

#### What is Requirements Management? 🤔
Requirements management is keeping track of all the packages (libraries) your project needs to run properly.

#### Do's ✅
- **DO** maintain an updated `requirements.txt` file
- **DO** specify version numbers for critical dependencies
- **DO** organize requirements into development and production sets if needed
- **DO** regularly update dependencies for security fixes

#### Don'ts ❌
- **DON'T** manually edit `requirements.txt` (use pip commands instead)
- **DON'T** install unnecessary packages
- **DON'T** use very old package versions with known security issues
- **DON'T** specify exact versions unless necessary (use `>=` for minor updates)

#### Step-by-Step Example 👣

**Step 1: Install packages in your virtual environment**
```bash
# Make sure your virtual environment is activated
pip install requests pandas matplotlib
```

**Step 2: Create a requirements.txt file**
```bash
pip freeze > requirements.txt
```

**Step 3: Examine the file**
```bash
# The file will contain package names and versions like:
# requests==2.28.1
# pandas==1.5.3
# matplotlib==3.7.1
```

**Step 4: Install from requirements.txt in a new environment**
```bash
pip install -r requirements.txt
```

**Step 5: Create separate requirements files (optional)**
```bash
# requirements-dev.txt for development tools
pip install pytest black flake8
pip freeze > requirements-dev.txt

# Edit requirements.txt to keep only production dependencies
```

### Environment Variables ⚙️

#### What are Environment Variables? 🤔
Environment variables are like secret notes you can access in your code without writing them directly in your code.

#### Do's ✅
- **DO** use environment variables for secrets and configuration
- **DO** use `.env` files for local development
- **DO** add `.env` to your `.gitignore` file
- **DO** provide a sample `.env.example` file with dummy values

#### Don'ts ❌
- **DON'T** commit `.env` files to your repository
- **DON'T** hardcode sensitive information in your code
- **DON'T** use environment variables for non-sensitive configuration that rarely changes
- **DON'T** store complicated data structures in environment variables

#### Step-by-Step Example 👣

**Step 1: Create a `.env` file in your project root**
```bash
# In a file named .env
API_KEY=your_secret_key_here
DATABASE_URL=postgres://username:password@localhost/dbname
DEBUG=True
MAX_CONNECTIONS=5
```

**Step 2: Create a `.env.example` file for others**
```bash
# In a file named .env.example
API_KEY=your_api_key_here
DATABASE_URL=postgres://username:password@localhost/dbname
DEBUG=False
MAX_CONNECTIONS=5
```

**Step 3: Add `.env` to `.gitignore`**
```bash
# In a file named .gitignore
.env
__pycache__/
venv/
```

**Step 4: Install python-dotenv**
```bash
pip install python-dotenv
```

**Step 5: Load environment variables in your code**
```python
# In a file named config.py
import os
from dotenv import load_dotenv

# Load environment variables from .env file
load_dotenv()

# Access environment variables
API_KEY = os.getenv("API_KEY")
DATABASE_URL = os.getenv("DATABASE_URL")
DEBUG = os.getenv("DEBUG", "False") == "True"  # Convert string to boolean
MAX_CONNECTIONS = int(os.getenv("MAX_CONNECTIONS", "3"))  # Convert string to int

# Validate required environment variables
if not API_KEY:
    raise ValueError("API_KEY environment variable is not set!")
```

**Step 6: Use the config in other files**
```python
# In a file named api_client.py
from config import API_KEY

def make_api_request(endpoint):
    headers = {"Authorization": f"Bearer {API_KEY}"}
    # Make request with headers
```

## Code Organization 📂

### Project Structure 🏗️

#### What is Project Structure? 🤔
Project structure is how you organize your files and folders to make your code easy to navigate and maintain.

#### Do's ✅
- **DO** organize related files into modules and packages
- **DO** follow a consistent naming convention
- **DO** separate code from tests and documentation
- **DO** use descriptive names for files and folders

#### Don'ts ❌
- **DON'T** put everything in one file
- **DON'T** mix code with data files
- **DON'T** use generic names like `util.py` or `helper.py` without context
- **DON'T** deeply nest folders (try to keep it to 2-3 levels)

#### Step-by-Step Example 👣

**Step 1: Create a basic project structure**
```
my_project/                      # Main project folder
│
├── README.md                    # Project documentation
├── requirements.txt             # Dependencies
├── .env                         # Environment variables (not in git!)
├── .gitignore                   # Files to ignore in git
├── setup.py                     # Package installation info
│
├── my_package/                  # Your main package
│   ├── __init__.py              # Makes folder a package
│   ├── main.py                  # Entry point
│   ├── config.py                # Configuration
│   ├── models/                  # Data models
│   │   ├── __init__.py
│   │   └── user.py
│   ├── utils/                   # Utility functions
│   │   ├── __init__.py
│   │   └── helpers.py
│   └── api/                     # API-related code
│       ├── __init__.py
│       └── client.py
│
├── tests/                       # Test files
│   ├── __init__.py
│   ├── test_main.py
│   └── test_utils.py
│
├── docs/                        # Documentation
│   └── user_guide.md
│
└── scripts/                     # Utility scripts
    └── db_setup.py
```

### Modules and Packages 📦

#### What are Modules and Packages? 🤔
A module is a simple Python file with functions and classes. A package is a folder with multiple modules and an `__init__.py` file.

#### Do's ✅
- **DO** divide your code into logical modules
- **DO** include `__init__.py` in each package directory
- **DO** use relative imports within a package
- **DO** expose key functionality in `__init__.py`

#### Don'ts ❌
- **DON'T** create modules with unrelated functions
- **DON'T** make circular imports between modules
- **DON'T** make modules too large (aim for less than 500 lines)
- **DON'T** import everything with `from module import *`

#### Step-by-Step Example 👣

**Step 1: Create a simple module**
```python
# In my_package/utils/helpers.py
def format_date(date_obj):
    """Format a date object to string."""
    return date_obj.strftime("%Y-%m-%d")

def calculate_tax(amount, rate=0.1):
    """Calculate tax on an amount."""
    return amount * rate
```

**Step 2: Create a package with __init__.py**
```python
# In my_package/utils/__init__.py
from .helpers import format_date, calculate_tax

# You can add version info
__version__ = "0.1.0"
```

**Step 3: Import your modules in other files**
```python
# In my_package/main.py

# Import from a module in the same package
from .utils import format_date, calculate_tax

# OR using relative imports
from .utils.helpers import format_date, calculate_tax

# Import from a different package
from datetime import datetime

def main():
    today = datetime.now()
    formatted_date = format_date(today)
    print(f"Today is {formatted_date}")

    amount = 100
    tax = calculate_tax(amount)
    print(f"Tax on ${amount} is ${tax}")

if __name__ == "__main__":
    main()
```

### Constants and Configuration 🔒

#### What are Constants? 🤔
Constants are values that don't change during your program's execution. Configuration is a collection of settings that control how your program behaves.

#### Do's ✅
- **DO** use UPPERCASE for constant names
- **DO** define constants in a separate module
- **DO** use meaningful names for constants
- **DO** separate configuration from code

#### Don'ts ❌
- **DON'T** change constants during program execution
- **DON'T** hardcode values that might need to change
- **DON'T** repeat the same constant in multiple places
- **DON'T** use magic numbers or strings without explanation

#### Step-by-Step Example 👣

**Step 1: Create a constants module**
```python
# In my_package/constants.py
# Network settings
MAX_RETRY_COUNT = 3
DEFAULT_TIMEOUT = 30
BASE_API_URL = "https://api.example.com/v1"

# File paths
DATA_DIR = "data"
LOG_DIR = "logs"
CONFIG_FILE = "config.json"

# Application settings
MAX_CONNECTIONS = 10
CACHE_TTL = 60 * 60  # 1 hour in seconds
DEFAULT_LANGUAGE = "en"
```

**Step 2: Create a configuration module**
```python
# In my_package/config.py
import os
import json
from dotenv import load_dotenv
from .constants import CONFIG_FILE, DEFAULT_TIMEOUT, DEFAULT_LANGUAGE

# Load environment variables
load_dotenv()

# Configuration class
class Config:
    def __init__(self):
        # Load from environment variables
        self.api_key = os.getenv("API_KEY")
        self.debug = os.getenv("DEBUG", "False") == "True"
        
        # Load from constants with environment overrides
        self.timeout = int(os.getenv("TIMEOUT", str(DEFAULT_TIMEOUT)))
        self.language = os.getenv("LANGUAGE", DEFAULT_LANGUAGE)
        
        # Load from config file if it exists
        self._load_from_file()
    
    def _load_from_file(self):
        if not os.path.exists(CONFIG_FILE):
            return
            
        try:
            with open(CONFIG_FILE, 'r') as f:
                config_data = json.load(f)
                
            # Update config with file values
            for key, value in config_data.items():
                if not hasattr(self, key):
                    setattr(self, key, value)
        except Exception as e:
            print(f"Error loading config file: {e}")

# Create a singleton config object
config = Config()
```

## Security Best Practices 🔐

### Password and Secret Management 🤫

#### What is Secret Management? 🤔
Secret management is keeping sensitive information like passwords and API keys safe and out of your code.

#### Do's ✅
- **DO** use environment variables for secrets
- **DO** use a `.env` file for local development
- **DO** use a secrets manager in production
- **DO** rotate secrets regularly

#### Don'ts ❌
- **DON'T** hardcode secrets in your code
- **DON'T** store secrets in version control
- **DON'T** log secrets
- **DON'T** include secrets in error messages

#### Step-by-Step Example 👣

**Step 1: Store secrets in .env file**
```bash
# In .env file
DATABASE_PASSWORD=super_secret_password
API_KEY=abcdef123456
JWT_SECRET=very_long_random_string
```

**Step 2: Load secrets properly**
```python
# In my_package/database.py
import os
from dotenv import load_dotenv

# Load environment variables
load_dotenv()

def get_database_connection():
    """Get a database connection using environment variables."""
    username = os.getenv("DATABASE_USERNAME", "default_user")
    password = os.getenv("DATABASE_PASSWORD")
    host = os.getenv("DATABASE_HOST", "localhost")
    
    # Check if password is set
    if not password:
        raise ValueError("DATABASE_PASSWORD environment variable is not set!")
    
    # Create connection (example for PostgreSQL)
    connection_string = f"postgresql://{username}:{password}@{host}/mydb"
    
    # GOOD: Password not visible in logs or code
    print(f"Connecting to database at {host} as {username}")
    
    # Connect to database (using psycopg2, SQLAlchemy, etc.)
    # ...
    
    return connection
```

### Input Validation ✅

#### What is Input Validation? 🤔
Input validation is checking data from users or external sources to make sure it's safe and in the correct format.

#### Do's ✅
- **DO** validate all user input
- **DO** use type hints and validation libraries
- **DO** fail early and provide clear error messages
- **DO** validate both type and content

#### Don'ts ❌
- **DON'T** trust user input
- **DON'T** use `eval()` on user input
- **DON'T** perform validation only on the client side
- **DON'T** assume input is always in the expected format

#### Step-by-Step Example 👣

**Step 1: Simple input validation**
```python
def validate_age(age_str):
    """Validate and convert an age string to an integer."""
    # Check if input is provided
    if not age_str:
        raise ValueError("Age cannot be empty")
    
    # Try to convert to integer
    try:
        age = int(age_str)
    except ValueError:
        raise ValueError("Age must be a number")
    
    # Check if age is in a reasonable range
    if age < 0:
        raise ValueError("Age cannot be negative")
    if age > 150:
        raise ValueError("Age is too high")
    
    return age

# Usage:
def process_user_data(name, age_str):
    # Validate name
    if not name:
        raise ValueError("Name cannot be empty")
    if len(name) > 100:
        raise ValueError("Name is too long")
    
    # Validate age
    try:
        age = validate_age(age_str)
        return f"{name} is {age} years old"
    except ValueError as e:
        return f"Error: {str(e)}"
```

### SQL Injection Prevention 💉

#### What is SQL Injection? 🤔
SQL injection is when attackers put SQL commands in user input to mess with your database.

#### Do's ✅
- **DO** use parameterized queries
- **DO** use an ORM (Object-Relational Mapper) like SQLAlchemy
- **DO** validate and sanitize input before use
- **DO** use the principle of least privilege for database users

#### Don'ts ❌
- **DON'T** build SQL with string concatenation
- **DON'T** use user input directly in SQL
- **DON'T** grant excessive database permissions
- **DON'T** store sensitive data without encryption

#### Step-by-Step Example 👣

**BAD example (vulnerable to SQL injection)**
```python
# NEVER DO THIS
def get_user_by_username(username):
    # Create database connection
    conn = get_db_connection()
    cursor = conn.cursor()
    
    # BAD: Direct string formatting
    query = f"SELECT * FROM users WHERE username = '{username}'"
    
    # Execute query
    cursor.execute(query)
    
    # Return results
    result = cursor.fetchone()
    cursor.close()
    conn.close()
    
    return result
```

**GOOD example (using parameterized queries)**
```python
def get_user_by_username(username):
    # Create database connection
    conn = get_db_connection()
    cursor = conn.cursor()
    
    # GOOD: Using parameterized query
    query = "SELECT * FROM users WHERE username = %s"  # For PostgreSQL
    # For SQLite: query = "SELECT * FROM users WHERE username = ?"
    
    # Execute query with parameters
    cursor.execute(query, (username,))
    
    # Return results
    result = cursor.fetchone()
    cursor.close()
    conn.close()
    
    return result
```

### Dependency Security 🔍

#### What is Dependency Security? 🤔
Dependency security is checking that the libraries your project uses don't have known security problems.

#### Do's ✅
- **DO** use tools to scan dependencies for vulnerabilities
- **DO** specify version constraints for dependencies
- **DO** update dependencies regularly
- **DO** use a lockfile to ensure consistent dependency versions

#### Don'ts ❌
- **DON'T** use abandoned packages
- **DON'T** ignore security warnings
- **DON'T** use packages with a history of security issues
- **DON'T** install unnecessary dependencies

#### Step-by-Step Example 👣

**Step 1: Specify version constraints in requirements.txt**
```
# In requirements.txt
# Specify minimum versions
requests>=2.25.0
flask>=2.0.0

# Pin exact versions for critical dependencies
cryptography==38.0.1
pyjwt==2.4.0

# Specify compatible versions (major.minor.*)
sqlalchemy~=1.4.0
```

**Step 2: Check for vulnerabilities**
```bash
# Install safety package
pip install safety

# Check installed packages
safety check

# Check requirements file
safety check -r requirements.txt
```

## Testing Your Code 🧪

### Unit Testing 🔬

#### What is Unit Testing? 🤔
Unit testing is testing small parts of your code (like functions) to make sure they work correctly.

#### Do's ✅
- **DO** write tests for your functions
- **DO** use a testing framework like pytest or unittest
- **DO** test edge cases and error conditions
- **DO** keep tests independent

#### Don'ts ❌
- **DON'T** skip testing
- **DON'T** test only the happy path (successful cases)
- **DON'T** write tests that depend on other tests
- **DON'T** test external services directly

#### Step-by-Step Example 👣

**Step 1: Create a module to test**
```python
# In my_package/math_utils.py
def add(a, b):
    """Add two numbers."""
    return a + b

def divide(a, b):
    """Divide a by b."""
    if b == 0:
        raise ValueError("Cannot divide by zero")
    return a / b
```

**Step 2: Write tests using pytest**
```python
# In tests/test_math_utils_pytest.py
import pytest
from my_package.math_utils import add, divide

# Test add function
def test_add_positive_numbers():
    assert add(2, 3) == 5

def test_add_negative_numbers():
    assert add(-1, -1) == -2
    assert add(-1, 1) == 0

# Test divide function
def test_divide_basic():
    assert divide(10, 2) == 5
    assert divide(5, 2) == 2.5

def test_divide_by_zero():
    with pytest.raises(ValueError, match="Cannot divide by zero"):
        divide(10, 0)
```

### Test Coverage 📊

#### What is Test Coverage? 🤔
Test coverage measures how much of your code is tested by your test suite.

#### Do's ✅
- **DO** aim for high test coverage
- **DO** use coverage tools
- **DO** focus on testing complex logic
- **DO** test boundary conditions

#### Don'ts ❌
- **DON'T** obsess over 100% coverage
- **DON'T** write tests just to increase coverage
- **DON'T** ignore uncovered critical code
- **DON'T** test trivial code

#### Step-by-Step Example 👣

**Step 1: Install coverage tools**
```bash
# Install coverage and pytest-cov
pip install coverage pytest-cov
```

**Step 2: Run tests with coverage**
```bash
# Using pytest-cov
pytest --cov=my_package
```

**Step 3: Generate coverage report**
```bash
# Generate an HTML report
coverage html
```

### Mock and Patch 🎭

#### What is Mocking? 🤔
Mocking replaces real objects with fake ones for testing, especially for external services.

#### Do's ✅
- **DO** mock external dependencies
- **DO** use `unittest.mock` or `pytest-mock`
- **DO** mock at the appropriate level
- **DO** verify mock calls

#### Don'ts ❌
- **DON'T** mock the system under test
- **DON'T** create overly complex mocks
- **DON'T** mock everything
- **DON'T** forget to verify important mock interactions

#### Step-by-Step Example 👣

**Step 1: Create a module that has external dependencies**
```python
# In my_package/user_service.py
import requests

def get_user(user_id):
    """Get user data from API."""
    response = requests.get(f"https://api.example.com/users/{user_id}")
    response.raise_for_status()
    return response.json()
```

**Step 2: Test with pytest-mock**
```python
# In tests/test_user_service_pytest.py
def test_get_user(mocker):
    """Test get_user function using pytest-mock."""
    # Setup the mock
    mock_get = mocker.patch("my_package.user_service.requests.get")
    mock_response = mocker.Mock()
    mock_response.json.return_value = {"id": 123, "name": "Test User"}
    mock_response.raise_for_status.return_value = None
    mock_get.return_value = mock_response
    
    # Call the function
    result = get_user(123)
    
    # Assertions
    mock_get.assert_called_once_with("https://api.example.com/users/123")
    assert result == {"id": 123, "name": "Test User"}
```

## Documentation 📝

### Code Comments 💬

#### What are Good Comments? 🤔
Good comments explain why code exists, not just what it does.

#### Do's ✅
- **DO** write docstrings for modules, classes, and functions
- **DO** explain complex or non-obvious code
- **DO** use comments to explain "why," not "what"
- **DO** keep comments up-to-date with code changes

#### Don'ts ❌
- **DON'T** write comments for obvious code
- **DON'T** use comments to hide bad code
- **DON'T** leave outdated comments
- **DON'T** write unclear or ambiguous comments

#### Step-by-Step Example 👣

**Step 1: Write good docstrings**
```python
"""
User management module.

This module provides functions for managing users in the system,
including creation, retrieval, and authentication.
"""

def get_user_by_id(user_id):
    """
    Get a user by their ID.
    
    Args:
        user_id (int): The unique identifier of the user.
        
    Returns:
        dict: The user data as a dictionary, or None if not found.
        
    Raises:
        ValueError: If user_id is not a positive integer.
        ConnectionError: If the database is not accessible.
    """
    if not isinstance(user_id, int) or user_id <= 0:
        raise ValueError("User ID must be a positive integer")
    
    # Query database
    # ...
    
    return user_data
```

### README File 📖

#### What is a Good README? 🤔
A good README is like a map for your project. It helps new people understand what your project does and how to use it.

#### Do's ✅
- **DO** include a clear project description
- **DO** provide installation and usage instructions
- **DO** document common use cases
- **DO** mention requirements and dependencies

#### Don'ts ❌
- **DON'T** assume users know everything about your project
- **DON'T** skip setup steps
- **DON'T** neglect to mention important limitations
- **DON'T** leave outdated instructions

#### Step-by-Step Example 👣

**Step 1: Create a basic README.md**
```markdown
# My Awesome Project

A Python utility for processing and analyzing customer data.

## Features

- Fast data import from CSV, Excel, and JSON
- Customer segmentation based on purchase history
- Automatic report generation
- Data visualization with interactive charts

## Installation

```bash
# Clone the repository
git clone https://github.com/username/my-awesome-project.git
cd my-awesome-project

# Create and activate virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

## Usage

```python
from my_package import CustomerAnalyzer

# Initialize the analyzer
analyzer = CustomerAnalyzer('data.csv')

# Perform segmentation
segments = analyzer.segment_customers()

# Generate report
analyzer.generate_report('report.pdf', segments)
```

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
```

## Scalability Considerations 📈

### Data Structures and Algorithms 🧠

#### What are Efficient Data Structures? 🤔
Using the right data structure helps your program run faster and use less memory, especially when working with large amounts of data.

#### Do's ✅
- **DO** choose the right data structure for your needs
- **DO** consider time and space complexity
- **DO** use built-in data structures when possible
- **DO** optimize for your most common operations

#### Don'ts ❌
- **DON'T** use lists for frequent lookups by key
- **DON'T** use dictionaries when order matters (before Python 3.7)
- **DON'T** prematurely optimize
- **DON'T** reinvent built-in data structures

#### Step-by-Step Example 👣

```python
# BAD: Using a list for frequent lookups
def find_user_by_id_bad(users, user_id):
    """Find a user by ID in a list of users."""
    for user in users:
        if user['id'] == user_id:
            return user
    return None

# GOOD: Using a dictionary for lookups
def build_user_lookup(users):
    """Build a lookup dictionary of users by ID."""
    return {user['id']: user for user in users}

def find_user_by_id_good(user_lookup, user_id):
    """Find a user by ID using a lookup dictionary."""
    return user_lookup.get(user_id)
```

### Avoid Global Variables 🌐

#### What are Global Variables? 🤔
Global variables are variables that can be accessed from anywhere in your program, but they can make your code hard to understand and test.

#### Do's ✅
- **DO** use function parameters and return values
- **DO** use classes to manage state
- **DO** use dependency injection
- **DO** use closures for maintaining state when appropriate

#### Don'ts ❌
- **DON'T** use global variables for state
- **DON'T** modify global variables from functions
- **DON'T** use global variables to share data between functions
- **DON'T** access function variables from outside the function

#### Step-by-Step Example 👣

```python
# BAD: Using global variables
total = 0

def add_to_total(value):
    global total
    total += value
    return total

# GOOD: Using return values
def add_values(current_total, value):
    return current_total + value

# GOOD: Using classes to manage state
class Counter:
    def __init__(self, initial_value=0):
        self.count = initial_value
    
    def increment(self):
        self.count += 1
        return self.count
    
    def get_count(self):
        return self.count
```

### Generator Functions 🔄

#### What are Generator Functions? 🤔
Generators are functions that can pause and resume, which helps save memory when working with large datasets.

#### Do's ✅
- **DO** use generators for processing large data
- **DO** use generators for infinite sequences
- **DO** create pipeline of generators for data processing
- **DO** prefer lazy evaluation when possible

#### Don'ts ❌
- **DON'T** use generators when you need to use the data multiple times
- **DON'T** create a generator when a simple list is sufficient
- **DON'T** assume generators will process all items immediately
- **DON'T** try to get the length of a generator (convert to list first)

#### Step-by-Step Example 👣

```python
# BAD: Loading all data into memory
def read_large_file_bad(file_path):
    """Read all lines from a file into memory."""
    with open(file_path, 'r') as file:
        return file.readlines()  # All lines loaded into memory at once

# GOOD: Using a generator to read line by line
def read_large_file_good(file_path):
    """Read a large file line by line using a generator."""
    with open(file_path, 'r') as file:
        for line in file:
            yield line.strip()  # One line at a time
```

### Caching Results 🗃️

#### What is Caching? 🤔
Caching saves the results of expensive operations so you don't have to calculate them again.

#### Do's ✅
- **DO** cache results of expensive operations
- **DO** use `functools.lru_cache` for simple function caching
- **DO** set appropriate cache sizes and expiration
- **DO** cache at the appropriate level (function, request, application)

#### Don'ts ❌
- **DON'T** cache everything (only expensive operations)
- **DON'T** cache results that change frequently
- **DON'T** forget to invalidate cache when data changes
- **DON'T** use mutable objects as cache keys

#### Step-by-Step Example 👣

```python
from functools import lru_cache

# BAD: Expensive calculation without caching
def fibonacci_bad(n):
    """Calculate Fibonacci numbers without caching."""
    if n <= 1:
        return n
    return fibonacci_bad(n-1) + fibonacci_bad(n-2)

# GOOD: With caching
@lru_cache(maxsize=128)
def fibonacci_good(n):
    """Calculate Fibonacci numbers with caching."""
    if n <= 1:
        return n
    return fibonacci_good(n-1) + fibonacci_good(n-2)
```

## Deployment Tips 🚢

### Containerization with Docker 🐳

#### What is Docker? 🤔
Docker helps you package your application and all its dependencies into a "container" so it works the same on any computer.

#### Do's ✅
- **DO** use official base images
- **DO** use specific version tags
- **DO** create minimal images
- **DO** document required environment variables

#### Don'ts ❌
- **DON'T** run as root
- **DON'T** include sensitive data in images
- **DON'T** install unnecessary packages
- **DON'T** use the `latest` tag in production

#### Step-by-Step Example 👣

**Step 1: Create a basic Dockerfile**
```dockerfile
# Use an official Python runtime as the base image
FROM python:3.9-slim

# Set working directory
WORKDIR /app

# Copy requirements file
COPY requirements.txt .

# Install dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Copy application code
COPY . .

# Run as a non-root user
RUN useradd -m appuser
USER appuser

# Command to run the application
CMD ["python", "-m", "my_package"]
```

### Continuous Integration (CI) 🔄

#### What is CI? 🤔
Continuous Integration automatically tests your code whenever you make changes, so you catch problems early.

#### Do's ✅
- **DO** run tests on every commit
- **DO** check code style and quality
- **DO** use multiple Python versions
- **DO** automate dependency security checks

#### Don'ts ❌
- **DON'T** run long tests on every small change
- **DON'T** ignore failing tests
- **DON'T** commit directly to main branch
- **DON'T** leave CI broken for long periods

#### Step-by-Step Example 👣

**Step 1: Create a GitHub Actions workflow file**
```yaml
# .github/workflows/python-app.yml
name: Python Application

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        python-version: [3.7, 3.8, 3.9]

    steps:
    - uses: actions/checkout@v2
    
    - name: Set up Python ${{ matrix.python-version }}
      uses: actions/setup-python@v2
      with:
        python-version: ${{ matrix.python-version }}
    
    - name: Install dependencies
      run: |
        python -m pip install --upgrade pip
        if [ -f requirements-dev.txt ]; then pip install -r requirements-dev.txt; fi
        if [ -f requirements.txt ]; then pip install -r requirements.txt; fi
        pip install -e .
    
    - name: Test with pytest
      run: |
        pytest --cov=my_package
```

## Advanced Topics 🧠

### Type Hints 📝

#### What are Type Hints? 🤔
Type hints tell Python what kind of data a function expects and returns.

#### Do's ✅
- **DO** use type hints for function parameters and return values
- **DO** use type hints for complex data structures
- **DO** use `Optional` for values that could be `None`
- **DO** use type checkers like mypy

#### Don'ts ❌
- **DON'T** use type hints everywhere (focus on public interfaces)
- **DON'T** make code overly complex just to satisfy type checking
- **DON'T** forget to use the `typing` module
- **DON'T** ignore type checker warnings

#### Step-by-Step Example 👣

```python
from typing import List, Dict, Optional, Union

# Function with type hints
def process_items(items: List[str]) -> Dict[str, int]:
    """Process a list of items and return counts."""
    result = {}
    for item in items:
        if item in result:
            result[item] += 1
        else:
            result[item] = 1
    return result

# Optional parameters
def get_user_info(user_id: int, include_profile: Optional[bool] = None) -> Dict[str, Union[str, int]]:
    """Get user information, optionally including profile data."""
    info = {"id": user_id, "username": f"user_{user_id}"}
    
    if include_profile:
        info["profile"] = "User profile data"
    
    return info
```

### Asynchronous Programming ⚡

#### What is Async Programming? 🤔
Asynchronous programming lets your code perform multiple operations concurrently without blocking the main thread, making it faster for I/O-bound tasks like network requests.

#### Do's ✅
- **DO** use async/await for I/O-bound operations (network, file operations)
- **DO** use libraries designed for async (aiohttp, asyncpg, etc.)
- **DO** understand how the event loop works
- **DO** gather multiple coroutines when they can run concurrently

#### Don'ts ❌
- **DON'T** use async for CPU-intensive tasks
- **DON'T** mix sync and async code without proper handling
- **DON'T** block the event loop with sleep() or long calculations
- **DON'T** forget to await coroutines

#### Step-by-Step Example 👣

```python
import asyncio
import aiohttp

# BAD: Synchronous function that blocks
def sync_function():
    """A synchronous function that blocks."""
    print("Starting sync function")
    time.sleep(1)  # This blocks the entire program
    print("Sync function complete")

# GOOD: Asynchronous function
async def async_function():
    """An asynchronous function that doesn't block."""
    print("Starting async function")
    await asyncio.sleep(1)  # This allows other tasks to run
    print("Async function complete")

# Fetching multiple URLs concurrently
async def fetch_url(session, url):
    """Fetch a single URL asynchronously."""
    async with session.get(url) as response:
        return await response.text()

async def fetch_urls_async(urls):
    """Fetch multiple URLs concurrently."""
    async with aiohttp.ClientSession() as session:
        tasks = [fetch_url(session, url) for url in urls]
        results = await asyncio.gather(*tasks)
    return results
```

### Context Managers 🔍

#### What are Context Managers? 🤔
Context managers provide a clean way to allocate and release resources properly, like file handles or database connections.

#### Do's ✅
- **DO** use context managers for resource management
- **DO** implement `__enter__` and `__exit__` methods
- **DO** use `contextlib.contextmanager` for simple cases
- **DO** handle exceptions in the `__exit__` method

#### Don'ts ❌
- **DON'T** manually manage resources when context managers are available
- **DON'T** use context managers for unrelated logic
- **DON'T** swallow exceptions without good reason
- **DON'T** perform long operations within a context manager

#### Step-by-Step Example 👣

```python
# BAD: Manual resource management
def read_file_bad(filename):
    """Read a file without a context manager."""
    file = open(filename, 'r')
    content = file.read()
    file.close()  # Could be forgotten or skipped due to exception
    return content

# GOOD: Using with statement
def read_file_good(filename):
    """Read a file with a context manager."""
    with open(filename, 'r') as file:
        content = file.read()
    return content  # File is automatically closed even if exceptions occur

# Creating your own context manager
class FileHandler:
    """A custom context manager for file operations."""
    
    def __init__(self, filename, mode):
        self.filename = filename
        self.mode = mode
        self.file = None
    
    def __enter__(self):
        """Open the file and return the file object."""
        self.file = open(self.filename, self.mode)
        return self.file
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        """Close the file when exiting the context."""
        if self.file:
            self.file.close()
        return False  # Re-raise any exceptions
```

### Decorators 🎭

#### What are Decorators? 🤔
Decorators are a powerful way to modify or enhance functions and methods without changing their code.

#### Do's ✅
- **DO** use decorators for cross-cutting concerns
- **DO** preserve the original function's metadata
- **DO** use `functools.wraps` in your decorators
- **DO** create parametrized decorators when needed

#### Don'ts ❌
- **DON'T** overuse decorators
- **DON'T** create decorators with complex logic
- **DON'T** modify the original function's behavior unexpectedly
- **DON'T** use decorators when a regular function would be clearer

#### Step-by-Step Example 👣

```python
import time
import functools

# BAD: Decorator without @functools.wraps
def timer_bad(func):
    """Time how long a function takes to execute."""
    def wrapper(*args, **kwargs):
        start_time = time.time()
        result = func(*args, **kwargs)
        end_time = time.time()
        print(f"{func.__name__} took {end_time - start_time:.4f} seconds")
        return result
    return wrapper

# GOOD: Decorator with @functools.wraps
def timer_good(func):
    """Time how long a function takes to execute."""
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        start_time = time.time()
        result = func(*args, **kwargs)
        end_time = time.time()
        print(f"{func.__name__} took {end_time - start_time:.4f} seconds")
        return result
    return wrapper

# Usage
@timer_good
def slow_function(n):
    """A slow function that calculates the sum of numbers."""
    return sum(i for i in range(n))
```

### Error Handling 🚨

#### What is Error Handling? 🤔
Error handling is how you deal with unexpected situations in your code without crashing your program.

#### Do's ✅
- **DO** use specific exception types
- **DO** handle exceptions at the right level
- **DO** include useful error messages
- **DO** clean up resources in finally blocks

#### Don'ts ❌
- **DON'T** catch all exceptions with `except:`
- **DON'T** swallow exceptions without logging
- **DON'T** raise generic exceptions
- **DON'T** use exceptions for flow control

#### Step-by-Step Example 👣

```python
# BAD: Too broad exception handling
def read_config_bad(filename):
    """Read a configuration file with poor error handling."""
    try:
        with open(filename, 'r') as file:
            return file.read()
    except:  # Catches ALL exceptions, including KeyboardInterrupt
        print("Error reading file")
        return None

# GOOD: Specific exception handling
def read_config_good(filename):
    """Read a configuration file with good error handling."""
    try:
        with open(filename, 'r') as file:
            return file.read()
    except FileNotFoundError:
        print(f"Configuration file '{filename}' not found")
        return None
    except PermissionError:
        print(f"No permission to read '{filename}'")
        return None
    except IOError as e:
        print(f"I/O error reading '{filename}': {e}")
        return None
```

### Performance Optimization 🚀

#### What is Performance Optimization? 🤔
Performance optimization means making your code run faster or use less memory.

#### Do's ✅
- **DO** profile your code before optimizing
- **DO** optimize algorithms and data structures first
- **DO** use built-in functions and libraries
- **DO** understand time and space complexity

#### Don'ts ❌
- **DON'T** optimize prematurely
- **DON'T** sacrifice readability for tiny performance gains
- **DON'T** reimplement functionality from standard libraries
- **DON'T** ignore memory usage

#### Step-by-Step Example 👣

```python
from collections import Counter

# BAD: Inefficient counting
def count_word_frequency_bad(text):
    """Count word frequency (inefficient)."""
    words = text.lower().split()
    frequency = {}
    
    for word in words:
        count = 0
        for w in words:
            if w == word:
                count += 1
        frequency[word] = count
    
    return frequency

# GOOD: Efficient counting
def count_word_frequency_good(text):
    """Count word frequency (efficient)."""
    words = text.lower().split()
    frequency = {}
    
    for word in words:
        if word in frequency:
            frequency[word] += 1
        else:
            frequency[word] = 1
    
    return frequency

# BEST: Using collections.Counter
def count_word_frequency_best(text):
    """Count word frequency using Counter."""
    words = text.lower().split()
    return Counter(words)
```

### Design Patterns 🧩

#### What are Design Patterns? 🤔
Design patterns are proven solutions to common programming problems that help you write better, more maintainable code.

#### Do's ✅
- **DO** understand common design patterns
- **DO** apply patterns appropriately for your problem
- **DO** use patterns to improve code structure
- **DO** combine patterns when necessary

#### Don'ts ❌
- **DON'T** use design patterns for simple problems
- **DON'T** overcomplicate code with unnecessary patterns
- **DON'T** force a pattern where it doesn't fit
- **DON'T** ignore Python's built-in features in favor of patterns

#### Step-by-Step Example 👣

**Singleton Pattern**
```python
def singleton(cls):
    """Make a class a Singleton class (only one instance)."""
    instances = {}
    
    @functools.wraps(cls)
    def get_instance(*args, **kwargs):
        if cls not in instances:
            instances[cls] = cls(*args, **kwargs)
        return instances[cls]
    
    return get_instance

@singleton
class Logger:
    """A singleton logger class."""
    
    def __init__(self, level=logging.INFO):
        self.level = level
        self.logger = logging.getLogger("app")
        self.logger.setLevel(level)
```

### Packaging and Distribution 📦

#### What is Packaging? 🤔
Packaging is preparing your code so others can easily install and use it.

#### Do's ✅
- **DO** use a proper project structure
- **DO** include setup files and documentation
- **DO** version your package properly
- **DO** publish to PyPI for public packages

#### Don'ts ❌
- **DON'T** omit important metadata
- **DON'T** include sensitive or unnecessary files
- **DON'T** forget to test your package installation
- **DON'T** hardcode dependencies

#### Step-by-Step Example 👣

**Step 1: Setting up setup.py**
```python
# setup.py
from setuptools import setup, find_packages

# Read README for the long description
with open("README.md", "r", encoding="utf-8") as fh:
    long_description = fh.read()

setup(
    name="my-package",
    version="0.1.0",
    author="Your Name",
    author_email="your.email@example.com",
    description="A short description of your package",
    long_description=long_description,
    long_description_content_type="text/markdown",
    url="https://github.com/yourusername/my-package",
    packages=find_packages(exclude=["tests", "tests.*"]),
    python_requires=">=3.7",
    install_requires=[
        "requests>=2.25.0",
        "pyyaml>=5.1",
    ],
    classifiers=[
        "Programming Language :: Python :: 3",
        "License :: OSI Approved :: MIT License",
        "Operating System :: OS Independent",
    ],
)
```

## Conclusion 🎉

By following these Python best practices, you'll build more robust, maintainable, and efficient applications. Remember that the basics like proper project setup, code organization, security practices, testing, and documentation form the foundation of good Python development. As you advance, you can incorporate more sophisticated techniques like optimized data structures, asynchronous programming, and design patterns.

These practices take time to master, so don't worry if you can't implement everything at once. Start with the basics and gradually incorporate more advanced techniques as your skills grow.

Happy coding! 💻
