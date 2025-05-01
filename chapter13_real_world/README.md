### **Chapter 13: Real-World Testing with FastAPI**

In this chapter, we will focus on how to handle **real-world testing** scenarios using **FastAPI** and **pytest**. FastAPI is a modern web framework for building APIs with Python, and it integrates well with pytest for writing tests.

We'll cover:
1. Testing an **API** built with FastAPI.
2. Mocking external services in FastAPI tests.
3. Organizing tests for complex applications.
4. Integration testing with databases and external services.

---

### **13.1 Testing an API with FastAPI**

One of the most common real-world testing scenarios is interacting with an **API**. FastAPI makes it easy to build and test APIs, and it provides built-in support for testing with pytest.

#### **Step 1: Create a simple FastAPI app**

Let’s start by creating a simple FastAPI application with a `/users` endpoint.

```python
# app.py (FastAPI Example)

from fastapi import FastAPI
from pydantic import BaseModel
from typing import List

app = FastAPI()

class User(BaseModel):
    id: int
    name: str

# A simple in-memory "database"
fake_users_db = [
    {"id": 1, "name": "Alice"},
    {"id": 2, "name": "Bob"}
]

@app.get("/users", response_model=List[User])
def get_users():
    return fake_users_db
```

This app has a `/users` endpoint that returns a list of users in JSON format.

#### **Step 2: Write the test for the FastAPI app**

We’ll now write a test to verify that the `/users` endpoint returns the correct data.

```python
# chapter13_real_world/tests/test_api.py

import pytest
from fastapi.testclient import TestClient
from app import app

# Initialize the test client
client = TestClient(app)

def test_get_users():
    response = client.get("/users")
    assert response.status_code == 200
    users = response.json()
    assert len(users) == 2
    assert users[0]["name"] == "Alice"
    assert users[1]["name"] == "Bob"
```

#### **Explanation:**
- We use `TestClient` from `fastapi.testclient` to make requests to our FastAPI app.
- We verify that the `/users` endpoint returns the expected status code and data.

#### **Step 3: Run the test**

Make sure pytest is installed and run the test:

```bash
pytest chapter13_real_world/tests/test_api.py
```

---

### **13.2 Mocking External Services in FastAPI Tests**

In real-world applications, your API might depend on external services (e.g., third-party APIs, databases, etc.). During testing, we often **mock** these external services to avoid making real HTTP requests.

#### **Example: Mocking an External API with `unittest.mock`**

Let’s say that our FastAPI app calls an external API to fetch user details. During testing, we can mock that external API.

```python
# app.py (with external API call)

import requests
from fastapi import FastAPI

app = FastAPI()

EXTERNAL_API_URL = "https://external-api.com/users"

def fetch_external_data():
    response = requests.get(EXTERNAL_API_URL)
    return response.json()

@app.get("/users")
def get_users():
    data = fetch_external_data()  # External API call
    return data
```

#### **Step 1: Mocking the external API**

We will mock the `fetch_external_data` function to simulate the response from the external API.

```python
# chapter13_real_world/tests/test_api.py

import pytest
from unittest.mock import patch
from fastapi.testclient import TestClient
from app import app

# Initialize the test client
client = TestClient(app)

# Mock response data
mock_data = [{"id": 1, "name": "Alice"}, {"id": 2, "name": "Bob"}]

@pytest.fixture
def mock_external_api():
    with patch('app.fetch_external_data') as mock:
        mock.return_value = mock_data
        yield mock

def test_get_users(mock_external_api):
    response = client.get("/users")
    assert response.status_code == 200
    assert len(response.json()) == 2
    assert response.json()[0]["name"] == "Alice"
    assert response.json()[1]["name"] == "Bob"
```

#### **Explanation:**
- We use `unittest.mock.patch` to mock the `fetch_external_data` function in the app.
- The mock version of `fetch_external_data` returns `mock_data`, simulating an external API response.
- The test verifies that the `/users` endpoint returns the mocked data.

---

### **13.3 Organizing Tests for Complex FastAPI Applications**

For real-world FastAPI applications, especially when they grow in complexity, it’s important to organize tests properly. This makes it easier to maintain and scale your tests.

Here’s how we might structure the tests for a more complex application.

```plaintext
pytest-learning/
├── chapter13_real_world/
│   ├── api/
│   │   └── app.py
│   └── tests/
│       ├── __init__.py
│       ├── test_api.py
│       └── test_database.py
└── requirements.txt
```

- **`api/`**: Contains the FastAPI application code.
- **`tests/`**: Contains test files, with each file focused on testing a specific part of the application (e.g., `test_api.py` for API tests, `test_database.py` for database-related tests).
- **`requirements.txt`**: List of dependencies, including `pytest`, `fastapi`, `requests`, and `pytest-mock`.

#### **Step 1: Testing a database-backed FastAPI app**

Suppose your FastAPI app uses a database like SQLAlchemy. Here’s an example of how to write integration tests for such an app.

```python
# app.py (FastAPI with SQLAlchemy)

from fastapi import FastAPI
from sqlalchemy import Column, Integer, String, create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

SQLALCHEMY_DATABASE_URL = "sqlite:///:memory:"  # In-memory database for testing

Base = declarative_base()
engine = create_engine(SQLALCHEMY_DATABASE_URL, connect_args={"check_same_thread": False})
SessionLocal = sessionmaker(autocommit=False, autoflush=False, bind=engine)

app = FastAPI()

class User(Base):
    __tablename__ = "users"
    id = Column(Integer, primary_key=True, index=True)
    name = Column(String, index=True)

Base.metadata.create_all(bind=engine)

@app.get("/users")
def get_users():
    db = SessionLocal()
    users = db.query(User).all()
    db.close()
    return users
```

#### **Step 2: Writing an integration test with the database**

```python
# chapter13_real_world/tests/test_database.py

import pytest
from fastapi.testclient import TestClient
from app import app, SessionLocal, User, engine, Base

# Initialize the test client
client = TestClient(app)

@pytest.fixture(scope="module")
def db():
    # Create tables
    Base.metadata.create_all(bind=engine)
    db = SessionLocal()
    yield db
    db.close()
    # Drop tables after tests
    Base.metadata.drop_all(bind=engine)

def test_get_users_with_database(db):
    # Insert test data
    user1 = User(name="Alice")
    user2 = User(name="Bob")
    db.add(user1)
    db.add(user2)
    db.commit()

    # Test the /users endpoint
    response = client.get("/users")
    assert response.status_code == 200
    users = response.json()
    assert len(users) == 2
    assert users[0]["name"] == "Alice"
    assert users[1]["name"] == "Bob"
```

#### **Explanation:**
- **`db` fixture**: This fixture sets up an in-memory database and creates the necessary tables before tests run.
- We insert sample users into the database and then verify that the `/users` endpoint returns the correct data.

---

### **13.4 Conclusion**

In this chapter, we:
- Tested **real-world FastAPI applications** like APIs and database interactions using pytest.
- Learned to mock **external services** like HTTP APIs in FastAPI tests.
- Structured tests for **complex FastAPI applications**.
- Wrote **integration tests** for database-backed FastAPI applications.

Real-world testing is an essential skill for maintaining the quality of complex applications. By using pytest along with FastAPI, we can ensure that our applications are reliable and robust.