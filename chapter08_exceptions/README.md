### **Chapter 8: Exceptions**

In this chapter, we will focus on testing how your code handles **exceptions**. Proper exception handling is crucial in most applications, and it's essential to ensure that your code raises the correct exceptions under the right conditions.

We'll learn how to:
1. Test for exceptions using `pytest.raises()`.
2. Handle and assert specific exception types.
3. Write tests for custom exceptions.

---

### **8.1 Introduction to Exception Handling in Pytest**

In Python, exceptions are typically raised when something goes wrong (like dividing by zero or accessing a nonexistent file). Pytest makes it easy to test that the right exceptions are raised under the right conditions.

#### **Example: Basic Exception Handling**

Here’s a simple example where we raise an exception:

```python
# chapter08_exceptions/test_exceptions.py

def divide(a, b):
    if b == 0:
        raise ValueError("Cannot divide by zero")
    return a / b

def test_divide_by_zero():
    with pytest.raises(ValueError):
        divide(1, 0)
```

In the above example:
- The `divide` function raises a `ValueError` if division by zero is attempted.
- The test `test_divide_by_zero` uses `pytest.raises()` to assert that calling `divide(1, 0)` raises a `ValueError`.

Pytest will pass the test if the exception is raised and fail if it isn't.

---

### **8.2 Asserting the Type of Exception**

You can use `pytest.raises()` not only to assert that an exception is raised but also to check the exact exception type.

#### **Example: Checking for a Specific Exception**

```python
# chapter08_exceptions/test_exceptions.py

def test_invalid_divide():
    with pytest.raises(ValueError) as exc_info:
        divide(1, 0)
    
    # Assert the exception message
    assert str(exc_info.value) == "Cannot divide by zero"
```

In this example:
- `exc_info` captures the exception raised during the test.
- We assert that the exception message matches the expected string: `"Cannot divide by zero"`.

You can also assert the exception type and the message separately for more fine-grained control over your tests.

---

### **8.3 Testing Custom Exceptions**

In real-world applications, you often define your own custom exceptions. Let’s look at how to write tests for custom exceptions.

#### **Example: Custom Exception**

```python
# chapter08_exceptions/test_exceptions.py

class MyCustomError(Exception):
    pass

def raise_custom_error():
    raise MyCustomError("This is a custom exception")

def test_custom_exception():
    with pytest.raises(MyCustomError) as exc_info:
        raise_custom_error()

    assert str(exc_info.value) == "This is a custom exception"
```

In this example:
- We define `MyCustomError`, which is a custom exception.
- The function `raise_custom_error()` raises this custom exception.
- The test `test_custom_exception()` verifies that the exception is correctly raised and that its message matches the expected string.

---

### **8.4 Handling Multiple Exceptions**

Sometimes, you might want to assert that any one of several different exceptions is raised. You can use `pytest.raises()` to handle multiple types of exceptions.

#### **Example: Handling Multiple Exceptions**

```python
# chapter08_exceptions/test_exceptions.py

def test_multiple_exceptions():
    with pytest.raises((ValueError, TypeError)) as exc_info:
        # This will raise a ValueError
        raise ValueError("An error occurred")
    
    assert isinstance(exc_info.value, ValueError)

    with pytest.raises((ValueError, TypeError)) as exc_info:
        # This will raise a TypeError
        raise TypeError("Another error occurred")
    
    assert isinstance(exc_info.value, TypeError)
```

In this example:
- `pytest.raises()` is given a tuple of exception types (`(ValueError, TypeError)`), meaning it will pass the test if either a `ValueError` or a `TypeError` is raised.
- We assert that the exception raised is indeed of the expected type.

---

### **8.5 Using `pytest.mark.xfail` for Expected Failures**

Sometimes, you may expect a test to fail due to known issues or unfinished functionality. In such cases, you can mark the test as an **expected failure** using `pytest.mark.xfail`.

#### **Example: Expected Failure**

```python
# chapter08_exceptions/test_exceptions.py

import pytest

@pytest.mark.xfail
def test_expected_failure():
    # This test is expected to fail
    raise ValueError("This test is expected to fail")
```

In this example:
- The `test_expected_failure` test is marked with `@pytest.mark.xfail`, indicating that we expect the test to fail.
- If the test does fail, it will be reported as an expected failure and won’t be counted as a failure in the overall test results.

If the test passes despite the `xfail` mark, `pytest` will report it as an unexpected pass.

---

### **8.6 Asserting No Exception Was Raised**

Sometimes, you may want to verify that no exception is raised during the execution of a block of code. You can use `pytest.raises()` in combination with `assert` to ensure no exception is raised.

#### **Example: No Exception Raised**

```python
# chapter08_exceptions/test_exceptions.py

def test_no_exception():
    try:
        result = divide(2, 1)
        assert result == 2
    except Exception:
        pytest.fail("No exception should have been raised")
```

In this example:
- We perform a division that should not raise an exception.
- If any exception is raised during the test, `pytest.fail()` is called to explicitly fail the test.

Alternatively, you can use the `assert` statement directly without any exception handling if you’re confident that no exception will occur.

---

### **8.7 Conclusion**

In this chapter, we’ve learned how to:
- Use `pytest.raises()` to assert that exceptions are raised.
- Capture and assert exception messages and types.
- Write tests for custom exceptions.
- Handle multiple types of exceptions in a single test.
- Use `pytest.mark.xfail` to mark tests as expected failures.
- Assert that no exceptions were raised during a test.

Exception testing is a critical part of ensuring that your application handles errors gracefully and raises appropriate exceptions when something goes wrong.