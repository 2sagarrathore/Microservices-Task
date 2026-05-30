# Microservices Containerization Assessment

> **GitHub Repository:** https://github.com/2sagarrathore/Microservices-Task

## Overview

This project containerizes four Node.js microservices using Docker and Docker Compose.

| Service | Port | Description |
|---------|------|-------------|
| user-service | 3000 | Returns user data |
| product-service | 3001 | Returns product catalog |
| order-service | 3002 | Manages orders |
| gateway-service | 3003 | API gateway routing to all services |

## Project Structure

```
Microservices/
├── user-service/
│   ├── Dockerfile
│   ├── .dockerignore
│   ├── app.js
│   └── package.json
├── product-service/
│   ├── Dockerfile
│   ├── .dockerignore
│   ├── app.js
│   └── package.json
├── order-service/
│   ├── Dockerfile
│   ├── .dockerignore
│   ├── app.js
│   └── package.json
├── gateway-service/
│   ├── Dockerfile
│   ├── .dockerignore
│   ├── app.js
│   └── package.json
└── docker-compose.yml
```

**Submission folder:**
```
submission/
├── user-service/
│   └── Dockerfile
├── product-service/
│   └── Dockerfile
├── order-service/
│   └── Dockerfile
├── gateway-service/
│   └── Dockerfile
├── docker-compose.yml
└── README.md
```

## Prerequisites

- [Docker](https://docs.docker.com/get-docker/) (v20+)
- [Docker Compose](https://docs.docker.com/compose/install/) (v2+)

## Setup Instructions

```bash
git clone https://github.com/mohanDevOps-arch/Microservices-Task.git
cd Microservices-Task/Microservices
docker compose build
docker compose up -d
```

## How to Test Services

```bash
# Individual services
curl http://localhost:3000/users
curl http://localhost:3001/products
curl http://localhost:3002/orders

# Via gateway
curl http://localhost:3003/api/users
curl http://localhost:3003/api/products
curl http://localhost:3003/api/orders

# Health checks
curl http://localhost:3000/health
curl http://localhost:3001/health
curl http://localhost:3002/health
curl http://localhost:3003/health
```

### Expected Responses

**GET /users** → `[{"id":1,"name":"John Doe"},{"id":2,"name":"Jane Smith"}]`

**GET /products** → `[{"id":1,"name":"Laptop","price":999},{"id":2,"name":"Phone","price":699}]`

**GET /orders** → `[]` (empty array initially; use POST /api/orders to create orders)

**GET /api/users** → same as /users, proxied through gateway

**GET /api/products** → same as /products, proxied through gateway

**GET /api/orders** → same as /orders, proxied through gateway

## Useful Docker Commands

```bash
# Check running containers
docker compose ps

# View all logs
docker compose logs

# View logs for a specific service
docker compose logs user-service
docker compose logs product-service
docker compose logs order-service
docker compose logs gateway-service

# Stop all containers
docker compose down

# Rebuild and restart
docker compose up --build
```

## Troubleshooting

- **Port already in use**: Stop the existing process (`lsof -i :<port>` to find it) or change the host port in docker-compose.yml
- **Container exits immediately**: Check `docker compose logs <service-name>`
- **npm ci fails**: The packages use `npm install` fallback if no lock file is present
- **Gateway cannot reach services**: Ensure all containers are on the `microservices-network`; gateway uses Docker service names (`user-service`, `product-service`, `order-service`), not `localhost`
- **Rebuild after changes**: Run `docker compose up --build` to rebuild images

## Screenshots

### docker compose ps
> Run `docker compose ps` after `docker compose up -d` and capture the output showing all 4 containers in **running** state.

### Service Responses
> Capture `curl http://localhost:3000/users` showing the users JSON array.

> Capture `curl http://localhost:3001/products` showing the products JSON array.

> Capture `curl http://localhost:3002/orders` showing the orders array (empty `[]` is valid).

> Capture `curl http://localhost:3003/api/users` showing gateway proxying to user-service.

> Capture `curl http://localhost:3003/api/products` showing gateway proxying to product-service.

> Capture `curl http://localhost:3003/api/orders` showing gateway proxying to order-service.
