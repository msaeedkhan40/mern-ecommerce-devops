# E Commerce Pro

A full stack MERN e commerce application with Docker based deployment.

## Project Overview

E Commerce Pro is a responsive e commerce platform built using MongoDB, Express.js, React, and Node.js.

The project includes user authentication, product browsing, shopping cart management, order management, an admin dashboard, and image uploads.

## Technology Stack

| Component | Technology |
|---|---|
| Frontend | React, Redux Toolkit, Tailwind CSS |
| Backend | Node.js, Express.js |
| Database | MongoDB 6.0 |
| Database ODM | Mongoose |
| Web Server | NGINX |
| Containerization | Docker |
| Container Management | Docker Compose |
| Authentication | JWT and bcryptjs |

## Core Features

### Customer Features

1. User registration and login.
2. Product browsing and category filtering.
3. Shopping cart with persistent state.
4. Shipping information management.
5. Order history and profile management.
6. Product reviews.

### Admin Features

1. Product creation, editing, and deletion.
2. Product image uploads.
3. Order management.
4. Customer management.

### DevOps Features

1. Separate Docker images for frontend and backend.
2. Multi container application setup using Docker Compose.
3. NGINX reverse proxy for frontend and backend communication.
4. MongoDB running inside a Docker container.
5. Persistent Docker volumes for database data and uploaded images.
6. Environment variable based secret configuration.

## Application Architecture

The application runs through three main services:

1. **Frontend:** React application served by NGINX on port 80 inside its container.
2. **Backend:** Express API running on port 5000 inside its container.
3. **MongoDB:** Database available internally to the backend through the Docker network.

NGINX serves the frontend and forwards API requests to the backend.

The MongoDB port is not exposed to the host machine.

## Prerequisites

Install the following tools:

1. Git
2. Docker Engine
3. Docker Compose plugin
4. OpenSSL

Verify Docker installation:

```bash
docker --version
docker compose version
```

## Run with Docker Compose

### 1. Clone the repository

```bash
git clone git@github.com:msaeedkhan40/mern-ecommerce-devops.git
cd mern-ecommerce-devops
```

You can also clone using HTTPS:

```bash
git clone https://github.com/msaeedkhan40/mern-ecommerce-devops.git
cd mern-ecommerce-devops
```

### 2. Create the environment file

Create a root `.env` file containing a secure JWT secret:

```bash
printf "JWT_SECRET=%s\n" "$(openssl rand -hex 32)" > .env
chmod 600 .env
```

The `.env` file is excluded from Git to protect secrets.

Required variable:

| Variable | Description |
|---|---|
| `JWT_SECRET` | Secret used to sign authentication tokens |

### 3. Build and start the containers

```bash
docker compose up -d --build
```

This command builds the frontend and backend images and starts all application services.

### 4. Check container status

```bash
docker compose ps
```

The MongoDB service should become healthy, and the backend and frontend should be running.

### 5. Access the application

Open the following address in your browser:

http://localhost:8080

The frontend is served through NGINX, which forwards API requests to the backend.

## Useful Docker Commands

View running services:

```bash
docker compose ps
```

View logs from all services:

```bash
docker compose logs
```

View backend logs:

```bash
docker compose logs backend
```

View MongoDB logs:

```bash
docker compose logs mongo
```

Restart services:

```bash
docker compose restart
```

Stop the application:

```bash
docker compose down
```

Rebuild application images:

```bash
docker compose up -d --build
```

## Persistent Data

Docker Compose uses two named volumes:

| Volume | Purpose |
|---|---|
| `mongo_data` | Stores MongoDB database files |
| `backend_uploads` | Stores uploaded product images |

These volumes preserve application data when containers are restarted or removed using `docker compose down`.

**Important:** Avoid `docker compose down -v` unless you intentionally want to delete the persistent volumes and their data.

## Network Configuration

The application uses an internal Docker network for communication between services.

1. Frontend traffic enters through port 8080 on the host.
2. NGINX forwards `/api/` requests to the backend service.
3. NGINX forwards `/uploads/` requests to the backend service.
4. Backend communicates with MongoDB using the internal service name `mongo`.

MongoDB is not published directly to the host.

## Current Configuration Notes

1. The application is configured for local Docker based testing.
2. The frontend is available at `http://localhost:8080`.
3. The current Compose configuration does not include Stripe payment credentials.
4. Production deployment would require additional security, payment verification, HTTPS, and deployment configuration.

## Repository

GitHub: https://github.com/msaeedkhan40/mern-ecommerce-devops

## License

See the [LICENSE](LICENSE) file for license information.
