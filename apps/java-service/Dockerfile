# ---------- build stage ----------
FROM maven:3.9-eclipse-temurin-21 AS build
WORKDIR /app

# We use repo-root build context; copy from apps/java-service/...
COPY apps/java-service/pom.xml ./pom.xml
RUN mvn -q -B -DskipTests dependency:go-offline

COPY apps/java-service/src ./src
# Build an executable Spring Boot jar (spring-boot-maven-plugin -> repackage)
RUN mvn -q -B -DskipTests package

# ---------- run stage ----------
FROM eclipse-temurin:21-jre
WORKDIR /app
# Copy the boot jar (single artifact in target)
COPY --from=build /app/target/*.jar /app/app.jar
EXPOSE 8080
ENTRYPOINT ["java","-jar","/app/app.jar"]
