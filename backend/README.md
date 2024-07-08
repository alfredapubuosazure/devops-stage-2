Backend - FastAPI with PostgreSQL
This directory contains the backend of the application built with FastAPI and a PostgreSQL database.

Prerequisites
Python 3.8 or higher
Poetry (for dependency management)
PostgreSQL (ensure the database server is running)
Installing Poetry
To install Poetry, follow these steps:

bash
Copy code
curl -sSL https://install.python-poetry.org | python3 -
Add Poetry to your PATH (if not automatically added):

bash
Copy code
export PATH="$HOME/.poetry/bin:$PATH"
Verify the installation:

bash
Copy code
poetry --version
Setup Instructions
Manually
Clone the repository:

bash
Copy code
git clone https://github.com/hngprojects/devops-stage-2.git
cd devops-stage-2/backend
Install dependencies using Poetry:

bash
Copy code
poetry install
Configure the Database:

Ensure PostgreSQL is installed and running.
Create a new PostgreSQL database:
bash
Copy code
sudo -u postgres psql
CREATE DATABASE mydatabase;
CREATE USER myuser WITH PASSWORD 'mypassword';
ALTER ROLE myuser SET client_encoding TO 'utf8';
ALTER ROLE myuser SET default_transaction_isolation TO 'read committed';
ALTER ROLE myuser SET timezone TO 'UTC';
GRANT ALL PRIVILEGES ON DATABASE mydatabase TO myuser;
\q
Update the .env file:

bash
Copy code
cp .env.example .env
Modify the .env file to include your database credentials:

dotenv
Copy code
DATABASE_URL=postgresql://myuser:mypassword@localhost/mydatabase
Set up the database with the necessary tables:

bash
Copy code
poetry run bash ./prestart.sh
Run the backend server:

bash
Copy code
poetry run uvicorn app.main:app --reload
Using Docker
Install Docker:
Follow the instructions on the Docker website to install Docker on your machine.

Clone the repository:

bash
Copy code
git clone https://github.com/hngprojects/devops-stage-2.git
cd devops-stage-2/backend
Create a Dockerfile:
Ensure the repository contains a Dockerfile with the following content:

dockerfile
Copy code
FROM python:3.8-slim

WORKDIR /app

COPY pyproject.toml poetry.lock ./
RUN pip install poetry && poetry install --no-dev

COPY . .

CMD ["poetry", "run", "uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]
Create a Docker Compose file:
Ensure the repository contains a docker-compose.yml file with the following content:

yaml
Copy code
version: '3.1'

services:
  db:
    image: postgres:13
    environment:
      POSTGRES_USER: myuser
      POSTGRES_PASSWORD: mypassword
      POSTGRES_DB: mydatabase
    volumes:
      - postgres-data:/var/lib/postgresql/data
    ports:
      - "5432:5432"

  web:
    build: .
    command: poetry run uvicorn app.main:app --host 0.0.0.0 --port 8000
    volumes:
      - .:/app
    ports:
      - "8000:8000"
    depends_on:
      - db

volumes:
  postgres-data:
Update the .env file:

bash
Copy code
cp .env.example .env
Modify the .env file to include your database credentials:

dotenv
Copy code
DATABASE_URL=postgresql://myuser:mypassword@db/mydatabase
Build and run the Docker containers:

bash
Copy code
docker-compose up --build
Verify the containers are running:

bash
Copy code
docker ps
