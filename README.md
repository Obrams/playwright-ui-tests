# UI Autotests for SauceDemo

A full-featured UI test automation framework in Python using Playwright and the Page Object pattern for testing [saucedemo.com](https://www.saucedemo.com/).

## Contents

- [Description](#description)
- [Project structure](#project-structure)
- [Requirements](#requirements)
- [Installation](#installation)
- [Running tests](#running-tests)
- [Reports](#reports)
- [Test markers](#test-markers)
- [Test scenarios](#test-scenarios)

## Description

The project is a set of automated UI tests for the SauceDemo online store. The tests use:

- **Playwright** - a modern browser automation framework
- **Pytest** - a powerful Python testing framework
- **pytest-xdist** - parallel test execution for maximum speed
- **Page Object Pattern** - a design pattern for structuring test code
- **Allure/HTML Reports** - for generating rich reports

## Project structure

```
playwrite_ui_test/
├── pages/                    # Page Object classes
│   ├── __init__.py
│   ├── base_page.py         # Base class for all pages
│   ├── login_page.py        # Login page
│   ├── inventory_page.py    # Product catalog page
│   ├── cart_page.py         # Cart page
│   ├── checkout_page.py     # Checkout page
│   └── checkout_complete_page.py  # Order complete page
├── tests/                    # Test scenarios
│   ├── __init__.py
│   ├── conftest.py          # Pytest fixtures and configuration
│   ├── test_login.py        # Login tests (6 tests)
│   ├── test_inventory.py    # Product catalog tests (10 tests)
│   ├── test_cart.py         # Cart tests (8 tests)
│   └── test_checkout.py     # Checkout tests (9 tests)
├── utils/                    # Helper utilities
│   ├── __init__.py
│   └── test_data.py         # Test data
├── screenshots/              # Screenshots on test failure (created automatically)
├── allure-results/           # Allure results (created automatically)
├── allure-report/            # Generated Allure report
├── pytest.ini               # pytest configuration
├── requirements.txt         # Project dependencies
└── README.md                # Documentation

```

## Requirements

- Python 3.8 or higher
- pip (Python package manager)
- Git (optional)

## Installation

### 1. Clone the repository (if using Git)

```bash
git clone <repository-url>
cd playwrite_ui_test
```

### 2. Create a virtual environment (recommended)

**Windows:**
```bash
python -m venv venv
venv\Scripts\activate
```

**macOS/Linux:**
```bash
python3 -m venv venv
source venv/bin/activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Install Playwright browsers

```bash
playwright install
```

Or install Chromium only:
```bash
playwright install chromium
```

## Running tests

### Basic commands

**Run all tests:**
```bash
pytest
```

**Run with print output:**
```bash
pytest -s
```

**Run in headed mode (with a visible browser):**
```bash
pytest --headed
```

**Run with slow-mo (to watch the process):**
```bash
pytest --headed --slowmo=1000
```

### Run by markers

**Run smoke tests only:**
```bash
pytest -m smoke
```

**Run login tests only:**
```bash
pytest -m login
```

**Run regression tests:**
```bash
pytest -m regression
```

**Run cart and checkout tests:**
```bash
pytest -m "cart or checkout"
```

### Run specific files or tests

**Run a specific file:**
```bash
pytest tests/test_login.py
```

**Run a specific test:**
```bash
pytest tests/test_login.py::TestLogin::test_successful_login
```

**Run a specific test class:**
```bash
pytest tests/test_login.py::TestLogin
```

### 🚀 Parallel run (RECOMMENDED for 35 tests)

> **Important:** for 35 tests, parallel execution **significantly speeds up** the run (4–5x faster)!

**Run all tests in parallel (auto-detect workers):**
```bash
pytest -n auto
```

**Run with a given number of workers:**
```bash
pytest -n 8                      # Use 8 workers
pytest -n 4 -m smoke            # 4 workers, smoke tests only
```

**Fast headless run (no GUI, ideal for CI/CD):**
```bash
pytest -n auto --headed=false                    # Maximum speed
pytest -n 6 --headed=false                       # 6 workers, headless
pytest -n auto --headed=false -m regression      # Headless regression tests
```

**Stop on first failure:**
```bash
pytest -x                        # Stop on first failure
pytest -n 4 -x                   # Parallel, stop on failure
```

### Useful pytest options

**Debugging and output:**
```bash
pytest -v                        # Verbose output
pytest -s                        # Show print() in console
pytest -l                        # Show local variables on failure
pytest -vv                       # Very verbose output
```

**Execution control:**
```bash
pytest -x                        # Stop on first failure
pytest --maxfail=3               # Stop after 3 failures
pytest --lf                      # Run only failed tests
pytest --ff                      # Failed first, then the rest
```

**Combined commands:**
```bash
# Smoke tests in headless mode, multi-threaded, with Allure
pytest -n auto --headed=false -m smoke --alluredir=allure-results

# Regression tests with 6 workers and Allure
pytest -n 6 -m regression --alluredir=allure-results

# Quick check, stop on first failure
pytest -n auto --headed=false -x -m smoke

# Verbose output with local variables
pytest -vv -l tests/test_login.py
```

### Browser selection

**Run in Firefox:**
```bash
pytest --browser firefox
```

**Run in Safari:**
```bash
pytest --browser webkit
```

**Run in all browsers:**
```bash
pytest --browser chromium --browser firefox --browser webkit
```

## Reports

### Allure report (recommended) 🎯

The project is fully integrated with the Allure Framework for rich, informative reports.

#### Install the Allure CLI

**macOS:**
```bash
brew install allure
```

**Linux:**
```bash
sudo apt-add-repository ppa:qameta/allure
sudo apt-get update
sudo apt-get install allure
```

**Windows:**
```bash
scoop install allure
```

Or download it from the [official site](https://github.com/allure-framework/allure2/releases).

#### Running with Allure

**1. 🚀 Run all tests in parallel with Allure (RECOMMENDED):**
```bash
pytest -n auto --alluredir=allure-results
pytest -n 8 --alluredir=allure-results
```

**2. 🚀 Fast headless run (no GUI, maximum speed):**
```bash
pytest -n auto --headed=false --alluredir=allure-results
pytest -n 8 --headed=false --alluredir=allure-results
```

**3. Run smoke tests with Allure (multi-threaded):**
```bash
pytest -n auto -m smoke --alluredir=allure-results
pytest -n 4 -m smoke --alluredir=allure-results
```

**4. Run regression tests with Allure (multi-threaded):**
```bash
pytest -n auto -m regression --alluredir=allure-results
pytest -n 6 -m regression --alluredir=allure-results
```

**5. Sequential run with Allure (for debugging):**
```bash
pytest --alluredir=allure-results
```

**6. View the report:**
```bash
allure serve allure-results
```

#### Manual run with Allure

**Run tests and generate results:**
```bash
pytest --alluredir=allure-results
```

**View the report (starts a local server):**
```bash
allure serve allure-results
```

**Generate a static HTML report:**
```bash
allure generate allure-results -o allure-report --clean
```

**Open the generated report:**
```bash
allure open allure-report
```

#### Allure report features in this project

✅ **Automatic screenshots** - on test failure, a screenshot is automatically attached to the report
✅ **Structured steps** - every action in a test is a separate step
✅ **Categories and labels** - tests are organized by Epic, Feature and Story
✅ **Severity levels** - every test has a severity level (blocker, critical, normal, minor)
✅ **Run history** - tracking test stability across runs

#### Test organization in the report

- **Epic**: "E-commerce"
- **Features**: "Login", "Cart", "Product catalog", "Checkout"
- **Stories**: specific user scenarios
- **Severities**: Blocker, Critical, Normal, Minor

### HTML report (pytest-html)

**Generate an HTML report:**
```bash
pytest --html=report.html --self-contained-html
```

The report is saved to `report.html` in the project root.

### Screenshots

On any test failure, a screenshot is automatically saved in two places:

1. **Locally** in the `screenshots/` directory, named `{test_name}_{timestamp}.png`
2. **In the Allure report** - automatically attached to the failed test

## Test markers

| Marker | Description | Number of tests |
|--------|----------|-------------------|
| `smoke` | Fast smoke tests for core functionality | ~10 tests |
| `regression` | Full regression suite | ~20 tests |
| `login` | Login tests | 6 tests |
| `inventory` | Product catalog tests | 10 tests |
| `cart` | Cart tests | 8 tests |
| `checkout` | Checkout tests | 9 tests |

## Test scenarios

### Login tests (test_login.py)

1. Successful login with valid credentials
2. Login attempt with a locked-out user
3. Login with invalid data (parameterized test)
4. Login attempt with empty username
5. Login attempt with empty password
6. All elements are present on the login page

### Product catalog tests (test_inventory.py)

1. All products are displayed (6 products)
2. Add one product to the cart
3. Add several products to the cart
4. Remove a product from the cart on the catalog page
5. Sort products by name (A to Z)
6. Sort products by name (Z to A)
7. Sort products by price (low to high)
8. Sort products by price (high to low)
9. Cart counter updates when adding/removing products
10. Navigate to the cart

### Cart tests (test_cart.py)

1. View products in the cart
2. View several products in the cart
3. Remove a product from the cart
4. Remove one of several products
5. Continue shopping from the cart
6. Empty cart check
7. Product prices in the cart
8. Proceed to checkout

### Checkout tests (test_checkout.py)

1. Full purchase flow (E2E test)
2. Form validation - empty first name
3. Form validation - empty last name
4. Form validation - empty postal code
5. Cancel checkout at the first step
6. Order total is calculated correctly
7. Number of items in the order overview
8. Return to the catalog after completing an order

## Test users

```python
standard_user / secret_sauce       # Standard user
locked_out_user / secret_sauce     # Locked-out user
problem_user / secret_sauce        # User with UI bugs
performance_glitch_user / secret_sauce  # User with performance delays
```

## Additional commands

**Check the Playwright version:**
```bash
playwright --version
```

**List installed browsers:**
```bash
playwright show-browsers
```

**Update dependencies:**
```bash
pip install --upgrade -r requirements.txt
```

## Useful links

- [Playwright Documentation](https://playwright.dev/python/)
- [Pytest Documentation](https://docs.pytest.org/)
- [Allure Documentation](https://docs.qameta.io/allure/)
- [SauceDemo Test Site](https://www.saucedemo.com/)

## Author

The project was created as an example of a full-featured UI test framework using modern tools and best practices.

## License

This project was created for educational purposes.
