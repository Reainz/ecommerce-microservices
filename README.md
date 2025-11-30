# E-Commerce Web App

**This project is created for the final project of Web Programming with Node.js course at Ton Duc Thang University.**

An e-commerce web application for computers and computer related products only, which provides fully user friendly features (The features are implemented according to the course's final project requirements).

## Table of Contents
- [Features](#features)
- [Technologies Used](#technologies-used)
- [Project Architecture](#project-architecture)
- [Services](#services)
- [Getting Started](#getting-started)
- [Design](#design)
- [License](#license)

## Features  
- User authentication (Sign up, Login, Logout)
- Product browsing and search functionality
- Add to cart and checkout process 
- Product rating system and commenting system
- Admin dashboard for managing the whole web app
- Analysis on various categories to admin access

## Technologies Used
- **Language**: JavaScript, HTML, CSS
- **Frontend**: JavaScript, HTML, CSS, Bootstrap, Tailwind, EJS
- **Backend**: Node.js with Express.js
- **Database**: MongoDB
- **Architecture**: Microservice Architecture, MVC
- **Tools**: Git + GitHub, VSCode, Docker, Docker Compose

## Project Architecture
The architecture consists of 4 independent web services, each following MVC structure and its own database. Each service uses MongoDB as its database. The gateway service acts as a reverse proxy and frontend rendering layer, connecting to all other services.

### Architecture Diagram
```
┌─────────────┐
│   Gateway   │ (Port 3000)
│   Service   │
└──────┬──────┘
       │
   ┌───┴───┬──────────┬──────────────┐
   │       │          │              │
┌──▼──┐ ┌──▼──┐  ┌───▼───┐    ┌─────▼─────┐
│Auth │ │Prod │  │ Order │    │  MongoDB  │
│(3k1)│ │(3k2)│  │ (3k3) │    │ Instances │
└─────┘ └─────┘  └───────┘    └───────────┘
```

## Services
- **Authentication Service** (Port 3001): User login, registration, JWT handling
- **Product Service** (Port 3002): Product CRUD, category, variants, reviews
- **Order Service** (Port 3003): Cart, checkout, order history, loyalty points
- **Gateway Service** (Port 3000): Frontend page rendering and connecting to other services

## Getting Started

### Prerequisites
- Node.js (v14 or higher)
- Docker and Docker Compose
- Git

### Quick Start
1. Clone the repository:
```bash
git clone https://github.com/MyatThiriMaung3/e-commerce_web_app.git
cd e-commerce_web_app
```

2. Start all services using Docker Compose:
```bash
docker-compose up --build
```

3. Access the application:
- Gateway Service: http://localhost:3000
- Auth Service API: http://localhost:3001
- Product Service API: http://localhost:3002
- Order Service API: http://localhost:3003

For detailed setup instructions, environment variables, and development setup, please see [SETUP.md](SETUP.md).

## Design
The base design is done by [MD Rimel](https://www.figma.com/@mdrimel15). We updated the design of the designer credited, to match the requirements of the project provided by our instructor. The visual presentation of the credited designer still remains.

### View design in Figma
[Full E Commerce Website UI UX Design](https://www.figma.com/community/file/1219312065205187851) - used under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)

## License
This project is licensed under the MIT license. See [LICENSE](LICENSE) for more information.
