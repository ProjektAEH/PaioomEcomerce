# PaioomEcomerce: E-commerce Platform (Microservices with Spring Cloud)

## Overview

**PaioomEcomerce** is a demonstration project showcasing a distributed e-commerce platform built using the **microservice architecture** with **Spring Cloud**. It simulates a basic online store where users can manage their accounts, browse products, maintain shopping carts, and administrators can manage platform data.

## Services

The platform consists of the following microservices:

*   **`user-service`:** Manages user accounts and authentication.
*   **`inventory-service`:** Handles product catalog, inventory, and related business logic.
*   **`cart-service`:**  Manages user shopping carts.
*   **`api-gateway`:** Provides a unified entry point for clients, routing requests to the appropriate services.
*   **`config-server`:** Centralized configuration server for managing and distributing configurations to other services.
*   **`eureka-server`:** Service discovery server, enabling services to locate and communicate with each other.

Each microservice has its own `README.md` with detailed information about its functionality, API documentation, events, etc.

## Technology Stack

*   **Java 17**
*   **Spring Boot 3**
*   **Spring Cloud 2022.0.x (e.g., Hoxton)**
*   **Microservice Architecture**
*   **Spring Cloud Config Server:** Centralized configuration management.
*   **Spring Cloud Netflix Eureka:** Service discovery.
*   **Spring Cloud Gateway:** API gateway.
*   **Resilience4j:** Circuit breaker pattern implementation.
*   **Micrometer, OpenTelemetry, Zipkin:** Observability and distributed tracing.
*   **RabbitMQ:** Asynchronous inter-service communication.
*   **MySQL:** Relational database for `inventory-service`.
*   **MongoDB (version <= 4.4 due to AVX limitations):** Document database for `user-service` and `cart-service`.
*   **Redis:** In-memory data store for caching or session management (if applicable).
*   **Docker:** Containerization.
*   **Docker Compose:** Orchestration of services and dependencies.

## Prerequisites

*   **Java 17 JDK:** Ensure you have Java 17 JDK installed and configured.
*   **Docker:** Install the latest version of Docker Desktop (recommended) or Docker Engine.
*   **Docker Compose:**  Install the latest version of Docker Compose.
*   **Git:**  Install Git to manage the configuration repository.
*   **CPU without AVX instruction set support (Important Note):** Due to the use of an older MongoDB version (to avoid the AVX requirement), your system's CPU will not need to support AVX instructions.

## Getting Started

1.  **Clone the Repository:**

    ```bash
    git clone <your-repository-url>
    cd PaioomEcomerce
    ```

2.  **Configure the `config-repo`:**

    *   Navigate to the `config-repo` directory:
        ```bash
        cd config-repo
        ```
    *   Initialize a Git repository (if not already initialized):
        ```bash
        git init
        git add .
        git commit -m "Initial commit"
        ```
    *   Set your Git user name and email (if you haven't already configured them globally):
        ```bash
        git config user.email "[email address removed]"
        git config user.name "Your Name"
        ```
    *  Add configuration files for each service (e.g., `user-service.yml`, `inventory-service.yml`, `cart-service.yml`, `api-gateway.yml`) to the `config-repo` directory, make sure that the config files name match the spring application name of each service. If you want to have different configuration based on active profile, you can use `spring.config.activate.on-profile` property.
    *   **Important:** In your configuration files, make sure the following settings are correct:
        *   **Database URLs:** Use the Docker Compose service names as hostnames (e.g., `bms-mysql` for MySQL, `bms-mongodb` for MongoDB).
        *   **RabbitMQ URL:** Use `bms-rmq` as the hostname, port `5672`, and the username/password you've defined in `docker-compose.yml`.
        *   **Eureka Server URL:** `http://eureka-server:8761/eureka`
        *   **Config Server URL:** `http://config-server:8888`
    *   Commit the configuration files to the `config-repo`.

3.  **Build the Project (Optional):**

    *   If you need to build the microservices from source code, navigate to the root directory of the project (where `docker-compose.yml` is located) and run:
        ```bash
        docker-compose build
        ```

4.  **Start the Services:**

    ```bash
    docker-compose up -d
    ```

5.  **Monitor Service Startup:**

    *   Use `docker ps -a` to check the status of the containers.
    *   Use `docker logs <container_name>` to view the logs of individual services.
    *   Wait for all services to become `healthy` (this might take a few minutes).

## Accessing the Application

Once all services are healthy, you can access the different components of your e-commerce platform:

*   **Eureka Server:** [http://localhost:8761](http://localhost:8761)
*   **Config Server:** [http://localhost:8888](http://localhost:8888)
*   **API Gateway:** [http://localhost:8765](http://localhost:8765)
*   **User Service:** [http://localhost:8001](http://localhost:8001)
*   **Cart Service:** [http://localhost:8002](http://localhost:8002)
*   **Inventory Service:** [http://localhost:8004](http://localhost:8004)
*   **Zipkin (Distributed Tracing):** [http://localhost:9411](http://localhost:9411)
*   **RabbitMQ Management UI:** [http://localhost:15672](http://localhost:15672) (Default credentials: `guest`/`guest`)
*   **MySQL:** `localhost:3306` (Use a MySQL client to connect)
*   **MongoDB:** `localhost:27017` (Use a MongoDB client to connect)
*   **Redis:** `localhost:6379` (Use a Redis client to connect)

**Note:** You will interact with the microservices primarily through the API Gateway using the routes you have defined.

## Important Considerations

*   **MongoDB Version:** Due to CPU compatibility limitations (no AVX support), this project uses MongoDB version 3.6. You might need to adjust your database interactions accordingly.
*   **Unofficial MongoDB Image (If Applicable):** If you opted to use an unofficial MongoDB image to bypass the AVX requirement, be aware of the potential security and stability risks.
*   **Error Handling:** This `README.md` provides basic setup instructions. For a production-ready application, you would need to implement robust error handling, logging, and monitoring.

