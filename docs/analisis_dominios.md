# 📊 Análisis de Dominios del Modelo de Datos

## 1. Introducción

El presente documento tiene como objetivo analizar el modelo de datos proporcionado en el archivo `modelo_postgresql.sql`, identificando los dominios funcionales que componen el sistema. Este análisis permite comprender la organización lógica del modelo, sus responsabilidades y las relaciones entre sus componentes, sirviendo como base para su estabilización, versionamiento y evolución técnica.

---

## 2. Identificación de Dominios Funcionales

A partir del análisis del modelo, se identifican doce (12) dominios funcionales. Aunque el modelo base permite una agrupación más general, se propone una descomposición más granular con el fin de mejorar la mantenibilidad, escalabilidad y claridad del sistema.

Los dominios identificados son:

### 2.1 Geografía

**Responsabilidad:**

* Gestión de países, ciudades y regiones.

**Relaciones:**

* Es referenciado por aeropuertos, clientes y operaciones de vuelo.

---

### 2.2 Aerolínea

**Responsabilidad:**

* Gestión de la información institucional de la aerolínea.

**Relaciones:**

* Se relaciona con aeronaves y operaciones de vuelo.

---

### 2.3 Identidad

**Responsabilidad:**

* Gestión de tipos de documento e identificación de entidades.

**Relaciones:**

* Base para clientes y seguridad.

---

### 2.4 Seguridad

**Responsabilidad:**

* Gestión de usuarios, roles y permisos.

**Relaciones:**

* Depende del dominio de identidad.

---

### 2.5 Clientes

**Responsabilidad:**

* Gestión de la información personal de los clientes.

**Relaciones:**

* Relacionado con reservas, ventas y fidelización.

---

### 2.6 Fidelización

**Responsabilidad:**

* Gestión de programas de puntos, niveles y beneficios.

**Relaciones:**

* Asociado directamente a clientes.

---

### 2.7 Aeropuerto

**Responsabilidad:**

* Gestión de información de aeropuertos.

**Relaciones:**

* Depende de geografía y se relaciona con operaciones de vuelo.

---

### 2.8 Aeronaves

**Responsabilidad:**

* Gestión de la información técnica de aeronaves.

**Relaciones:**

* Asociado a la aerolínea y operaciones de vuelo.

---

### 2.9 Operaciones de Vuelo

**Responsabilidad:**

* Gestión de la programación y ejecución de vuelos.

**Relaciones:**

* Depende de aeropuertos y aeronaves.

---

### 2.10 Ventas

**Responsabilidad:**

* Gestión de transacciones comerciales.

**Relaciones:**

* Relacionado con clientes y pagos.

---

### 2.11 Reservas

**Responsabilidad:**

* Gestión de reservas de vuelos.

**Relaciones:**

* Depende de clientes y operaciones de vuelo.

---

### 2.12 Pagos y Facturación

**Responsabilidad:**

* Gestión de pagos, facturación y métodos de pago.

**Relaciones:**

* Asociado a ventas.

---

## 3. Dependencias entre dominios

Se identifican las siguientes dependencias clave entre dominios:

* Geografía → Aeropuerto → Operaciones de vuelo
* Clientes → Reservas → Ventas → Pagos y facturación
* Identidad → Seguridad
* Clientes → Fidelización
* Aerolínea → Aeronaves → Operaciones de vuelo

Estas dependencias permiten establecer un orden lógico tanto para la creación de estructuras como para el poblamiento de datos.

---

## 4. Justificación de la descomposición

Se propone trabajar con doce dominios en lugar de una agrupación más general debido a:

* Separación clara de responsabilidades.
* Mejor mantenibilidad del modelo.
* Facilidad para versionamiento con Liquibase.
* Escalabilidad del sistema.
* Mayor claridad en la trazabilidad de cambios.

---

## 5. Impacto en la estrategia técnica

La identificación de dominios permite:

* Organizar los changelogs de Liquibase por dominio funcional.
* Reducir conflictos en el versionamiento del esquema.
* Facilitar el trabajo en paralelo entre equipos.
* Mejorar la trazabilidad de cambios en la base de datos.

---

## 6. Conclusión

El modelo de datos presenta una estructura robusta y organizada. La identificación de doce dominios funcionales permite establecer una base sólida para su estabilización, evolución y despliegue, garantizando coherencia técnica, mantenibilidad y escalabilidad a largo plazo.
