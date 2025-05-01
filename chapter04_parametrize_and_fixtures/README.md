Great! Let's dive into **Chapter 4: Parametrize and Fixtures**. This chapter will help you understand how to write **parameterized tests** and how to use **fixtures** to set up and tear down test data.

---

### **Chapter 4: Parametrize and Fixtures**

#### **4.1 Parametrization in Pytest**

One of the key features of `pytest` is **parametrization**, which allows you to run the same test function with different sets of input data. This is particularly useful when you need to test a function with multiple inputs and expected outputs.

##### **4.1.1 Using `@pytest.mark.parametrize` Decorator**

`pytest.mark.parametrize` allows you to define multiple sets of input values for a test function. Here's how you can use it.

**Example: Parametrizing a Simple Test**

```python
# chapter04_parametrize_and_fixtures/test_parametrize.py

import pytest

# Parametrizing the test to run it with multiple sets of input data
@pytest.mark.parametrize("a, b, expected", [
    (1, 2, 3),
    (2, 3, 5),
    (3, 4, 7),
])
def test_addition(a, b, expected):
    assert a + b == expected
```

Here, the test `test_addition` will run three times with the following sets of input values:
- `a=1, b=2, expected=3`
- `a=2, b=3, expected=5`
- `a=3, b=4, expected=7`

**Run the test**:

```bash
pytest chapter04_parametrize_and_fixtures/test_parametrize.py
```

You will see the output like this:

```
============================= test session starts ==============================
collected 3 items

chapter04_parametrize_and_fixtures/test_parametrize.py ..F                 [100%]

=================================== FAILURES ====================================
_________________________________ test_addition _________________________________

a = 3, b = 4, expected = 7

    def test_addition(a, b, expected):
>       assert a + b == expected
E       assert 7 == 7

=========================== short test summary info ============================
FAILED test_parametrize.py::test_addition  [100%]
============================== 1 failed in 0.02 seconds ============================
```

As you can see, this allows you to run the same test multiple times with different values.

---

#### **4.2 Fixtures in Pytest**

A **fixture** is a piece of code that sets up some state or context needed for your tests. Fixtures can be used to provide:
- Database connections
- Mock data
- Web server setup

Fixtures are automatically executed before the test runs and can be shared across tests.

##### **4.2.1 Creating a Fixture**

Here's how you can create a simple fixture that returns test data.

```python
# chapter04_parametrize_and_fixtures/test_fixtures.py

import pytest

# Define a fixture using the @pytest.fixture decorator
@pytest.fixture
def sample_data():
    return {"username": "testuser", "password": "testpass"}

def test_login_with_valid_credentials(sample_data):
    assert sample_data["username"] == "testuser"
    assert sample_data["password"] == "testpass"
```

In the above example:
- The `sample_data` fixture returns a dictionary containing `username` and `password`.
- The test `test_login_with_valid_credentials` uses the `sample_data` fixture by adding it as a function argument.

Pytest will automatically detect that `sample_data` is a fixture and call it before running the test.

##### **4.2.2 Using Fixtures for Setup and Teardown**

Fixtures can also be used for cleanup after tests. To do this, you can use the `yield` statement in the fixture. Code before the `yield` runs as setup, and code after the `yield` runs as teardown.

**Example of a Fixture with Setup and Teardown**

```python
# chapter04_parametrize_and_fixtures/test_fixtures.py

import pytest

@pytest.fixture
def setup_teardown():
    # Setup code
    print("Setting up the test environment.")
    
    # Yield control to the test
    yield
    
    # Teardown code
    print("Tearing down the test environment.")

def test_with_setup_teardown(setup_teardown):
    print("Running the test with setup and teardown.")
    assert True
```

In this example:
- The `setup_teardown` fixture has setup code before the `yield` and teardown code after.
- The test `test_with_setup_teardown` uses the fixture, and pytest will handle calling the fixture, performing setup before the test, and teardown afterward.

---

#### **4.3 Parametrizing Fixtures**

You can also parametrize fixtures, meaning you can run the same fixture with multiple sets of values.

**Example of Parametrized Fixture**

```python
# chapter04_parametrize_and_fixtures/test_fixtures.py

import pytest

# Parametrizing the fixture
@pytest.fixture(params=[(1, 2), (3, 4), (5, 6)])
def data(request):
    return request.param

def test_addition_with_fixture(data):
    a, b = data
    assert a + b == a + b
```

Here, the `data` fixture is parametrized with three sets of input values. The test `test_addition_with_fixture` will run three times with different sets of values.

---

### **4.4 Conclusion**

In this chapter, we’ve learned how to:
- Use `pytest.mark.parametrize` to run tests with multiple sets of input values
- Create and use fixtures to set up test environments and provide data to tests
- Use fixtures for setup and teardown operations
- Parametrize fixtures to run the same fixture with different values