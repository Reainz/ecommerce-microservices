# Setup Guide

This guide will help you set up and run the E-Commerce Web App on your local machine.

## Table of Contents
- [Prerequisites](#prerequisites)
- [Quick Start with Docker](#quick-start-with-docker)
- [Manual Setup](#manual-setup)
- [Environment Variables](#environment-variables)
- [Database Setup](#database-setup)
- [Running Services Individually](#running-services-individually)
- [Seeding Data](#seeding-data)
- [Troubleshooting](#troubleshooting)

## Prerequisites

Before you begin, ensure you have the following installed:

- **Node.js**: v14 or higher ([Download](https://nodejs.org/))
- **Docker**: Latest version ([Download](https://www.docker.com/get-started))
- **Docker Compose**: Usually included with Docker Desktop
- **Git**: For cloning the repository ([Download](https://git-scm.com/))

Verify your installations:
```bash
node --version
npm --version
docker --version
docker-compose --version
```

## Quick Start with Docker

The easiest way to run the entire application is using Docker Compose:

1. **Clone the repository** (if you haven't already):
```bash
git clone https://github.com/MyatThiriMaung3/e-commerce_web_app.git
cd e-commerce_web_app
```

2. **Start all services**:
```bash
docker-compose up --build
```

This command will:
- Build Docker images for all services
- Start 3 MongoDB instances (auth, product, order)
- Start all 4 microservices (auth, product, order, gateway)
- Set up networking between services

3. **Access the application**:
- **Main Application**: http://localhost:3000
- **Auth Service API**: http://localhost:3001/api/users
- **Product Service API**: http://localhost:3002
- **Order Service API**: http://localhost:3003

4. **Stop all services**:
```bash
docker-compose down
```

To stop and remove volumes (clears database data):
```bash
docker-compose down -v
```

## Manual Setup

If you prefer to run services without Docker:

### 1. Install Dependencies

Navigate to each service directory and install dependencies:

```bash
# Auth Service
cd auth-service
npm install

# Product Service
cd ../product-service
npm install

# Order Service
cd ../product-order-service
npm install

# Gateway Service
cd ../gateway-service
npm install
```

### 2. Set Up MongoDB

You need to run 3 MongoDB instances. You can either:

**Option A: Use Docker for MongoDB only**
```bash
docker run -d -p 27017:27017 --name auth-mongo mongo
docker run -d -p 27018:27017 --name product-mongo mongo
docker run -d -p 27019:27017 --name order-mongo mongo
```

**Option B: Install MongoDB locally and run 3 instances**
- Install MongoDB from [mongodb.com](https://www.mongodb.com/try/download/community)
- Configure 3 instances on different ports (27017, 27018, 27019)

### 3. Configure Environment Variables

Create `.env` files in each service directory (see [Environment Variables](#environment-variables) section below).

### 4. Start Services

Open 4 terminal windows and run each service:

**Terminal 1 - Auth Service:**
```bash
cd auth-service
npm run devStart
```

**Terminal 2 - Product Service:**
```bash
cd product-service
npm run devStart
```

**Terminal 3 - Order Service:**
```bash
cd product-order-service
npm run devStart
```

**Terminal 4 - Gateway Service:**
```bash
cd gateway-service
npm run devStart
```

## Environment Variables

### Auth Service (`auth-service/.env`)
```env
PORT=3001
MONGO_URI=mongodb://localhost:27017/auth_service
JWT_SECRET=your-secret-key-here
```

### Product Service (`product-service/.env`)
```env
PORT=3002
MONGO_URI=mongodb://localhost:27018/product_service
```

### Order Service (`product-order-service/.env`)
```env
PORT=3003
MONGO_URI=mongodb://localhost:27019/order_service
```

### Gateway Service (`gateway-service/.env`)
```env
PORT=3000
SESSION_KEY=your-session-secret-key-here
JWT_SECRET=your-jwt-secret-key-here
AUTH_SERVICE_URL=http://localhost:3001
PRODUCT_SERVICE_URL=http://localhost:3002
ORDER_SERVICE_URL=http://localhost:3003
```

**Note**: When using Docker Compose, these environment variables are automatically set. For local development, create `.env` files in each service directory.

## Database Setup

### MongoDB Connection Details

When using Docker Compose:
- **Auth MongoDB**: `mongodb://auth-mongo:27017/auth_service` (internal) / `localhost:27017` (external)
- **Product MongoDB**: `mongodb://product-mongo:27017/product_service` (internal) / `localhost:27018` (external)
- **Order MongoDB**: `mongodb://order-mongo:27017/order_service` (internal) / `localhost:27019` (external)

### Database Schema

The database schema is defined in `database_schema.json` at the root of the project. Each service manages its own database and collections.

## Running Services Individually

### Development Mode (with auto-reload)

Each service supports development mode with nodemon:

```bash
# In each service directory
npm run devStart
```

### Production Mode

```bash
# In each service directory
npm start
```

## Seeding Data

### Product Service Seed

To populate the product database with sample data:

**With Docker:**
```bash
docker-compose exec product-service npm run seed
```

**Without Docker:**
```bash
cd product-service
npm run seed
```

### Auth Service Seed

To seed user data:

**With Docker:**
```bash
docker-compose exec auth-service node seed.js
```

**Without Docker:**
```bash
cd auth-service
node seed.js
```

## Troubleshooting

### Port Already in Use

If you get a "port already in use" error:

**Windows:**
```bash
# Find process using the port
netstat -ano | findstr :3000

# Kill the process (replace PID with actual process ID)
taskkill /PID <PID> /F
```

**Linux/Mac:**
```bash
# Find process using the port
lsof -i :3000

# Kill the process
kill -9 <PID>
```

### Docker Issues

**Reset Docker containers:**
```bash
docker-compose down -v
docker-compose up --build
```

**View logs:**
```bash
# All services
docker-compose logs

# Specific service
docker-compose logs auth-service
docker-compose logs gateway-service
```

**Rebuild specific service:**
```bash
docker-compose build auth-service
docker-compose up auth-service
```

### MongoDB Connection Issues

**Check if MongoDB is running:**
```bash
# With Docker
docker ps | grep mongo

# Check MongoDB logs
docker logs auth-mongo
```

**Test MongoDB connection:**
```bash
# Connect to MongoDB shell
docker exec -it auth-mongo mongosh
```

### Service Communication Issues

Ensure all services are running and accessible:
- Auth Service: http://localhost:3001
- Product Service: http://localhost:3002
- Order Service: http://localhost:3003
- Gateway Service: http://localhost:3000

Check service health by accessing their endpoints or viewing logs.

### Environment Variables Not Loading

- Ensure `.env` files are in the correct service directories
- Verify variable names match exactly (case-sensitive)
- Restart the service after changing `.env` files

### Dependencies Issues

If you encounter dependency errors:

```bash
# Remove node_modules and package-lock.json
rm -rf node_modules package-lock.json

# Reinstall
npm install
```

## Additional Resources

- [Node.js Documentation](https://nodejs.org/docs/)
- [Express.js Documentation](https://expressjs.com/)
- [MongoDB Documentation](https://docs.mongodb.com/)
- [Docker Documentation](https://docs.docker.com/)

## Support

If you encounter issues not covered in this guide, please:
1. Check the service logs
2. Verify all prerequisites are installed
3. Ensure all environment variables are set correctly
4. Open an issue on the GitHub repository

