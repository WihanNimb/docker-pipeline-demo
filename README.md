# 📚 Bookstore API

A simple RESTful API for managing books, built with **FastAPI** and **SQLAlchemy**. Supports basic CRUD operations, pagination, and configurable environment settings.

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

## 🐳 Run with Docker

### 🔨 Build the image

```bash
docker build -t bookstore-api .
```

### ▶️ Run the container

```bash
docker run -d -p 8080:8080 --name bookstore-api bookstore-api
```

The API will be accessible at: [http://localhost:8080/books](http://localhost:8080/books)

---

## ✅ Confirm the Application is Working

Once the Docker container is running, you can verify that the API is up and working as expected:

### 🌐 Access the Swagger UI

Open your browser and go to:

[http://localhost:8080/docs](http://localhost:8080/docs)

This is FastAPI’s interactive API documentation, where you can explore and test all endpoints (e.g. `/books/`, `/books/{book_id}`).

---

### 🧪 Test Endpoints via `curl`

#### 📘 Create a Book

```bash
curl -X POST http://localhost:8080/books/ \
    -H "Content-Type: application/json" \
    -d '{
        "title": "The Hitchhiker'\''s Guide to the Galaxy",
        "author": "Douglas Adams",
        "description": "A comedic science fiction series.",
        "price": 12.99
    }'
```

#### 📚 Get All Books

```bash
curl http://localhost:8080/books/
```

#### 🔎 Get a Book by ID (e.g., ID = 1)

```bash
curl http://localhost:8080/books/1
```

If these return valid JSON, your API is fully functional inside Docker.

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
