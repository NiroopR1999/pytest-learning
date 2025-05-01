### **Chapter 2: Installing and Setting Up Pytest**

Now that you understand the importance of testing, let's get **pytest** up and running so you can start writing tests for your Python projects.

---

### **2.1 Installing Pytest**

To start using `pytest`, you first need to install it. Here's how you can do that:

#### **Step 1: Create a Virtual Environment (Optional but Recommended)**

It’s a good practice to use a virtual environment for your projects. This way, dependencies are kept separate from the system-wide Python installation, making it easier to manage your project’s dependencies.

1. **Create a virtual environment**:
   
   ```bash
   python -m venv venv
   ```

2. **Activate the virtual environment**:

   - On **Windows**:
     ```bash
     venv\Scripts\activate
     ```

   - On **macOS/Linux**:
     ```bash
     source venv/bin/activate
     ```

   You should now see `(venv)` in your terminal, indicating that the virtual environment is active.

#### **Step 2: Install pytest**

Now that your virtual environment is active, install `pytest` via `pip`:

```bash
pip install pytest
```

To confirm `pytest` is installed correctly, run:

```bash
pytest --version
```

This will print the version of `pytest` installed on your system. For example:

```
pytest 7.1.2
```

---

### **2.2 Writing Your First Test Case**

Let's write a simple test case and run it with `pytest` to ensure everything is working.

1. **Create a Python file** `test_example.py` in the project directory:
   
   ```python
   # test_example.py

   def test_addition():
       assert 1 + 2 == 3
   ```

   This is a very basic test case where we’re simply checking if `1 + 2` equals `3`.

2. **Run the test**:

   To run the test, simply use `pytest` from the command line in the folder containing `test_example.py`:

   ```bash
   pytest test_example.py
   ```

   Output:
   ```bash
   ============================= test session starts ==============================
   collected 1 item

   test_example.py .                                                           [100%]

   ============================== 1 passed in 0.03 seconds ==============================
   ```

   The test passed successfully. If the test fails, pytest will display more detailed information about what went wrong.

---

### **2.3 Understanding Test Discovery**

One of the coolest features of `pytest` is **test discovery**. You don’t need to explicitly tell `pytest` which test functions to run—it automatically discovers them by looking for files that match the pattern `test_*.py` or `*_test.py`, and functions that start with `test_`.

For example:
- `test_example.py`
- `test_math_operations.py`
- `test_my_feature.py`

This means that all you need to do is run `pytest` in the directory, and it will automatically discover and run all the tests it finds.

```bash
pytest
```

You don't need to specify individual test files. It will find all test files and run them.

---

### **2.4 Running Tests with Different Options**

You can run `pytest` with various options to tailor your testing experience:

- **Run tests with more verbose output**:

   ```bash
   pytest -v
   ```

   This will show more detailed output, including the names of the individual tests.

- **Run tests in a specific file or folder**:

   If you have multiple test files and want to run only specific ones, use:

   ```bash
   pytest test_example.py
   ```

   or, to run all tests in a folder:

   ```bash
   pytest tests/
   ```

- **Run tests and stop after the first failure**:

   If you want to stop at the first failure, use the `-x` option:

   ```bash
   pytest -x
   ```

---

### **2.5 Conclusion**

In this chapter, we've learned how to:
- Install `pytest` in a virtual environment
- Write and run your first test case
- Understand how `pytest` automatically discovers tests
- Use some common `pytest` command-line options

---