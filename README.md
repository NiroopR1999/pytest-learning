# 🧪 Pytest Learning Journey

Welcome to the **Pytest Learning Repository**! This project is structured in progressive chapters to help you go from beginner to advanced with `pytest`, Python's most popular testing framework.

---

## 📚 Chapters Overview

| Chapter | Topic                                      |
|--------:|--------------------------------------------|
| 01     | Introduction to Testing in Python          |
| 02     | Installing and Setting Up Pytest           |
| 03     | Writing Basic Tests                        |
| 04     | Parametrization and Fixtures               |
| 05     | Organizing Tests                           |
| 06     | Test Coverage and Reporting                |
| 07     | Mocking and Patching                       |
| 08     | Testing Exceptions and Warnings            |
| 09     | Advanced Fixtures                          |
| 10     | Plugins and Extensions                     |
| 11     | Testing Asynchronous Code                  |
| 12     | Integrating with CI/CD Pipelines           |
| 13     | Real-World Test Scenarios (API, DB, etc.)  |
| 14     | Debugging and Best Practices               |

---

## 🚀 Getting Started

### 📦 Requirements
- Python 3.8+
- pip

### 📥 Setup Instructions

```bash
# Create a virtual environment (optional but recommended)
python -m venv venv
venv\Scripts\activate   # On Windows

# Install pytest and useful tools
pip install pytest pytest-cov
```

### ✅ Running Tests

You can run tests for any chapter by navigating to its folder:

```bash
cd chapter03_basic_tests
pytest
```

To run tests with coverage:

```bash
pytest --cov=src
```

---

## 🏁 Goals

By the end of this course, you will be able to:
- Write clear, maintainable, and scalable test cases
- Use fixtures effectively for setup/teardown
- Mock external dependencies
- Integrate your tests with CI/CD tools
- Apply best practices for real-world testing

---

## 📌 Notes

This repo is for learning and experimenting. Feel free to modify, break, and rebuild things to deepen your understanding.

---

pytest-learning/
├── README.md
├── chapter01_intro_to_testing/
│   └── test_basics.py
├── chapter02_setup_pytest/
│   └── test_setup.py
├── chapter03_basic_tests/
│   └── test_math_operations.py
├── chapter04_parametrize_and_fixtures/
│   ├── test_parametrize.py
│   └── test_fixtures.py
├── chapter05_organizing_tests/
│   ├── test_grouped.py
│   └── tests/
│       ├── __init__.py
│       └── test_nested.py
├── chapter06_coverage_reporting/
│   ├── src/
│   │   └── calculator.py
│   └── tests/
│       └── test_calculator.py
├── chapter07_mocking/
│   └── test_mocking.py
├── chapter08_exceptions/
│   └── test_exceptions.py
├── chapter09_advanced_fixtures/
│   └── test_advanced_fixtures.py
├── chapter10_plugins/
│   └── conftest.py
├── chapter11_async_testing/
│   └── test_async_code.py
├── chapter12_ci_cd/
│   └── .github/workflows/pytest.yml
├── chapter13_real_world/
│   ├── api/
│   │   └── app.py
│   └── tests/
│       └── test_api.py
└── chapter14_best_practices/
    └── test_best_practices.py
