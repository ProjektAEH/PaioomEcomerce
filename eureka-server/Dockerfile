FROM openjdk:18-jdk-alpine
RUN apk --no-cache add curl

# Declare ARG instructions to receive build-time arguments
ARG BMS_PROFILE=production
ARG CONFIG_REPO_PATH=/home/adel/Desktop/PaioomEcomerce/config-repo
ARG EUREKA_SERVER_URL=http://eureka-server:8761/eureka
ARG CONFIG_SERVER_URL=http://config-server:8888

# Set environment variables using the ARG values (or their defaults)
ENV BMS_PROFILE=${BMS_PROFILE}
ENV CONFIG_REPO_PATH=${CONFIG_REPO_PATH}
ENV EUREKA_SERVER_URL=${EUREKA_SERVER_URL}
ENV CONFIG_SERVER_URL=${CONFIG_SERVER_URL}
HEALTHCHECK --interval=10s --timeout=5s --retries=3 \
 CMD curl -f http://localhost:8761/actuator/health || exit 1

WORKDIR /app
COPY mvnw .
COPY .mvn .mvn
COPY pom.xml .
RUN ./mvnw dependency:go-offline -B
COPY src src
RUN ./mvnw package -DskipTests
EXPOSE 8761

ENTRYPOINT ["java","-jar","target/eureka-server-0.0.1-SNAPSHOT.jar"]