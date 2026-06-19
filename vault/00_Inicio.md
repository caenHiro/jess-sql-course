---
title: Curso Express SQL — Inicio
tags:
  - sql
  - mysql
  - curso
date: 2026-06-18
---

# Curso Express SQL

Curso de preparación para entrevista técnica de desarrolladora de base de datos.

---

## Ruta de aprendizaje

### 1. Levanta el entorno
Antes de estudiar teoria, ten MySQL corriendo en Docker y DBeaver conectado.

- [[02_Setup/01_Docker_MySQL|Levantar MySQL con Docker]]
- [[02_Setup/02_DBeaver|Conectar DBeaver]]

### 2. Teoria

Estudia en orden. Cada nota tiene ejemplos que puedes ejecutar en DBeaver.

| Nota | Tema |
|---|---|
| [[01_Conceptos/01_Fundamentos_BD]] | SMBD, BD relacional, tablas, integridad, constraints |
| [[01_Conceptos/02_DDL]] | CREATE, ALTER, DROP — estructura de tablas |
| [[01_Conceptos/03_DML]] | SELECT, INSERT, UPDATE, DELETE — datos |
| [[01_Conceptos/04_Procedimientos_Almacenados]] | DELIMITER, IN/OUT/INOUT, CALL |
| [[01_Conceptos/05_Disparadores]] | BEFORE/AFTER, NEW/OLD, SIGNAL |
| [[01_Conceptos/06_Consultas_SELECT_y_Funciones]] | SELECT, WHERE, GROUP BY, HAVING, funciones de agregacion |

### 3. Crea la base de datos

Ejecuta los scripts en DBeaver en este orden:

```
scripts/01_ddl.sql          -- crea las tablas
scripts/02_dml.sql          -- inserta los datos de prueba
scripts/03_procedimientos.sql
scripts/04_disparadores.sql
```

### 4. Resuelve los ejercicios

Lee primero [[03_Ejercicios/00_Descripcion_BD]] para entender el modelo de datos.

| Ejercicios | Nivel | Soluciones |
|---|---|---|
| [[03_Ejercicios/01_Consultas_Basicas]] | Basico — SELECT, WHERE, ORDER BY, LIKE, BETWEEN, IN | `scripts/05_consultas_basicas_solucion.sql` |
| [[03_Ejercicios/02_Consultas_Intermedias]] | Intermedio — GROUP BY, HAVING, subconsultas, JOINs multiples | `scripts/06_consultas_intermedias_solucion.sql` |

> Intenta resolver cada consulta tú sola antes de ver la solución.

---

## Estructura del vault

```
alumno/vault/
├── 00_Inicio.md             <-- estas aqui
├── 01_Conceptos/            teoria: fundamentos, DDL, DML, procedures, triggers, consultas
├── 02_Setup/                como levantar Docker y conectar DBeaver
└── 03_Ejercicios/           descripcion de la BD + consultas basicas e intermedias
```

---

## Conexion a la base de datos

| Campo | Valor |
|---|---|
| Host | `localhost` |
| Puerto | `3306` |
| Base de datos | `rrhh` |
| Usuario | `root` |
| Password | `rrhh2025` |
