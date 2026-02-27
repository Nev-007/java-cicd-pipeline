# Multi-stage build for Spring Boot application
# Stage 1: Build the application
FROM maven:3.8.6-openjdk-11 AS builder

WORKDIR /app

# Copy pom.xml first for dependency caching
COPY pom.xml .
RUN mvn dependency:go-offline -B

# Copy source and build
COPY src ./src
RUN mvn clean package

# Stage 2: Runtime image
FROM adoptopenjdk/openjdk11:alpine-jre

WORKDIR /opt/app

# Create a non-root user for security
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

COPY --from=builder /app/target/spring-boot-web.jar app.jar

RUN chown appuser:appgroup app.jar

USER appuser

EXPOSE 8080

ENTRYPOINT ["java", "-jar", "app.jar"]
