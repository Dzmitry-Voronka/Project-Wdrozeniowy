# Stage 1: build
FROM maven:3.9-eclipse-temurin-17 AS build
WORKDIR /workspace

COPY pom.xml .
COPY common/pom.xml common/pom.xml
COPY forum/pom.xml forum/pom.xml
COPY analytics/pom.xml analytics/pom.xml
COPY app/pom.xml app/pom.xml

# Download dependencies first (layer cache)
RUN mvn dependency:go-offline -B

COPY common/src common/src
COPY forum/src forum/src
COPY analytics/src analytics/src
COPY app/src app/src

RUN mvn clean package -pl app -am -DskipTests -B

# Stage 2: runtime
FROM eclipse-temurin:17-jre-alpine
WORKDIR /app
COPY --from=build /workspace/app/target/*.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
