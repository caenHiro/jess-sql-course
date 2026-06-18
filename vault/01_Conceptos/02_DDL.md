---
tema: DDL — Lenguaje de Definición de Datos
estado: en-progreso
---

# DDL — Lenguaje de Definición de Datos

> DDL = Data Definition Language. Son los comandos que crean y modifican la **estructura** de la base de datos (no los datos en sí).

---

## Comandos DDL principales

| Comando | Para qué |
|---|---|
| `CREATE` | Crear base de datos, tabla, índice |
| `ALTER` | Modificar una tabla existente |
| `DROP` | Eliminar una tabla o base de datos |
| `TRUNCATE` | Vaciar una tabla (borra todos los registros, mantiene la estructura) |

---

## CREATE DATABASE

```sql
CREATE DATABASE rrhh;
USE rrhh;
```

`USE` le dice a MySQL con qué base de datos trabajar.

---

## CREATE TABLE

Sintaxis general:

```sql
CREATE TABLE nombre_tabla (
    columna1 TIPO_DATO CONSTRAINTS,
    columna2 TIPO_DATO CONSTRAINTS,
    ...
    CONSTRAINT nombre_fk FOREIGN KEY (columna) REFERENCES otra_tabla(columna)
);
```

### Tipos de datos más usados en MySQL

| Tipo | Para qué | Ejemplo |
|---|---|---|
| `INT` | Números enteros | `id INT` |
| `DECIMAL(p,d)` | Dinero y precisión | `salario DECIMAL(10,2)` |
| `VARCHAR(n)` | Texto variable hasta n caracteres | `nombre VARCHAR(100)` |
| `TEXT` | Texto largo | `descripcion TEXT` |
| `DATE` | Fecha `YYYY-MM-DD` | `fecha_contratacion DATE` |
| `DATETIME` | Fecha y hora | `fecha_registro DATETIME` |
| `TINYINT(1)` | Booleano (0 = false, 1 = true) | `activo TINYINT(1)` |
| `ENUM(...)` | Lista cerrada de valores | `estado ENUM('activo','inactivo')` |

---

## Ejemplo: tabla `departamentos`

```sql
CREATE TABLE departamentos (
    id_departamento INT          PRIMARY KEY AUTO_INCREMENT,
    nombre          VARCHAR(100) NOT NULL,
    ubicacion       VARCHAR(100),
    presupuesto     DECIMAL(15,2)
);
```

- `AUTO_INCREMENT` — MySQL genera el ID automáticamente: 1, 2, 3...
- `NOT NULL` — el nombre es obligatorio
- `ubicacion` y `presupuesto` son opcionales (pueden ser NULL)

---

## Ejemplo: tabla con Foreign Key

```sql
CREATE TABLE empleados (
    id_empleado       INT          PRIMARY KEY AUTO_INCREMENT,
    nombre            VARCHAR(100) NOT NULL,
    apellido          VARCHAR(100) NOT NULL,
    email             VARCHAR(150) UNIQUE NOT NULL,
    fecha_contratacion DATE         NOT NULL,
    salario           DECIMAL(10,2) NOT NULL,
    id_departamento   INT,
    id_jefe           INT,
    activo            TINYINT(1) DEFAULT 1,

    CONSTRAINT fk_emp_depto FOREIGN KEY (id_departamento)
        REFERENCES departamentos(id_departamento),

    CONSTRAINT fk_emp_jefe FOREIGN KEY (id_jefe)
        REFERENCES empleados(id_empleado)
);
```

Puntos clave:
- `UNIQUE` en `email` — no puede haber dos empleados con el mismo correo
- `DEFAULT 1` — si no especificas `activo`, vale `1` (activo)
- `id_jefe` referencia a la misma tabla `empleados` — esto se llama **autoreferencia**

---

## ALTER TABLE — modificar una tabla existente

```sql
-- Agregar una columna
ALTER TABLE empleados ADD COLUMN telefono VARCHAR(15);

-- Modificar el tipo de una columna
ALTER TABLE empleados MODIFY COLUMN telefono VARCHAR(20);

-- Eliminar una columna
ALTER TABLE empleados DROP COLUMN telefono;

-- Agregar un índice
ALTER TABLE empleados ADD INDEX idx_apellido (apellido);

-- Agregar una FK después de crear la tabla
ALTER TABLE empleados
    ADD CONSTRAINT fk_emp_puesto
    FOREIGN KEY (id_puesto) REFERENCES puestos(id_puesto);
```

---

## DROP TABLE

```sql
-- Eliminar tabla (y todos sus datos)
DROP TABLE empleados;

-- Solo si existe (no da error si no existe)
DROP TABLE IF EXISTS empleados;
```

**Cuidado:** si la tabla tiene datos y otras tablas tienen FK que apuntan a ella, MySQL no dejará eliminarla.

---

## TRUNCATE TABLE

```sql
TRUNCATE TABLE empleados;
```

- Borra todos los registros
- Mantiene la estructura de la tabla (columnas, constraints)
- Reinicia el `AUTO_INCREMENT` a 1
- Es más rápido que `DELETE FROM empleados` porque no registra cada fila eliminada

---

## Orden correcto para crear tablas relacionadas

Las tablas deben crearse en el orden correcto: **primero las que no tienen FK**, luego las que dependen de ellas.

Para el sistema de empleados:
```
1. departamentos
2. puestos
3. empleados          (FK → departamentos, puestos, empleados)
4. proyectos          (FK → departamentos)
5. empleado_proyecto  (FK → empleados, proyectos)
6. historial_salarios (FK → empleados)
```

---

## Orden correcto para eliminar tablas

Al revés — primero las que tienen FK, luego las referenciadas:
```
1. historial_salarios
2. empleado_proyecto
3. proyectos
4. empleados
5. puestos
6. departamentos
```

---

> Siguiente: [[03_DML]] — insertar, actualizar y consultar datos
