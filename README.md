# Dockerized Todo App

A Django-based Todo List application containerized with Docker and
Docker Compose. The project demonstrates how to package a web
application and its database into separate containers and run them
together in an isolated environment.

## Overview

The application consists of two main services:

-   **Python Application** --- runs the Django Todo List application.
-   **MySQL Database** --- stores application data in a dedicated
    database container.

Docker Compose is used to define and manage both services, their
network, port mappings, and persistent database storage.

## Technologies Used

-   **Python 3.8**
-   **Django**
-   **Docker**
-   **Docker Compose**
-   **MySQL**
-   **Linux**

## Project Structure

``` text
Dockerization_todoapp/
├── accounts/             # User account functionality
├── api/                  # Application API
├── lists/                # Todo list functionality
├── todolist/             # Django project configuration
├── .github/
│   └── workflows/        # GitHub Actions workflows
├── Dockerfile            # Django application image
├── Dockerfile.mysql      # MySQL image configuration
├── docker-compose.yml    # Multi-container configuration
├── manage.py             # Django management utility
├── requirements.txt      # Python dependencies
└── README.md
```

## Architecture

The application runs using two Docker containers connected through a
dedicated Docker network.

``` text
              ┌──────────────────────┐
              │       Browser        │
              │  http://localhost:8080│
              └──────────┬───────────┘
                         │
                         ▼
              ┌──────────────────────┐
              │    Python / Django   │
              │      Container       │
              │      Port: 8080      │
              └──────────┬───────────┘
                         │
                         ▼
              ┌──────────────────────┐
              │    MySQL Database    │
              │      Container       │
              │      Port: 3306      │
              └──────────┬───────────┘
                         │
                         ▼
              ┌──────────────────────┐
              │   Docker Volume      │
              │   Persistent Data    │
              └──────────────────────┘
```

## Getting Started

### Prerequisites

Make sure the following tools are installed:

-   Docker
-   Docker Compose (or Docker Compose V2)

### 1. Clone the repository

``` bash
git clone https://github.com/VadymMelnyk/Dockerization_todoapp.git
cd Dockerization_todoapp
```

### 2. Build and start the containers

Run the following command:

``` bash
docker compose up --build -d
```

This command builds the application and database images and starts both
services in detached mode.

### 3. Check running containers

``` bash
docker compose ps
```

### 4. Access the application

Open your browser and navigate to:

``` text
http://localhost:8080
```

### 5. View container logs

To view logs from all services:

``` bash
docker compose logs -f
```

To view logs from a specific service:

``` bash
docker compose logs -f pythonapp
```

``` bash
docker compose logs -f mysql
```

## Docker Configuration

### Application Container

The application Dockerfile uses a multi-stage build approach:

-   Uses Python 3.8 as the base image.
-   Sets `/app` as the working directory.
-   Installs dependencies from `requirements.txt`.
-   Exposes port `8080`.
-   Runs Django database migrations before starting the application.

### MySQL Container

The MySQL service:

-   Uses the official MySQL Docker image.
-   Creates the application database and database user.
-   Exposes port `3306`.
-   Uses a named Docker volume to persist database data.

### Docker Compose

The `docker-compose.yml` file defines:

  Service     Image                 Container Port   Host Port
  ----------- ------------------- ---------------- -----------
  pythonapp   todoapp:2.0.0                   8080        8080
  mysql       mysql-local:1.0.0               3306        3306

Both containers communicate through the `todoapp-net` Docker network.

Database data is stored in the `todoapp-db-data` named volume.

## Managing the Application

Stop and remove containers:

``` bash
docker compose down
```

Stop containers and remove the persistent database volume:

``` bash
docker compose down -v
```

**Warning:** The second command permanently deletes the database volume
and its stored data.

Rebuild the application after making changes:

``` bash
docker compose up --build -d
```

## What I Practiced

Through this project, I gained practical experience with:

-   Writing Dockerfiles to containerize a Django application.
-   Building custom Docker images.
-   Running multiple services with Docker Compose.
-   Configuring container networking.
-   Managing persistent data with Docker volumes.
-   Configuring a MySQL database container.
-   Understanding container dependencies and service communication.

## License

This project is licensed under the MIT License. See the
[LICENSE](LICENSE) file for details.
