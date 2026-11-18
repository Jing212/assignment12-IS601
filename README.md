
# Assignment 12 - IS601: FastAPI Calculation API

## Project Overview

This project implements a RESTful Calculation API using FastAPI,
SQLAlchemy ORM, Pydantic, and PostgreSQL.

The application supports user registration, JWT-based authentication,
and BREAD operations (Browse, Read, Edit, Add, and Delete) for
managing calculations.

The project also uses Docker Compose for containerized application
deployment and GitHub Actions for automated testing and CI/CD.

## Features

- User registration and login
- JWT-based authentication and authorization
- User-specific calculation records
- Addition, subtraction, multiplication, and division
- SQLAlchemy polymorphic inheritance
- Factory design pattern for calculation creation
- Pydantic request and response validation
- Complete BREAD operations
- Updating calculation inputs and calculation types
- Division-by-zero validation
- PostgreSQL database persistence
- Database management through pgAdmin
- Docker containerization
- Automated unit, integration, and end-to-end testing
- GitHub Actions CI/CD pipeline

## Technologies Used

| Technology | Purpose |
| --- | --- |
| Python 3.11 | Application development |
| FastAPI | RESTful API framework |
| SQLAlchemy | Object-relational mapping |
| Pydantic | Data validation and serialization |
| PostgreSQL 17 | Relational database |
| pgAdmin | Database administration |
| JWT | Authentication and authorization |
| Docker | Application containerization |
| Docker Compose | Multi-container orchestration |
| Pytest | Automated testing |
| Playwright | End-to-end testing |
| GitHub Actions | Continuous integration and deployment |

## Application Architecture

```text
Client / Swagger UI
        |
        v
    FastAPI Application
        |
        +--- JWT Authentication
        |
        +--- Pydantic Validation
        |
        +--- Calculation Factory
        |
        v
    SQLAlchemy ORM
        |
        v
    PostgreSQL Database
        |
        +--- pgAdmin
```


## Prerequisites

Before running the application, make sure the following tools
are installed:

- Git
- Python 3.11
- Docker Desktop
- Docker Compose

## Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/Jing212/assignment12-IS601.git
cd assignment12-is601
```

### 2. Create a Virtual Environment

```bash
python3.11 -m venv venv
source venv/bin/activate
```

For Windows:

```bash
venv\Scripts\activate
```

### 3. Install Dependencies

```bash
python -m pip install --upgrade pip
pip install -r requirements.txt
```

### 4. Configure Environment Variables

Configure the required database credentials and JWT settings
according to the application configuration and Docker Compose file.

Sensitive information, such as passwords and secret keys,
should be stored in environment variables and should not be
committed to GitHub.

### 5. Build and Run the Application

Start Docker Desktop and run:

```bash
docker compose up -d --build
```

Check the container status:

```bash
docker compose ps
```

The application uses three main services:

| Service | Description | Port |
| --- | --- | --- |
| web | FastAPI application | 8000 |
| db | PostgreSQL database | 5432 |
| pgadmin | Database management | 5050 |

### 6. Access the Application

- Swagger API Documentation: http://localhost:8000/docs
- Health Check: http://localhost:8000/health
- pgAdmin: http://localhost:5050

### 7. Stop the Application

```bash
docker compose down
```

This stops and removes the containers while preserving named
database volumes.

## API Endpoints

### Health Check

| Method | Endpoint | Description |
| --- | --- | --- |
| GET | /health | Check application health |

### Authentication

| Method | Endpoint | Description |
| --- | --- | --- |
| POST | /auth/register | Register a new user |
| POST | /auth/login | Authenticate a user using JSON |
| POST | /auth/token | Obtain an access token for Swagger UI |

### Calculation BREAD Operations

| Operation | Method | Endpoint |
| --- | --- | --- |
| Browse | GET | /calculations |
| Read | GET | /calculations/{calc_id} |
| Edit | PUT | /calculations/{calc_id} |
| Add | POST | /calculations |
| Delete | DELETE | /calculations/{calc_id} |

All calculation endpoints require authentication.
Users can only access and modify their own calculation records.

## API Usage Examples

### 1. Register a User

Endpoint:

```text
POST /auth/register
```

Request body:

```json
{
  "username": "demo_user",
  "email": "demo@example.com",
  "first_name": "Demo",
  "last_name": "User",
  "password": "ExamplePass123!",
  "confirm_password": "ExamplePass123!"
}
```

Expected status code: `201 Created`

### 2. Login

Endpoint:

```text
POST /auth/login
```

Request body:

```json
{
  "username": "demo_user",
  "password": "ExamplePass123!"
}
```

Expected status code: `200 OK`

The application returns an access token and a refresh token.

Use the Authorize button in Swagger UI to authenticate before
accessing protected calculation endpoints.

### 3. Create a Calculation

Endpoint:

```text
POST /calculations
```

Request body:

```json
{
  "type": "addition",
  "inputs": [10.5, 3, 2]
}
```

Expected result:

```json
{
  "type": "addition",
  "inputs": [10.5, 3, 2],
  "result": 15.5
}
```

Expected status code: `201 Created`

### 4. Browse Calculations

Endpoint:

```text
GET /calculations
```

Retrieves calculation records belonging to the authenticated user.

Expected status code: `200 OK`

### 5. Read a Calculation

Endpoint:

```text
GET /calculations/{calc_id}
```

Retrieves a specific calculation using its unique UUID.

Expected status code: `200 OK`

### 6. Update a Calculation

Endpoint:

```text
PUT /calculations/{calc_id}
```

Request body:

```json
{
  "type": "division",
  "inputs": [42, 7]
}
```

Expected result:

```json
{
  "type": "division",
  "inputs": [42, 7],
  "result": 6.0
}
```

Expected status code: `200 OK`

### 7. Delete a Calculation

Endpoint:

```text
DELETE /calculations/{calc_id}
```

Deletes a calculation belonging to the authenticated user.

Expected status code: `204 No Content`

## Design Patterns

### Factory Pattern

The Calculation factory method creates the appropriate calculation
object based on the requested operation type.

Supported subclasses include:

- Addition
- Subtraction
- Multiplication
- Division

This design separates calculation creation from calculation execution
and improves maintainability.

### Polymorphic Inheritance

SQLAlchemy single-table inheritance allows different calculation
subclasses to share a common database table.

Each calculation subclass implements its own `get_result()` method.

The calculation type acts as the polymorphic discriminator, allowing
SQLAlchemy to identify the appropriate subclass.

## Project Enhancements

In addition to the original course implementation, the project
includes the following modifications:

- Enhanced the calculation update schema to accept changes to
  both calculation inputs and operation types.
- Updated the PUT endpoint to support changing an existing
  calculation from one operation type to another.
- Used the factory pattern to select the appropriate calculation
  subclass when changing operation types.
- Designed the update logic to preserve the original calculation
  ID and creation timestamp.
- Added rollback handling for calculation update errors.
- Updated project dependencies and Docker configuration.
- Updated Redis-related authentication configuration.
- Updated the GitHub Actions workflow.

## Database Management

PostgreSQL is used to persist application data.

The two main database tables are:

| Table | Purpose |
| --- | --- |
| users | Stores registered user information |
| calculations | Stores calculation types, inputs, results, and ownership |

The calculations table references the users table through a
foreign key relationship.

pgAdmin provides a graphical interface for inspecting database
tables, running SQL queries, and verifying calculation records.

## Testing

The project includes three categories of automated tests.

### Unit Tests

Test individual calculation operations and application logic.

### Integration Tests

Test database interactions, user authentication, schema validation,
and relationships between application components.

### End-to-End Tests

Use Playwright to test application behavior.

### Run All Tests

Ensure that the required test database and environment variables
are configured before running:

```bash
python -m pytest tests/ -v
```

### Run Tests with Coverage

```bash
python -m pytest tests/ -v \
  --cov=app \
  --cov-report=term-missing \
  --cov-fail-under=90
```

### Run Specific Test Categories

```bash
python -m pytest tests/unit/ -v
```

```bash
python -m pytest tests/integration/ -v
```

```bash
python -m pytest tests/e2e/ -v
```

## GitHub Actions CI/CD

The automated workflow is located at:

```text
.github/workflows/test.yml
```

GitHub Actions is used to automate project validation and
support a consistent development workflow.

The workflow can be monitored from the Actions tab of the
GitHub repository.

## Screenshots

### GitHub Actions Successful Workflow

<!-- Add successful GitHub Actions screenshot here. -->

### User Registration

<!-- Add registration screenshot here. -->

### User Login

<!-- Add login screenshot here. -->

### Create Calculation

<!-- Add POST calculation screenshot here. -->

### Browse Calculations

<!-- Add GET calculations screenshot here. -->

### Read Calculation

<!-- Add GET calculation by ID screenshot here. -->

### Update Calculation

<!-- Add PUT calculation screenshot here. -->

### Delete Calculation

<!-- Add DELETE calculation screenshot here. -->

### PostgreSQL Database and pgAdmin

<!-- Add database screenshot here if desired. -->

## Useful Docker Commands

| Command | Description |
| --- | --- |
| docker compose up -d --build | Build and start services |
| docker compose ps | Check container status |
| docker compose logs web | View application logs |
| docker compose logs db | View database logs |
| docker compose restart web | Restart the web service |
| docker images | List local Docker images |
| docker compose down | Stop and remove containers |

## Learning Outcomes

Through this assignment, I gained hands-on experience with:

- Developing RESTful APIs using FastAPI.
- Implementing secure user authentication using JWT.
- Managing relational databases using SQLAlchemy ORM.
- Applying factory patterns and polymorphic inheritance.
- Implementing complete BREAD operations.
- Validating requests and responses using Pydantic.
- Troubleshooting Docker networking and database connectivity.
- Managing PostgreSQL data using pgAdmin.
- Performing automated software testing.
- Configuring CI/CD workflows using GitHub Actions.

## Author

**Jing Jia**

IS601 - Assignment 12

## Acknowledgments

This project is based on the original IS601 Module 12 course
starter repository. Additional modifications were made as part
of the Assignment 12 implementation.


