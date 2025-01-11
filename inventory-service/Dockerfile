FROM openjdk:17-jdk-alpine
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


# Set the working directory
WORKDIR /app

# Copy Maven wrapper, pom.xml, and source code
COPY mvnw .
COPY .mvn .mvn
COPY pom.xml .
COPY src src

# Use Maven to resolve dependencies, build, and package the project
RUN ./mvnw clean install -DskipTests
EXPOSE 8004

ENTRYPOINT ["java","-jar","target/inventory-service-0.0.1-SNAPSHOT.jar"]