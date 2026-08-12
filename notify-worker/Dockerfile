# Estágio 1: Build da aplicação (Multi-stage build economiza espaço)
FROM eclipse-temurin:21-jdk-alpine as builder
WORKDIR /app

# Copia os arquivos do Maven (estamos assumindo que você gerou com Maven)
COPY mvnw .
COPY .mvn .mvn
COPY pom.xml .
COPY src src

# Dá permissão de execução ao wrapper do Maven e gera o build
RUN chmod +x ./mvnw
RUN ./mvnw clean package -DskipTests

# Estágio 2: Imagem final apenas com o JRE
FROM eclipse-temurin:21-jre-alpine
WORKDIR /app

# Copia o .jar gerado do estágio anterior
COPY --from=builder /app/target/*.jar app.jar

# Expõe a porta (8080 para a API. Para o Worker é opcional, mas útil para o Actuator/K8s)
EXPOSE 8080

ENTRYPOINT ["java", "-jar", "app.jar"]