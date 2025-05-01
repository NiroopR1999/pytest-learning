### **Chapter 9: Advanced Fixtures**

In this chapter, we'll dive deeper into **fixtures** in pytest. Fixtures are a powerful feature in pytest that allow you to set up and tear down resources for your tests. They can be used to create complex test environments, manage database connections, mock services, or provide reusable data.

We'll explore:
1. Advanced fixture usage.
2. Fixture scopes.
3. Autouse fixtures.
4. Using fixtures in tests and other fixtures.
5. Combining fixtures with parametrization.

---

### **9.1 Recap of Fixtures**

A **fixture** in pytest is a function that runs before a test is executed. It is used to set up any state or resources needed by the test. Fixtures are marked with the `@pytest.fixture` decorator, and they can be used as function arguments in your test functions.

#### **Simple Fixture Example**

```python
# chapter09_advanced_fixtures/test_advanced_fixtures.py

import pytest

@pytest.fixture
def sample_data():
    return {"name": "John", "age": 30}

def test_sample_data(sample_data):
    assert sample_data["name"] == "John"
    assert sample_data["age"] == 30
```

In this example:
- We define a simple fixture `sample_data`, which returns a dictionary.
- The fixture is automatically passed to the test function `test_sample_data`, where we use the data to assert values.

---

### **9.2 Fixture Scopes**

The **scope** of a fixture determines how long the fixture will live. You can specify the scope of a fixture to control how often it is set up and torn down. The possible scopes are:
- `function` (default): The fixture is set up and torn down for each test function.
- `class`: The fixture is set up and torn down once per test class.
- `module`: The fixture is set up and torn down once per module.
- `session`: The fixture is set up and torn down once per session (all tests in the entire run).

#### **Example: Function and Class Scopes**

```python
# chapter09_advanced_fixtures/test_advanced_fixtures.py

import pytest

@pytest.fixture(scope="function")
def function_scope_fixture():
    print("Setting up function scope fixture")
    return "function scope"

@pytest.fixture(scope="class")
def class_scope_fixture():
    print("Setting up class scope fixture")
    return "class scope"

class TestClass:
    def test_function_scope(self, function_scope_fixture):
        assert function_scope_fixture == "function scope"
    
    def test_class_scope(self, class_scope_fixture):
        assert class_scope_fixture == "class scope"
```

In this example:
- The fixture `function_scope_fixture` is created and destroyed for each test function.
- The fixture `class_scope_fixture` is created and destroyed once for all the tests in the `TestClass` class.
- You will see the print statements indicating when the fixtures are set up.

#### **Scope Values**:
- `function`: The default scope, used for test functions.
- `class`: Useful for grouping tests into classes that share a common setup.
- `module`: Useful for grouping tests in a module that share a common setup.
- `session`: Useful for setting up expensive or shared resources that should last for the entire test session.

---

### **9.3 Autouse Fixtures**

By default, you have to explicitly request a fixture in the test function arguments. However, you can use the `autouse=True` argument to automatically use a fixture without having to specify it as a function argument.

#### **Example: Using Autouse Fixtures**

```python
# chapter09_advanced_fixtures/test_advanced_fixtures.py

import pytest

@pytest.fixture(autouse=True)
def setup_and_teardown():
    print("Setting up the test environment")
    yield
    print("Tearing down the test environment")

def test_example_1():
    print("Running test 1")

def test_example_2():
    print("Running test 2")
```

In this example:
- The fixture `setup_and_teardown` is automatically invoked for every test in the module due to `autouse=True`.
- The fixture runs before each test (setup) and after each test (teardown), without needing to be explicitly included as a parameter in the test functions.

The `yield` statement is used to separate the setup and teardown actions:
- Everything before `yield` runs before the test.
- Everything after `yield` runs after the test.

---

### **9.4 Using Fixtures in Other Fixtures**

Fixtures can depend on other fixtures. This allows you to break down complex setups into smaller, reusable parts. Pytest will automatically resolve and inject the necessary fixtures.

#### **Example: Fixtures Using Other Fixtures**

```python
# chapter09_advanced_fixtures/test_advanced_fixtures.py

import pytest

@pytest.fixture
def database_connection():
    return {"connection": "open"}

@pytest.fixture
def user_data(database_connection):
    database_connection["user"] = "John"
    return database_connection

def test_user_data(user_data):
    assert user_data["user"] == "John"
    assert user_data["connection"] == "open"
```

In this example:
- The `user_data` fixture depends on the `database_connection` fixture.
- The `user_data` fixture injects the `database_connection` fixture and modifies it before returning.
- The test `test_user_data` uses the `user_data` fixture, which automatically includes the `database_connection`.

---

### **9.5 Parametrizing Fixtures**

You can parametrize fixtures to run the same test with different input values. This allows you to avoid writing multiple test functions for similar scenarios, making your tests more compact and expressive.

#### **Example: Parametrizing Fixtures**

```python
# chapter09_advanced_fixtures/test_advanced_fixtures.py

import pytest

@pytest.fixture(params=[1, 2, 3])
def number(request):
    return request.param

def test_multiplication(number):
    result = number * 2
    assert result == number * 2
```

In this example:
- The `number` fixture is parametrized with three values: `1`, `2`, and `3`.
- The `test_multiplication` test runs three times, once for each parameter value.
- The `request.param` allows access to the parameter value for the current test.

---

### **9.6 Combining Fixtures and Parametrization**

You can combine both fixture parametrization and the `pytest.mark.parametrize` decorator for even more powerful tests.

#### **Example: Combining Fixtures and Parametrization**

```python
# chapter09_advanced_fixtures/test_advanced_fixtures.py

import pytest

@pytest.fixture
def setup_database():
    return {"user": "John", "db": "test_db"}

@pytest.mark.parametrize("new_user", ["Alice", "Bob", "Charlie"])
def test_add_user(setup_database, new_user):
    setup_database["user"] = new_user
    assert setup_database["user"] == new_user
```

In this example:
- We combine a fixture `setup_database` with the `pytest.mark.parametrize` decorator.
- The `test_add_user` test runs for each of the values provided by the parameterized `new_user` list, testing adding different users to the database.

---

### **9.7 Conclusion**

In this chapter, we covered:
- **Advanced fixture usage**, including scopes, dependencies, and parametrization.
- **Autouse fixtures** that are automatically applied to tests.
- How to **combine fixtures** and **parametrization** to make tests more efficient and expressive.

Advanced fixtures provide a lot of flexibility and power in setting up your test environment. They allow you to manage resources more effectively and ensure that your tests remain clean and maintainable.