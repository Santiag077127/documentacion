ADR-001: Consolidación del dominio de operaciones de vuelo

Título:

Estandarización del dominio de operaciones de vuelo

Contexto:

El modelo de base de datos incluye entidades relacionadas con la gestión de vuelos, tales como:

flight

flight_segment

flight_status

aircraft

airport


Estas entidades forman parte fundamental del sistema, ya que permiten representar la operación aérea y la planificación de vuelos.

Problema:

Las entidades relacionadas con vuelos no están formalmente organizadas como un dominio funcional dentro del proyecto, lo que puede dificultar:

la comprensión del modelo
la mantenibilidad del sistema
la evolución futura de las funcionalidades

Decisión:

Se define el dominio funcional Operaciones de Vuelo, agrupando las siguientes entidades:

flight

flight_segment

flight_status

aircraft

airport

Además, se establecen lineamientos:

Mantener coherencia en las relaciones entre entidades
Organizar los changelogs de Liquibase por dominio
Garantizar integridad de los datos mediante restricciones definidas en el modelo

Justificación técnica:

Permite estructurar el modelo por dominios funcionales
Facilita el mantenimiento y la escalabilidad del sistema
Mejora la organización del versionamiento con Liquibase
Asegura consistencia en la gestión de datos relacionados con vuelos

Consecuencias:

Mejor organización del modelo de datos
Mayor claridad para futuros desarrollos
Base sólida para ampliar funcionalidades (ej: rutas, escalas, horarios avanzados)
Necesidad de mantener disciplina en la organización por dominio