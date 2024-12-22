# Estágio 1: Build
FROM maven:3.8.5-openjdk-17-slim AS maven_build
WORKDIR /app
COPY ../../backend/pom.xml /app/pom.xml
RUN mvn dependency:go-offline
COPY ../../backend/src  /app/src
RUN mvn package -DskipTests

# Estágio 2: Runtime
FROM openjdk:17-jdk-slim
WORKDIR /app

# Copiar apenas o artefato gerado
COPY --from=maven_build /app/target/*.jar /app/app.jar

# Definir permissões e usuário
RUN chown 1001 /app
USER 1001

# Definir variáveis de ambiente
ENV APP_JAR=app.jar
ENV JAVA_OPTS=""

# Expor a porta e definir o entrypoint
EXPOSE 8082
ENTRYPOINT ["sh", "-c", "java $JAVA_OPTS -jar /app/$APP_JAR"]
