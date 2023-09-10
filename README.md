# Full-Stack React FastAPI CRUD - Login Page

## Project Description

This project is showcasing a full-stack development using React, FastAPI, PostgreSQL, and Python. It includes CRUD operations (Create, Read, Update, Delete) with token-based authentication for secure user access.
Understood, here's a revised version:

## 🚀 Update as of September 11, 2023:
+ The project features CRUD (Create, Read, Update, Delete) operations, __with the "Delete" operation pending implementation__ of the user role feature to grant superuser authority. As of now, the project demonstrates CRU operations with token-based authentication.

## Project Features

- **CRUD Operations:** Create, Read, Update, and Delete (In the future) user profiles.
- **Token-Based Authentication:** Secure user access using JWT (JSON Web Tokens).
- **React Frontend:** Simple UI by React.
- **FastAPI Backend** 
- **Database:** PostgreSQL database for storing user credentials.

## Getting Started

Below are the steps to get started:

### Prerequisites

- Node.js and npm installed
- Python 3.9+
- PostgreSQL database
- Poetry installed and added to Path. Follow these steps for installation (Windows OS):
    + Open Command Prompt: Press Win + R, type cmd, and press Enter.
    + Run this command to install Poetry:
      ```
      pip install poetry
      ```
    + Add Poetry to System PATH:
        - Search for "Environment Variables" in the Windows Start menu and select "Edit the system environment variables."
        - In the System Properties window, click the "Environment Variables" button.
        - Under "System variables," scroll down to find the "Path" variable and select it. Then click the "Edit" button.
        - In the Edit Environment Variable window, click the "New" button and add the path to Poetry's bin folder. By default, it should be something like (Replace <YourUSername> with Windows username):
          ```
          C:\Users\<YourUsername>\AppData\Roaming\Python\Scripts
          ```
        - Click "OK" to close the windows and save the changes.
        - Verify Poetry Installation: Open a new Command Prompt window and run the following command to verify that Poetry is correctly installed and added to your PATH:
          ```
          poetry --version
          ```

#### 1. Installation

Clone the repository:

   ```bash
   git clone https://github.com/ArdaniahJ/FullStack_React_FastAPI_CRUD.git
   ```
#### 2. Creating a Database in PostgreSQL
1. Prior to deploy the backend scripts, a database named `test` has to be created in PSQL.
2. These SQL Queries to create user below are optional as superuser credentials can be used (commands can be executed through cmd or PSQLAdmin: pgAdmin). Database 'test' is created for this 'user'.
  - Login to PostgreSQL in terminal using superuser credentials:
    ```sql
    psql -U postgres -d postgres
    ```
  - Create user (replace user with your username and your_password with your password):
    ```sql
    CREATE USER user WITH PASSWORD 'your_password';
    ```
  - Verify user 'user' exist in PSQL db:
    ```sql
    psql -U postgres -c "SELECT usename FROM pg_user WHERE usename = 'user';"
    ```
  - Create database named 'test' that belongs to user:
    ```sql
    CREATE DATABASE test WITH OWNER = user;
    ```
  - Grant privileges to 'user' so that user can access and work with 'test' db.
    ```sql
    GRANT ALL PRIVILEGES ON DATABASE test TO user;
    ```
  - Check if 'test' db exist and belongs to user 'user'.
    ```sql
    SELECT datname, datdba FROM pg_database WHERE datname = 'test';
    ```

#### 3. Starting Backend Server With Poetry
Below is the directory tree of backend:
```
backend/
├── app/
│   ├── controller/      # manage the REST interface to the business logic
│   ├── repository/      # storage of the entity/model bean in system
│   ├── model/           # data structure like table from ERD
│   └── service/         # business logic implementation
└── media/               # users default photo when register
```

1. Navigate to the backend directory:
  ```bash
  cd backend
  ```
2. Activate virtual environments
   - The backend server requires activating __two virtual environments__. This process might seem a bit redudant, but it serves a specific purpose:
   - __Python venv__: When a Python venv is created, it'll __isolates it Python dependencies__ to avoid conflict with the global Python env.
   - __Poetry venv__: Poetry on the other hand, __manages the project's dependencies including those within the venv__. It installs project dependencies into the Python venv created earlier and keeps track of them into the `pyproject.toml` file
3. Activate the Python virtual environment. If it fails, delete the venv and create a new one named 'venv' in backend directory. Activate it:
   ```bash
   python -m venv venv
   .\venv\Scripts\activate
   ```
4. Activate the Poetry virtual envinronment.
   - While in `backend` directory, activate poetry venv using this command:
     ```bash
     poetry shell
     ```
   - Install the dependencies from __pyproject.toml:__
     ```bash
     poetry install
     ```
   - Run Alembic Migration: Use Alembic to apply any db schema changes. Run the following command where `alembic.ini` is located. This command will create/update the db schema as needed.
     ```bash
     alembic upgrade head
     ```
   - Start the backend server (the 'uvicorn' command isn't used separately in this case since it's included in 'main.py'):
     ```bash
     poetry run start
     ```
  5. Open default browser, and navigate to `http://localhost:8888/docs` to access the __FastAPI Swagger UI__ locally.
  6. Test the backend API endpoints. Below is a set of user credentials in JSON format that was tested during development. Modified accordingly except for phone number.
<br> 📝__Note___: The phone number must be in the format: `+6012-3726630`. The `regex and validator` still needs some improvements. <br>
 
  ```
  {
  "username": "Maria_Smith",
  "email": "maria.smith@example.com",
  "name": "Maria Smith",
  "password": "StrongPass456",
  "phone_number": "+8012-3726630`",
  "birth": "15-05-1985", 
  "sex": "FEMALE",
  "profile": "base64"
  }
  ```

8. Validate the user creation in PgAdmin or DBVisualizer (or any tools to your taste)
   ```
   ├── Tables/
   │   ├── person/
   │   │   ├── role/
   │   │   ├── user_role/
   │   │   ├── users/
   │   │   ├── alembic_version/ 
   │   └── ...
   ```


#### 4. Frontend Deployment With NPM
Below is the tree directory for frontend:
```
frontend/
├── public/
│   ├── ... (other public files)
│
├── src/
│   ├── app.css
│   ├── app.js
│   ├── app.test.js
│   ├── home.js
│   ├── index.css
│   └── index.js
│
├── package-lock.json
├── package.json
├── postcss.config.js
└── tailwind.config.js
```
1. Open a separate terminal
2. Navigated to the frontend directory:
   ```bash
   cd frontend
   ```
3. Install JavaScript dependencies in package.json using npm:
   ```bash
   npm install
   ```
4. Start the React development server:
   ```bash
   npm start
   ```
5. Open default browser and visit `http://localhost:3000` to access the deployed frontend application.
6. Test the UI features with below credentials or any other fake user as long as it follows below format:
   ```bash
    Username: Maria_Smith,
    Email: maria.smith@example.com
    Name: Maria Smith
    Password: StrongPass456
    Phone number: +8012-3726630
    Birth Date: 15-05-1985 # user can also enter birth date in the format "dd-mm-yyyy" in the input field.
    Gender: Female
   ```
  


   
