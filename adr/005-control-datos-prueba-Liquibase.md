ADR-005: Estrategia de versionamiento y control de datos de prueba en Liquibase

Título:

Estrategia de control y carga de datos de prueba organizada por dependencias usando Liquibase

Contexto:

El proyecto de base de datos ya está estructurado con múltiples dominios funcionales y gestionado mediante Liquibase. Actualmente existen scripts de inserción (02_dml/00_inserts) organizados por entidades como geografía, aerolínea, identidad, seguridad, aeropuerto, aeronaves y vuelos.

Durante la ejecución de migraciones se evidencia que el orden de carga de datos es crítico debido a las relaciones entre tablas y restricciones de integridad referencial.

Problema:

Sin una estrategia clara para la carga de datos de prueba:

Se generan errores por violación de llaves foráneas o restricciones CHECK
El orden de inserción no está garantizado entre dominios
Es difícil reproducir entornos de prueba consistentes
Los cambios en datos pueden romper migraciones en Liquibase
No hay trazabilidad clara de qué datos dependen de otros

Decisión:
Se define una estrategia de control de datos de prueba basada en Liquibase con las siguientes reglas:

🔹 1. Orden de carga obligatorio por dependencias

Los inserts deben ejecutarse siguiendo esta lógica:

Dominios base (sin dependencias):

geografía

identidad

seguridad

Dominios intermedios:

aerolínea

aeropuerto

aeronaves

clientes

Dominios operacionales:

vuelos

reservas

ventas

pagos

abordaje

🔹 2. Separación por archivo y dominio

Cada conjunto de datos debe estar en un archivo independiente.

🔹 3. Integración controlada con Liquibase

Cada archivo se ejecuta como un changeset independiente
Se garantiza trazabilidad en databasechangelog
No se mezclan inserts de distintos dominios en un solo changeset

🔹 4. Reglas de consistencia de datos

Los UUID deben ser consistentes o referenciados correctamente
No se deben insertar datos hijos antes de sus padres
Se deben respetar constraints y CHECK definidos en el modelo

🔹 5. Reproducibilidad del entorno

El objetivo es que cualquier entorno (dev, qa o main) pueda:

Levantar la base desde cero

Ejecutar Liquibase

Obtener los mismos datos de prueba

Sin errores de integridad

Justificación técnica:

Asegura estabilidad en migraciones con Liquibase
Evita errores de integridad referencial en PostgreSQL
Mejora la reproducibilidad del entorno de desarrollo y pruebas
Facilita la depuración de datos en entornos QA
Permite escalar el modelo sin romper dependencias

Consecuencias:

✅ Positivas:

Migraciones más seguras y ordenadas
Menos errores en ejecución de inserts
Mejor control del ciclo de vida de datos
Entornos replicables y confiables

⚠️ Consideraciones:

Requiere disciplina en el orden de creación de inserts
Cambios en un dominio pueden afectar dependencias
Es necesario mantener documentación actualizada de relaciones