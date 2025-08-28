# --- Stage 1: The "Builder" ---
# Use an official Maven image that includes Java 21 (Temurin) as the base.
# This stage has all the tools needed to build the application.
FROM maven:3.9.9-eclipse-temurin-21 AS builder

# Set the working directory inside the container.
WORKDIR /app

# Copy only the pom.xml first. This is a key optimization.
# Docker caches layers, so if the pom.xml doesn't change, it won't re-download dependencies.
COPY pom.xml .

# Download all project dependencies into the local Maven repository.
# The "-B" flag runs it in non-interactive (batch) mode.
RUN mvn dependency:go-offline -B

# Copy the rest of the application's source code into the container.
COPY src ./src

# Compile the code, run tests, and package it into a .jar file.
# "clean" removes any previous builds, and "package" creates the final artifact.
RUN mvn clean package


# --- Stage 2: The "Runner" ---
# Use a slim, official OpenJDK image as the base for the final image.
# This image only contains what's necessary to run a Java application.
FROM openjdk:21-jdk AS runner

# Set the working directory inside the final container.
WORKDIR /app

# Copy the compiled .jar file from the "builder" stage into this new stage.
# The path is the standard location where Maven places the build artifact.
COPY --from=builder /app/target/billingservice-0.0.1-SNAPSHOT.jar ./app.jar

# Expose the HTTP port that Spring Boot's web server will use.
EXPOSE 4001

# Expose the gRPC port that the gRPC server will use.
EXPOSE 9001

# The command that will be executed when the container starts.
# This runs the Java application from the packaged .jar file.
ENTRYPOINT ["java", "-jar", "app.jar"]