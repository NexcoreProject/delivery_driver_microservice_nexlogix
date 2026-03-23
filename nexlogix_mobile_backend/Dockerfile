# Etapa 1: Construcción
FROM eclipse-temurin:21-jdk-alpine AS builder

WORKDIR /build

# Copiamos archivos de configuración primero para aprovechar caché
COPY mvnw .
COPY .mvn .mvn/
COPY pom.xml .

# Damos permisos
RUN chmod +x mvnw

# Descargamos dependencias con caché persistente
RUN --mount=type=cache,target=/root/.m2 \
    ./mvnw dependency:go-offline -B

# Copiamos el código fuente (después de las dependencias para que no invalide la capa anterior)
COPY src ./src

# Compilamos (también usando caché)
RUN --mount=type=cache,target=/root/.m2 \
    ./mvnw clean package -DskipTests && \
    mv target/*.jar app.jar