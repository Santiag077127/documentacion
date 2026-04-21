ADR-004: Estrategia de versionamiento del repositorio con ramas develop, qa y main

Título:

Definición de estrategia de ramas para control de versiones del proyecto

Contexto:

El proyecto de base de datos se está gestionando en un repositorio Git con múltiples ramas activas, lo que evidencia la necesidad de una estrategia clara de versionamiento para evitar conflictos, mantener estabilidad y organizar el flujo de trabajo entre desarrollo, pruebas y producción.

Actualmente se observan ramas como:

main

develop

qa


Esto demuestra un crecimiento del proyecto que requiere una estructura formal.

Problema:

Sin una estrategia definida de ramas:

Se pueden mezclar cambios inestables con versiones productivas
Se dificulta la integración de cambios entre equipos o fases
No hay un flujo claro de aprobación y pruebas
Se pierde control sobre qué versión está en cada estado del sistema

Decisión:

Se adopta una estrategia de versionamiento basada en tres ramas principales:

🔹 main
Rama estable y productiva
Solo contiene versiones validadas
No se realizan cambios directos

🔹 qa
Rama de pruebas (Quality Assurance)
Aquí se validan cambios antes de producción
Recibe integraciones desde develop

🔹 develop
Rama de desarrollo activo
Aquí se integran nuevas funcionalidades
Punto de unión de ramas de HU o dominios

🔹 Flujo de trabajo definido:
Se desarrolla en ramas de trabajo (HU o dominios)
Se integran cambios en develop
Se valida en qa
Se despliega en main cuando está estable

Justificación técnica:

Permite separar claramente desarrollo, pruebas y producción
Reduce riesgos de romper la base de datos en entornos críticos
Facilita el trabajo colaborativo entre múltiples cambios simultáneos
Se alinea con prácticas profesionales de Git Flow
Mejora la trazabilidad de cambios por funcionalidad (HU o dominio)

Consecuencias:

✅ Positivas:

Mayor control del ciclo de vida del software
Mejor organización del equipo de desarrollo
Despliegues más seguros y controlados
Integración ordenada con Liquibase y PostgreSQL

⚠️ Consideraciones:

Requiere disciplina para no trabajar directamente en main
Es necesario mantener sincronización constante entre ramas
Puede aumentar la complejidad inicial del flujo de trabaj