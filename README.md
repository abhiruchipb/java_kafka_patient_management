Patient Management System
This project is a microservices-based application designed to manage patient information and their corresponding billing accounts. It demonstrates a decoupled architecture where services communicate efficiently using gRPC.

Architecture
The system is composed of two main microservices:

patient-service: A traditional Spring Boot application that will expose a REST API for managing patient data (CRUD operations). It acts as the client in the gRPC communication.

billingservice: A Spring Boot application that exposes a gRPC API to handle the creation and management of patient billing accounts. It acts as the server in the gRPC communication.

The communication between the two services is defined by a Protocol Buffers (.proto) contract, ensuring type safety and high performance.

Modules
1. billingservice
Responsibility: Manages all billing-related logic.

API: gRPC

Endpoint: CreateBillingAccount

Technology: Java, Spring Boot, gRPC, Maven.

Containerized: Yes, using the provided Dockerfile.

2. patient-service
Responsibility: Manages patient demographic data (e.g., name, email).

API: REST (for external clients) and gRPC (for internal communication).

Technology: Java, Spring Boot, Spring Data JPA, PostgreSQL, Maven.

Technology Stack
Language: Java 21

Framework: Spring Boot 3

Build Tool: Maven

Inter-service Communication: gRPC with Protocol Buffers

Database: PostgreSQL

Containerization: Docker

Getting Started
Prerequisites
Java Development Kit (JDK) 21 or later.

Apache Maven.

Docker and Docker Desktop.

Running the Services
1. Run the billingservice
Option A: Using Maven

# Navigate to the billingservice directory
cd billingservice

# Compile and run the application
mvn spring-boot:run

The gRPC server will start on port 9001.

Option B: Using Docker

# Navigate to the billingservice directory
cd billingservice

# Build the Docker image
docker build -t billingservice-app .

# Run the container
docker run -p 4001:4001 -p 9001:9001 billingservice-app

2. Run the patient-service
# Navigate to the patient-service directory
cd patient-service

# Compile and run the application
mvn spring-boot:run

The REST API server will start on its configured port (e.g., 8080).

Usage & API Testing
Testing the gRPC Endpoint
You can test the billingservice using a gRPC client like Postman.

Server URL: localhost:9001

Import Proto File: Load the billingservice/src/main/proto/billing_service.proto file.

Method: Select BillingService/CreateBillingAccount.

Request Body:

{
    "patientId": "12345",
    "name": "John Doe",
    "email": "john.doe@example.com"
}

Invoke the request to receive the response.
