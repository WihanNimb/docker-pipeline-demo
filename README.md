# 📚 Bookstore API

A simple RESTful API for managing books, built with **FastAPI** and **SQLAlchemy**. Supports basic CRUD operations, pagination, and configurable environment settings.

---

## 📑 Table of Contents

- [⚙️ Configuration](#️-configuration)
- [🐳 Running Locally with Docker](#-running-locally-with-docker)
- [🎯 Deploying with Helm (Tested with Minikube)](#-deploying-with-helm-tested-with-minikube)
- [🚀 CI/CD Pipeline](#-cicd-pipeline)
- [✅ Testing the API](#-testing-the-api)
- [🧾 API Endpoints](#-api-endpoints)
- [📝 Additional Notes](#additional-notes)

---

## ⚙️ Configuration

The application reads configuration via environment variables. Defaults are used if not explicitly set:

| Variable          | Default                                  | Description                                                      |
|-------------------|------------------------------------------|------------------------------------------------------------------|
| `DATABASE_URL`    | `sqlite:///./books.db`                   | SQLAlchemy database connection string                            |
| `LOG_LEVEL`       | `INFO`                                   | Logging level (`DEBUG`, `INFO`, `WARNING`, etc.)                 |
| `LOG_FORMAT`      | `%(levelname)s:%(name)s:%(message)s`     | Python `logging` format string                                   |
| `PAGE_SIZE`       | `10`                                     | Pagination size for `/books/` endpoint                           |
| `APP_ENV`         | `dev`                                    | Environment label (`dev`, `staging`, `prod`)                     |
| `HOST`            | `0.0.0.0`                                | Host address for Uvicorn                                         |
| `PORT`            | `8080`                                   | Port to run the API on                                           |
| `RELOAD`          | `False`                                  | Uvicorn live reload flag (`True`/`False`)                        |
| `ALLOWED_ORIGINS` | `*`                                      | CORS settings (comma-separated origins)                          |
| `DB_POOL_SIZE`    | `5`                                      | SQLAlchemy DB connection pool size                               |
| `DB_MAX_OVERFLOW` | `10`                                     | SQLAlchemy max overflow connections                              |

---

## 🐳 Running Locally with Docker

### Build the Docker image

```bash
docker build -t bookstore-api .
```

### Run the container

```bash
docker run -d -p 8080:8080 --name bookstore-api bookstore-api
```

The API will be accessible at: [http://localhost:8080/books](http://localhost:8080/books)

---

## 🎯 Deploying with Helm (Tested with Minikube)

This project includes a Helm chart for deploying the Bookstore API on Kubernetes.

### Prerequisites

- Kubernetes cluster (Minikube used for testing)
- Helm installed

### Steps to deploy with Minikube:

1. Start Minikube if not already running:

```bash
minikube start
```

2. Enable ingress addon:

```bash
minikube addons enable ingress
```

3. Add the following entries to your `/etc/hosts` file to map the ingress hostname:

```
192.168.67.2 bookstore.local
127.0.0.1 bookstore.local
```

*(Adjust `192.168.67.2` to your Minikube IP — find it with `minikube ip`)*

4. Install the Helm chart with environment values (adjust paths as needed):

```bash
helm install bookstore ./helm/bookstore-api -f ./helm/bookstore-api/env/dev.yaml
```

5. Verify the pods, services, and ingress are running:

```bash
kubectl get pods,svc,ingress
```

6. Run the Minikube tunnel in a separate terminal to enable LoadBalancer services:

```bash
minikube tunnel
```

7. Access the API docs in your browser:

```
http://bookstore.local/docs
```

### Customize your deployment

Edit the values files (e.g., `./helm/env/dev.yaml`) to update environment variables, image tags, replica count, ingress hostnames, etc.

---

## 🚀 CI/CD Pipeline

A pipeline is included for:

- Building the Docker image on every commit
- Pushing the image automatically to Docker Hub

See `.github/workflows/` for pipeline details.

---

## ✅ Testing the API

### Access Swagger UI

- Locally with Docker: [http://localhost:8080/docs](http://localhost:8080/docs)
- Deployed via Helm: [http://bookstore.local/docs](http://bookstore.local/docs)

### Example `curl` commands

Create a book:

```bash
curl -X POST http://localhost:8080/books/     -H "Content-Type: application/json"     -d '{
        "title": "The Hitchhiker'''s Guide to the Galaxy",
        "author": "Douglas Adams",
        "description": "A comedic science fiction series.",
        "price": 12.99
    }'

curl -X POST http://bookstore.local/books/     -H "Content-Type: application/json"     -d '{
        "title": "The Hitchhiker'''s Guide to the Galaxy",
        "author": "Douglas Adams",
        "description": "A comedic science fiction series.",
        "price": 12.99
    }'
```

List all books:

```bash
curl http://localhost:8080/books/

curl http://bookstore.local/books/
```

Get a book by ID (e.g., ID = 1):

```bash
curl http://localhost:8080/books/1

curl http://bookstore.local/books/1
```

---

## 🧾 API Endpoints

| Method | Endpoint          | Description              |
|--------|-------------------|--------------------------|
| POST   | `/books/`         | Create a new book        |
| GET    | `/books/`         | List books (paginated)   |
| GET    | `/books/{id}`     | Get a book by ID         |
| PUT    | `/books/{id}`     | Update an existing book  |
| DELETE | `/books/{id}`     | Delete a book            |

---

## 📝 Additional Notes

- Minikube requires `/etc/hosts` update and `minikube tunnel` to expose LoadBalancer services.
- Helm chart supports environment-specific customization.
- The CI/CD pipeline automates docker image builds.

---
