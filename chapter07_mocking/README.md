### **Chapter 7: Mocking**

In this chapter, we’ll cover **mocking** in pytest, a powerful technique for isolating parts of your code during testing. Mocking allows you to simulate objects and behaviors that your code depends on, such as database connections, APIs, or external services, without actually invoking those dependencies.

We'll learn:
1. What mocking is and when to use it.
2. How to mock objects using `unittest.mock`.
3. How to patch functions or methods.
4. Writing tests with `pytest-mock`.

---

### **7.1 Introduction to Mocking**

Mocking is a technique used in testing to simulate external dependencies and control their behavior. For example, instead of making an actual HTTP request to a remote server in a test, you can mock the HTTP request and return a pre-defined response. This speeds up tests and ensures they’re not dependent on external systems.

`pytest` does not provide mocking directly, but it integrates well with Python’s built-in `unittest.mock` library or the third-party `pytest-mock` plugin, which simplifies the process.

#### **When to Use Mocking**

You should use mocking when:
- You need to isolate the unit under test and avoid dependencies on external systems.
- You want to simulate scenarios (e.g., API errors, timeouts, database failures) that are hard to reproduce in a real environment.
- You need to control the behavior of dependencies to test specific code paths.

---

### **7.2 Mocking with `unittest.mock`**

Python’s standard library includes `unittest.mock`, which provides a `Mock` class and `patch` function for mocking.

#### **Mocking Objects**

The `Mock` class can be used to create mock objects, which you can then configure to simulate specific behaviors.

```python
# chapter07_mocking/test_mocking.py

from unittest.mock import Mock

# Mocking a simple object
def test_mock_object():
    mock_obj = Mock()
    mock_obj.some_method.return_value = "mocked value"

    # Call the mocked method
    result = mock_obj.some_method()

    assert result == "mocked value"
    mock_obj.some_method.assert_called_once()  # Verify method was called once
```

In this example:
- We create a mock object `mock_obj` and specify that calling `some_method()` will return `"mocked value"`.
- We call the method, assert that the returned value is `"mocked value"`, and verify that the method was called exactly once using `assert_called_once()`.

#### **Mocking Functions or Methods**

You can also use `patch` to mock functions or methods during the test. `patch` temporarily replaces the target function or method with a mock during the test.

```python
# chapter07_mocking/test_mocking.py

from unittest.mock import patch

def get_data_from_api():
    # Simulate an external API call
    return {"data": "real data"}

def test_mock_api_call():
    with patch('__main__.get_data_from_api', return_value={"data": "mocked data"}):
        result = get_data_from_api()
        assert result == {"data": "mocked data"}
```

In this example:
- We use `patch` to replace `get_data_from_api` with a mock function that returns `{"data": "mocked data"}`.
- Inside the `with` block, any call to `get_data_from_api` will return the mocked value.

---

### **7.3 Using `pytest-mock` Plugin**

While `unittest.mock` is powerful, the `pytest-mock` plugin simplifies the syntax when using mocks in pytest. It integrates seamlessly with pytest and provides a `mocker` fixture for mocking.

#### **Installing `pytest-mock`**

To use the `pytest-mock` plugin, you’ll need to install it first:

```bash
pip install pytest-mock
```

#### **Example: Mocking with `pytest-mock`**

```python
# chapter07_mocking/test_mocking.py

def test_mock_api_call(mocker):
    # Mocking the function with pytest-mock
    mocker.patch('__main__.get_data_from_api', return_value={"data": "mocked data"})

    result = get_data_from_api()

    assert result == {"data": "mocked data"}
```

In this example:
- We use the `mocker` fixture to patch the `get_data_from_api` function.
- The function now returns the mocked data when called during the test.

#### **Mocking a Method of an Object**

You can also mock methods of objects using `mocker.patch.object()`:

```python
# chapter07_mocking/test_mocking.py

class MyClass:
    def my_method(self):
        return "real value"

def test_mock_method(mocker):
    my_obj = MyClass()

    # Mocking the method of the object
    mocker.patch.object(my_obj, 'my_method', return_value="mocked value")

    result = my_obj.my_method()
    assert result == "mocked value"
```

In this example:
- We create a `MyClass` object and mock its `my_method` method to return `"mocked value"`.
- The test verifies that the mocked value is returned instead of the real value.

---

### **7.4 Asserting Mock Calls**

In addition to controlling the behavior of mocks, you can also verify that the mock objects were called in the expected way.

#### **Example: Verifying Mock Calls**

```python
# chapter07_mocking/test_mocking.py

def some_function():
    return "real data"

def test_mocking_calls(mocker):
    mock_func = mocker.patch('__main__.some_function', return_value="mocked data")

    # Call the mocked function
    result = some_function()

    assert result == "mocked data"
    mock_func.assert_called_once()  # Ensure the mock was called exactly once
    mock_func.assert_called_with()  # Ensure the mock was called with specific arguments
```

In this example:
- We use `assert_called_once()` to verify that the mocked function was called exactly once.
- We use `assert_called_with()` to verify that the mock was called with specific arguments.

---

### **7.5 Mocking Side Effects**

You can also use `side_effect` to make a mock return different values on successive calls or simulate an exception.

#### **Example: Using `side_effect`**

```python
# chapter07_mocking/test_mocking.py

def test_side_effect(mocker):
    mock_func = mocker.patch('__main__.some_function', side_effect=[1, 2, 3])

    # Call the function multiple times
    assert some_function() == 1
    assert some_function() == 2
    assert some_function() == 3
```

In this example:
- We use `side_effect` to make `some_function` return `1`, `2`, and `3` on subsequent calls.

#### **Example: Simulating an Exception**

```python
# chapter07_mocking/test_mocking.py

def test_side_effect_exception(mocker):
    mock_func = mocker.patch('__main__.some_function', side_effect=ValueError("An error occurred"))

    with pytest.raises(ValueError):
        some_function()
```

In this example:
- We simulate a `ValueError` being raised whenever `some_function` is called.

---

### **7.6 Conclusion**

In this chapter, we learned how to:
- Use `unittest.mock` for basic mocking of objects and functions.
- Patch functions or methods using `patch`.
- Simplify mocking with the `pytest-mock` plugin.
- Verify that mocked objects were called as expected.
- Simulate side effects, including returning different values and raising exceptions.

Mocking is a powerful technique that helps you isolate units of code for testing, allowing you to focus on testing the logic in isolation while controlling external dependencies.

Next, in **Chapter 8**, we’ll explore **exception handling** and how to test your code’s behavior when exceptions are raised.

Let me know if you're ready for Chapter 8 or if you have any questions!