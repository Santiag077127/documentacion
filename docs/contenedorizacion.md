# 📦 Contenerización del Proyecto: PostgreSQL + Liquibase

---

## 📌 1. Introducción

Este documento describe la implementación de un entorno contenerizado utilizando Docker Compose, integrando PostgreSQL como motor de base de datos y Liquibase como herramienta de versionamiento de cambios.

El objetivo principal es garantizar un entorno reproducible, escalable y controlado para el desarrollo, pruebas y evolución del sistema de base de datos.

---

## 📌 2. Arquitectura del entorno

El sistema está compuesto por dos servicios principales:

### 🐘 PostgreSQL
- Motor principal de base de datos
- Almacena toda la información del sistema
- Persistencia mediante volúmenes Docker

### 🧩 Liquibase
- Herramienta de versionamiento de base de datos
- Ejecuta cambios DDL, DML y DCL
- Garantiza trazabilidad de migraciones

---

## 📌 3. Docker Compose del proyecto

```yaml
name: ${COMPOSE_PROJECT_NAME:-new-project-db}

services:
  postgres:
    image: postgres:16-alpine
    environment:
      POSTGRES_DB: ${POSTGRES_DB:-new_project}
      POSTGRES_USER: ${POSTGRES_USER:-new_project_user}
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD:-new_project_password}
    ports:
      - "${POSTGRES_PORT:-5433}:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data
    healthcheck:
      test:
        - CMD-SHELL
        - pg_isready -U ${POSTGRES_USER:-new_project_user} -d ${POSTGRES_DB:-new_project}
      interval: 5s
      timeout: 5s
      retries: 10
    restart: unless-stopped

  liquibase:
    build:
      context: .
      dockerfile: docker/liquibase/Dockerfile
    image: ${LIQUIBASE_IMAGE_TAG:-new-project-liquibase:5.0.2}
    profiles:
      - tooling
    depends_on:
      postgres:
        condition: service_healthy
    working_dir: /workspace
    volumes:
      - ./:/workspace
    entrypoint:
      - liquibase
      - --search-path=/workspace
      - --changeLogFile=changelog-master.yaml
      - --classpath=/liquibase/liquibase_libs/postgresql-42.7.8.jar
      - --url=jdbc:postgresql://postgres:5432/${POSTGRES_DB:-new_project}
      - --username=${POSTGRES_USER:-new_project_user}
      - --password=${POSTGRES_PASSWORD:-new_project_password}
      - --show-banner=false
      - --log-level=INFO

volumes:
  postgres_data:
```

---

## 📌 4. Imagen personalizada de Liquibase

Se utiliza una imagen extendida para incluir el driver de PostgreSQL necesario para la conexión con la base de datos.

```dockerfile
FROM liquibase/liquibase:5.0.2

RUN lpm add postgresql@42.7.8
```

---

## 📌 5. Variables de entorno

La configuración del sistema se gestiona mediante variables de entorno para facilitar la portabilidad y evitar valores hardcodeados.

```env
# Project
COMPOSE_PROJECT_NAME=new-project-db

# PostgreSQL
POSTGRES_DB=new_project
POSTGRES_USER=new_project_user
POSTGRES_PASSWORD=new_project_password
POSTGRES_PORT=5433

# Liquibase image tag
LIQUIBASE_IMAGE_TAG=new-project-liquibase:5.0.2
```

---

## 📌 6. Flujo de ejecución

**1. Levantar PostgreSQL**
```bash
docker compose up -d postgres
```

**2. Ejecutar migraciones con Liquibase**
```bash
docker compose --profile tooling run --rm liquibase update
```

---

## 📌 7. Características técnicas

- ✔ Orquestación con Docker Compose
- ✔ Separación de servicios (base de datos / migraciones)
- ✔ Healthcheck para asegurar disponibilidad
- ✔ Variables de entorno para configuración flexible
- ✔ Ejecución controlada de Liquibase con perfil `tooling`
- ✔ Persistencia de datos con volumen Docker
- ✔ Driver PostgreSQL incluido en imagen personalizada

---

## 📌 8. Beneficios de la implementación

- 🔄 Entorno reproducible en cualquier máquina
- 🚀 Automatización del versionamiento de base de datos
- 🧪 Facilidad para pruebas en desarrollo y QA
- 🔐 Configuración segura y flexible
- 📦 Portabilidad del entorno

---

## 📌 9. Conclusión

La integración de Docker Compose con PostgreSQL y Liquibase permite un entorno profesional de desarrollo de bases de datos, asegurando control de versiones, trazabilidad de cambios y facilidad de despliegue en cualquier entorno.