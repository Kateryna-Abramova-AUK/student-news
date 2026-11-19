FROM maven:3.10-eclipse-temurin-27 AS build
WORKDIR /app

COPY pom.xml .
RUN mvn -B -q dependency:go-offline
COPY src ./src
RUN mvn -B -q clean package -DskipTests

FROM eclipse-temurin:27-jre-alpine
WORKDIR /app

RUN addgroup -S -g 10001 app && adduser -S -u 10001 -G app app && \
    mkdir -p /data/avatars && chown -R app:app /data/avatars

COPY --from=build /app/target/avatar-service.jar app.jar

USER 10001:10001
EXPOSE 8090
ENTRYPOINT ["java", "-jar", "/app/app.jar"]