### **Chapter 6: Coverage Reporting**

In this chapter, we will focus on generating **coverage reports** for your test suite. Test coverage helps you measure how much of your code is covered by tests. A higher test coverage percentage means that your tests are covering more of your application logic.

We will explore how to integrate **coverage reporting** with `pytest`, generate reports, and understand how to interpret them.

---

### **6.1 Introduction to Code Coverage**

**Code coverage** is a metric that tells you which parts of your codebase have been executed during your tests. The more code that is covered by tests, the better.

- **Statement Coverage**: Tracks which lines of code have been executed.
- **Branch Coverage**: Tracks which branches (e.g., `if`/`else` statements) have been executed.

In this chapter, we'll use `pytest-cov`, a plugin for `pytest` that integrates with the **coverage.py** library to generate coverage reports.

---

### **6.2 Installing `pytest-cov`**

First, you need to install the `pytest-cov` plugin. This allows you to generate coverage reports during your test runs.

You can install it using `pip`:

```bash
pip install pytest-cov
```

---

### **6.3 Generating a Coverage Report**

After installing `pytest-cov`, you can generate coverage reports by simply passing the `--cov` option when running `pytest`.

#### **Basic Example: Running Tests with Coverage Reporting**

Here’s how to generate a basic coverage report:

```bash
pytest --cov=src tests/
```

In this example:
- `--cov=src`: This tells `pytest` to track coverage for the `src/` directory, where your application code resides.
- `tests/`: This tells `pytest` to run the tests in the `tests/` directory.

After running the command, `pytest-cov` will generate a report in your terminal, showing the percentage of lines covered by your tests.

#### **Sample Coverage Report:**

```bash
============================= test session starts ==============================
collected 3 items

tests/test_calculator.py ...                                               [100%]

----------- coverage: platform linux, python 3.8.5-final-0 -----------
Name                           Stmts   Miss  Cover   Missing
------------------------------------------------------------
src/calculator.py                12      2    83%    4-5
------------------------------------------------------------
TOTAL                           12      2    83%
```

The output shows:
- **Stmts**: The number of statements (lines of code) in your source file.
- **Miss**: The number of statements not covered by tests.
- **Cover**: The percentage of statements covered by tests.

In this case, the `calculator.py` file has 12 statements, 2 of which were not covered, giving us a coverage of 83%.

---

### **6.4 Generating a Detailed Coverage Report**

You can generate a more detailed HTML report to visually inspect the coverage.

To generate an HTML report:

```bash
pytest --cov=src --cov-report=html tests/
```

This command generates a folder called `htmlcov/`, which contains an `index.html` file. You can open this file in your browser to see a detailed, interactive report that highlights the covered and uncovered parts of your code.

---

### **6.5 Coverage Reporting Formats**

`pytest-cov` allows you to generate coverage reports in several formats:

1. **Terminal (Text)**: The default report format.
   
   ```bash
   pytest --cov=src tests/
   ```

2. **HTML Report**: As we already saw, use `--cov-report=html` for a visual report.

3. **XML Report**: Useful for integrating with CI tools.

   ```bash
   pytest --cov=src --cov-report=xml tests/
   ```

   This will generate a `coverage.xml` file.

4. **JSON Report**: To integrate with other tools or for further processing.

   ```bash
   pytest --cov=src --cov-report=json tests/
   ```

   This generates a `coverage.json` file.

---

### **6.6 Excluding Files or Lines from Coverage**

Sometimes, there are certain files or lines of code that you don’t want to include in the coverage report. You can exclude them using the `--cov-config` option and by configuring a `.coveragerc` file.

#### **Example: `.coveragerc` Configuration File**

You can create a `.coveragerc` file in the root of your project to exclude specific files or lines from the coverage report.

```ini
[coverage:run]
branch = True

[coverage:report]
exclude_lines =
    # Exclude lines for testing purposes or commented-out code
    def __repr__
    raise NotImplementedError
    if __name__ == '__main__':
```

#### **Excluding Files**

You can also exclude files or directories from coverage by adding this to the `.coveragerc` file:

```ini
[coverage:run]
omit =
    src/tests/*
```

This would exclude all files inside the `tests/` directory from being included in the coverage report.

---

### **6.7 Combining Coverage from Multiple Runs**

If you are running tests in multiple environments (e.g., unit tests, integration tests), you can combine coverage results from different runs using the `--cov-append` option:

```bash
pytest --cov=src --cov-append tests/unit/
pytest --cov=src --cov-append tests/integration/
```

This will append the coverage data from both test runs into one combined report.

---

### **6.8 Conclusion**

In this chapter, we learned:
- How to install and use `pytest-cov` for coverage reporting.
- How to generate basic, detailed, and different types of coverage reports.
- How to exclude files or lines from the coverage report.
- How to combine coverage from multiple test runs.

Coverage reporting is a powerful way to ensure your tests are covering as much of your application as possible, helping you identify untested parts of your codebase.
