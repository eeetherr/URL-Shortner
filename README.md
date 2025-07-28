# URL Shortener

A high-performance, containerized URL shortener built with Go, Fiber, and Redis. This project provides a simple and efficient REST API to create short links and redirect users to the original URL.

## ✨ Features

* **Fast URL Shortening**: Quickly generate a unique, short ID for any long URL.
* **Seamless Redirection**: Automatically redirects users from the short link to the original destination.
* **Click Counter**: Tracks the number of times a shortened link is used.
* **RESTful API**: Simple and clean API endpoints for creating and resolving links.
* **Containerized**: Fully containerized with Docker and Docker Compose for easy setup and deployment.
* **Scalable Architecture**: Microservice-based design with separate API and database services.

## 🛠️ Tech Stack & Architecture

This project uses a microservice architecture, orchestrated by Docker Compose.

* **Backend API**: Written in **Go** using the **Fiber** web framework for its high performance.
* **Database**: **Redis** is used as an in-memory key-value store for fast read/write operations, perfect for mapping short URLs to their original counterparts.
* **Containerization**: **Docker** and **Docker Compose** are used to build and run the application services (`api` and `db`) in isolated containers.

The application is split into two main services:

1.  **`api`**: The Go service that handles all incoming HTTP requests, business logic, and communication with the database.
2.  **`db`**: The Redis service responsible for storing and retrieving URL data.

## ⚙️ API Endpoints

### 1. Shorten a URL

Send a `POST` request with the URL you want to shorten in the request body.

* **Endpoint**: `POST /api/v1`
* **Headers**: `Content-Type: application/json`
* **Body** (JSON):
    ```json
    {
      "url": "[https://github.com/gofiber/fiber](https://github.com/gofiber/fiber)"
    }
    ```
* **Success Response** (`200 OK`):
    ```json
    {
        "url": "http://localhost:3000/G3hD4kFp",
        "custom_short": "G3hD4kFp",
        "expiry": "24h0m0s",
        "x_rate_remaining": 9,
        "x_rate_limit_reset": "59m58s"
    }
    ```

### 2. Resolve and Redirect

To use the shortened URL, simply navigate to it in your browser or use a `GET` request.

* **Endpoint**: `GET /:url`
* **Example**: `http://localhost:3000/G3hD4kFp`

The server will respond with a `301 Moved Permanently` status code and redirect you to the original URL (`https://github.com/gofiber/fiber`).

## 🔧 Configuration

All application configuration is managed through environment variables located in the `api/.env` file.

* `APP_PORT`: The port on which the Go API service will run.
* `DOMAIN`: The base domain for the shortened URLs.
* `DB_ADDR`: The address of the Redis database service (set to the Docker service name `db:6379`).
* `DB_PASS`: The password for the Redis database (if any).
