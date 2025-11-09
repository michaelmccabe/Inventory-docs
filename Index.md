# Inventory Project Suite

Welcome! This site provides an overview of both the **Frontend** and **Backend** projects for the Inventory system, and guides you on how to run both applications locally for development or review.

---

## Projects Overview

### 1. Inventory Frontend

- **Repository:** [inventory-frontend](https://github.com/michaelmccabe/Inventory-frontend)
- **Description:**  
  A comprehensive inventory management web application built with Next.js 14, TypeScript, and Tailwind CSS. The application provides a modern interface for managing inventory items and processing orders.
  
- **Key Features:**
  - Responsive user interface with Tailwind CSS
  - Real-time inventory management
  - Order assembly and tracking with status badges
  - CORS-free API integration via Next.js API routes

### 2. Inventory Backend

- **Repository:** [inventory-backend](https://github.com/michaelmccabe/Inventory-backend)
- **Description:**  
  A RESTful API server built with Spring Boot for managing inventory data, orders, and providing data to the frontend application. Includes OpenTelemetry tracing with Jaeger.
  
- **Key Features:**
  - Secure REST APIs with OpenAPI specification
  - PostgreSQL database with Docker support
  - Distributed tracing with Jaeger and OpenTelemetry
  - Comprehensive order lifecycle management

---

## Running the Applications Locally

### Prerequisites

Before you begin, ensure you have the following installed:

- **Node.js 18+** (for the frontend)
- **Java 21** (for the backend)
- **Maven** (for building the backend)
- **Docker & Docker Compose** (for backend dependencies)
- **Git** (for cloning repositories)

---

## Step 1: Start the Backend

The backend requires Docker to run PostgreSQL, Jaeger, and OpenTelemetry Collector.

### 1.1 Clone the Backend Repository

```bash
git clone https://github.com/michaelmccabe/Inventory-backend.git
cd Inventory-backend
```

### 1.2 Start Docker Services

Navigate to the `local-dev` directory and start the required services:

```bash
cd local-dev
docker-compose up -d
```

This starts:
- PostgreSQL database
- Jaeger UI (for distributed tracing)
- OpenTelemetry Collector

To view the Jaeger Service UI, open: [http://localhost:16686/search](http://localhost:16686/search)

### 1.3 Build the Backend

Return to the project root and build the application:

```bash
cd ..
mvn clean install
```

### 1.4 Run the Backend Service

Start the Spring Boot application:

```bash
mvn spring-boot:run
```

✅ **The backend will run on [http://localhost:8080](http://localhost:8080)**

**Optional:** To test the API, you can create a test item:

```bash
curl -X POST http://localhost:8080/api/items \
  -H "Content-Type: application/json" \
  -H "X-Correlation-ID: test-123" \
  -d '{
    "name": "Test Widget",
    "sku": "TEST-001",
    "price": 9.99,
    "stock_quantity": 50
  }'
```

### 1.5 Stop the Backend (when done)

To stop the Docker services:

```bash
cd local-dev
docker-compose down -v
```

---

## Step 2: Start the Frontend

The frontend is a Next.js application that connects to the backend API.

### 2.1 Clone the Frontend Repository

Open a **new terminal window** and run:

```bash
git clone https://github.com/michaelmccabe/Inventory-frontend.git
cd Inventory-frontend
```

### 2.2 Install Dependencies

```bash
npm install
```

### 2.3 Configure Environment

Create the environment configuration file:

```bash
cp .env.local.example .env.local
```

Edit `.env.local` and set the backend API URL:

```env
NEXT_PUBLIC_API_BASE_URL=http://localhost:8080
```

### 2.4 Start the Frontend Development Server

```bash
npm run dev
```

✅ **The frontend will launch at [http://localhost:3000](http://localhost:3000)**

---

## Step 3: Use the Application

1. Open your browser and navigate to **[http://localhost:3000](http://localhost:3000)**
2. Navigate to **Items** to manage inventory items (create, edit, delete)
3. Navigate to **Orders** to:
   - Assemble new orders from available inventory
   - View, edit, and purchase existing orders
   - Track order status (SAVED, PURCHASED, HELD)

All frontend interactions communicate with your locally running backend at `http://localhost:8080`.

---

## Monitoring & Debugging

### Jaeger Distributed Tracing

To view traces from the Inventory Service:

1. Open the Jaeger UI at [http://localhost:16686/search](http://localhost:16686/search)
2. Select `inventory-service` from the **Services** dropdown
3. Click **Find Traces** to see all API calls and their performance metrics

This helps you understand request flow, latency, and troubleshoot issues.

---

## Quick Reference

| Component | URL | Description |
|-----------|-----|-------------|
| Frontend UI | http://localhost:3000 | Next.js web application |
| Backend API | http://localhost:8080 | Spring Boot REST API |
| Jaeger UI | http://localhost:16686 | Distributed tracing dashboard |
| API Docs | [OpenAPI Spec](https://raw.githubusercontent.com/michaelmccabe/Inventory/refs/heads/main/src/main/resources/openapi.yaml) | REST API specification |

---

## Additional Resources

- [Frontend Development Guide](https://github.com/michaelmccabe/Inventory-frontend/blob/main/DEVELOPMENT_GUIDE.md) - Comprehensive frontend documentation
- [Backend Local Dev Setup](https://github.com/michaelmccabe/Inventory-backend/tree/main/local-dev) - Docker configuration details
- [Frontend Repository](https://github.com/michaelmccabe/Inventory-frontend)
- [Backend Repository](https://github.com/michaelmccabe/Inventory-backend)

---

## Troubleshooting

**Backend won't start?**
- Ensure Docker is running and ports 5432, 16686, and 4317 are available
- Check that Java 21 is installed: `java -version`

**Frontend can't connect to backend?**
- Verify backend is running at http://localhost:8080
- Check `.env.local` has the correct `NEXT_PUBLIC_API_BASE_URL`

**Docker issues?**
- Stop and remove containers: `docker-compose down -v`
- Restart: `docker-compose up -d`

---

**Need help?**  
Create an issue in either repository or contact the project maintainers!

**Last Updated:** November 2025
