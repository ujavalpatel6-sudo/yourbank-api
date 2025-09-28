# -------- Stage 1: Build the JAR --------
FROM maven:3.9.6-eclipse-temurin-17 AS build

# Set working directory inside the container
WORKDIR /app

# Copy pom.xml and download dependencies first (for caching)
COPY ./demo/pom.xml .
RUN mvn dependency:go-offline -B

# Copy the rest of the source code
COPY src ./demo/src

# Build the application
RUN mvn clean package -DskipTests

# -------- Stage 2: Run the JAR --------
FROM openjdk:17-jdk-slim

# Set working directory inside the container
WORKDIR /app

# Copy the built JAR from the previous stage
COPY --from=build /app/target/*.jar app.jar

# Expose port 8080 for the app
EXPOSE 8080

# Run the Spring Boot app
ENTRYPOINT ["java", "-jar", "app.jar"]
