# --------------------
# STAGE 1: BUILD THE APPLICATION
# --------------------
# Use a Java 17 JDK/Maven image for building. 
FROM maven:3.9.6-eclipse-temurin-17 AS build

# Set the working directory inside the container
# We are creating the 'demo' folder as you intended
WORKDIR /app/demo

# Copy the pom.xml (This often needs to be done first to leverage caching)
COPY pom.xml .

# Copy the source code (assuming 'src' is in the root of your local build context)
COPY src ./src 

# The 'target' folder will be created inside /app/demo/ by the 'mvn package' command
# Build the application
RUN mvn clean package -DskipTests

# --------------------
# STAGE 2: CREATE THE FINAL RUNNING IMAGE
# --------------------
# Use a JRE (Java Runtime Environment) base image for a smaller final image.
FROM openjdk:17-jre-slim

# Set the working directory
WORKDIR /app

# Copy the final JAR file from the 'build' stage's target directory
# Adjust 'spring-boot-app.jar' to match the name in your pom.xml
ARG JAR_FILE=target/*.jar
COPY --from=build /app/demo/${JAR_FILE} app.jar

# Expose the port
EXPOSE 8080

# Command to run the application
ENTRYPOINT ["java", "-jar", "app.jar"]
