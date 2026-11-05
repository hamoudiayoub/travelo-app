FROM eclipse-temurin:21-jdk-alpine-3.22 AS builder

RUN apk update && apk upgrade --no-cache

WORKDIR /app

COPY pom.xml .
COPY mvnw .
COPY .mvn .mvn

RUN chmod +x mvnw

RUN ./mvnw dependency:go-offline -B

COPY src ./src

RUN ./mvnw clean package -DskipTests -B


FROM eclipse-temurin:21-jre-alpine-3.22

RUN apk update && apk upgrade --no-cache

RUN addgroup -S travelo && adduser -S travelo -G travelo

WORKDIR /app

COPY --from=builder /app/target/*.jar app.jar

RUN chown travelo:travelo app.jar

USER travelo

EXPOSE 8080

ENTRYPOINT ["java", "-jar", "app.jar"]