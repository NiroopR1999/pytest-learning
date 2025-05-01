### **Chapter 3: Writing Basic Tests**

In this chapter, we’ll dive deeper into writing tests with `pytest`. You'll learn how to write **assertions**, organize test cases, and use the features that make `pytest` more powerful.

---

### **3.1 Assertions in `pytest`**

In `pytest`, assertions are the cornerstone of test cases. Assertions compare values and check whether the test behaves as expected.

#### **Basic Assertion Example**

Let’s start with a basic assertion. The `assert` statement compares the left-hand side and right-hand side values. If they are equal, the test passes. Otherwise, it fails.

Here’s an example:

```python
# test_example.py

def test_addition():
    assert 1 + 2 == 3
```

In this test case, `1 + 2 == 3` is the assertion. If this evaluates to `True`, the test passes.

#### **More Complex Assertions**

You can also make assertions about more complex conditions. Here are a few examples:

1. **Comparing strings:**

   ```python
   def test_string_comparison():
       assert "hello" == "hello"
   ```

2. **Testing inequalities:**

   ```python
   def test_inequality():
       assert 10 > 5
   ```

3. **Testing list contents:**

   ```python
   def test_list():
       my_list = [1, 2, 3]
       assert 2 in my_list
   ```

4. **Testing for exceptions:**

   ```python
   def test_divide_by_zero():
       with pytest.raises(ZeroDivisionError):
           1 / 0
   ```

The `pytest.raises` function allows you to check that a specific exception is raised during the test.

---

### **3.2 Organizing Tests**

When writing multiple tests, it’s important to organize them to keep your code clean and maintainable. `pytest` supports test organization through:

- **Test Files**: Files should start with `test_` and contain functions starting with `test_`.
- **Test Functions**: Functions should start with `test_` and should represent a single unit of testing.

You can also group related tests using classes or separate them into different files and directories.

#### **Example: Grouping Related Tests in Classes**

```python
# test_operations.py

class TestMathOperations:
    
    def test_addition(self):
        assert 1 + 1 == 2

    def test_subtraction(self):
        assert 5 - 3 == 2
```

In this example, we have a class `TestMathOperations` with multiple test functions. This helps group related tests together, but note that classes are not strictly required.

---

### **3.3 Running Specific Tests**

You don’t always need to run all tests. Sometimes you want to run specific test functions. You can do this by specifying the test name when running `pytest`.

1. **Running a specific test function:**

   ```bash
   pytest test_operations.py::test_addition
   ```

   This will only run the `test_addition` function inside the `test_operations.py` file.

2. **Running multiple tests:**

   You can run multiple specific test functions by specifying each one:

   ```bash
   pytest test_operations.py::test_addition test_operations.py::test_subtraction
   ```

---

### **3.4 Marking Tests**

You can mark tests to categorize them for different purposes. For example, you might mark some tests as **slow**, **integration**, or **smoke** tests.

#### **Marking Tests Example**

```python
import pytest

@pytest.mark.slow
def test_large_data_processing():
    # Simulate a slow test
    assert 2 + 2 == 4
```

Then, to run tests with a specific mark, use:

```bash
pytest -m slow
```

This will only run tests marked with `@pytest.mark.slow`.

#### **Skipping Tests**

If you want to skip certain tests, you can use the `skip` decorator:

```python
import pytest

@pytest.mark.skip(reason="Skipping this test for now")
def test_skipped():
    assert 1 + 1 == 3
```

---

### **3.5 Assertions and Output**

When a test passes, `pytest` will show a dot (`.`) in the terminal. If a test fails, `pytest` will provide useful output, showing which assertion failed and why.

#### **Example of Test Failure Output:**

```bash
============================= test session starts ==============================
collected 1 item

test_example.py F                                                           [100%]

==================================== FAILURES =====================================
_________________________________ test_addition __________________________________

test_example.py:3 (test_addition)
        assert 1 + 2 == 3
E       assert 3 == 3
=========================== short test summary info ============================
FAILED test_example.py::test_addition  [100%]
============================== 1 failed in 0.03 seconds ============================
```

This output clearly shows which assertion failed and why.

---

### **3.6 Conclusion**

In this chapter, we’ve learned how to:
- Use assertions to check values and behaviors
- Organize tests into files and classes
- Run specific tests
- Mark and skip tests
