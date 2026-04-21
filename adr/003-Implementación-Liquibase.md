ADR-003: Implementación de Liquibase para versionamiento del DDL

Título:

Implementación de Liquibase para la gestión y versionamiento de la base de datos

Contexto:

El modelo de base de datos fue entregado como un único script (modelo_postgresql.sql), lo cual dificulta:

el control de cambios.

la trazabilidad de modificaciones.

el trabajo colaborativo.

Para solucionar esto, se integró Liquibase dentro del proyecto, ejecutándose mediante contenedor Docker, permitiendo gestionar los cambios de la base de datos de forma estructurada.

Actualmente:

El DDL está separado en changelogs organizados por dominio funcional
Existen changelogs para:

tablas

índices

inserts (DML)

roles y permisos (DCL)

Problema:

Trabajar con un único script genera:

pérdida de control sobre versiones.

dificultad para aplicar cambios en diferentes entornos.

riesgo de inconsistencias entre desarrolladores.

falta de trazabilidad técnica.

Decisión:

Se implementa Liquibase como herramienta de versionamiento de la base de datos con las siguientes reglas:

Uso de un archivo principal:

changelog-master.xml

Organización por capas:

01_ddl/ → estructura (tablas, índices)

02_dml/ → datos de prueba

03_dcl/ → roles y permisos

Separación por dominio funcional:

cada dominio tiene su propio changelog
no se agrupan todas las tablas en un solo archivo
Uso de sqlFile para ejecutar scripts externos

Ejecución mediante Docker:

integración con PostgreSQL en contenedor

ejecución controlada con comandos como:

docker compose --profile tooling run --rm liquibase update

Justificación técnica:

Permite versionar la base de datos igual que el código
Facilita el trabajo en equipo
Garantiza trazabilidad de cambios (tabla databasechangelog)
Permite rollback de cambios
Mejora la organización del proyecto
Se integra perfectamente con la estrategia de ramas (develop, qa, main)

Consecuencias:

✅ Positivas:

Control total sobre cambios en la base de datos
Despliegue reproducible en cualquier entorno
Historial claro de modificaciones
Mejor mantenimiento del sistema

⚠️ Consideraciones:

Se requiere disciplina al crear nuevos changesets
No modificar changesets ya ejecutados (crear nuevos en su lugar)
Mantener orden lógico según dependencias entre tablas
Validar scripts antes de ejecutarlos en producción