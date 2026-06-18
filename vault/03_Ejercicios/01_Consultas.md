---
tema: 10 Consultas intermedias de práctica
estado: en-progreso
---

# 10 Consultas Intermedias — Sistema RRHH

> Estas consultas cubren los temas que se preguntan en entrevistas para desarrolladora de bases de datos. Intenta resolverlas tú primero antes de ver la solución en `scripts/05_consultas_solucion.sql`.

---

## Consulta 1 — Lista de empleados con departamento y puesto

Obtén el nombre completo de cada empleado, el nombre de su departamento y el nombre de su puesto. Ordena el resultado por departamento y luego por apellido.

**Conceptos:** `INNER JOIN` de 3 tablas, `ORDER BY`

**Columnas esperadas:**
```
nombre_completo | departamento | puesto | salario
```

---

## Consulta 2 — Empleados con salario mayor al promedio general

Lista los empleados que ganan más que el promedio de salario de todos los empleados. Muestra nombre, apellido, salario y el promedio general.

**Conceptos:** subconsulta en `WHERE`, `AVG()`

**Columnas esperadas:**
```
nombre | apellido | salario | promedio_general
```

---

## Consulta 3 — Conteo de empleados por departamento

Muestra cuántos empleados activos hay en cada departamento. Incluye el nombre del departamento, no el ID.

**Conceptos:** `GROUP BY`, `COUNT()`, `INNER JOIN`

**Columnas esperadas:**
```
departamento | total_empleados
```

---

## Consulta 4 — Departamentos con más de 3 empleados

Filtra los departamentos que tienen más de 3 empleados activos.

**Conceptos:** `GROUP BY`, `HAVING`, `COUNT()`

**Columnas esperadas:**
```
departamento | total_empleados
```

---

## Consulta 5 — Empleados sin jefe directo

Obtén los empleados que no tienen jefe asignado (son directores o posiciones de mayor jerarquía).

**Conceptos:** `IS NULL`, `JOIN`

**Columnas esperadas:**
```
nombre | apellido | puesto | departamento
```

---

## Consulta 6 — Empleados contratados en los últimos 3 años

Lista los empleados que fueron contratados a partir del 1 de junio de 2023. Muestra nombre, apellido, fecha de contratación y cuántos días llevan en la empresa.

**Conceptos:** `WHERE` con fechas, `DATEDIFF()` o `TIMESTAMPDIFF()`

**Columnas esperadas:**
```
nombre | apellido | fecha_contratacion | dias_en_empresa
```

---

## Consulta 7 — Top 3 departamentos con mayor salario promedio

Muestra los 3 departamentos con el salario promedio más alto. Incluye el nombre del departamento y el promedio formateado con 2 decimales.

**Conceptos:** `GROUP BY`, `AVG()`, `ORDER BY DESC`, `LIMIT`, `ROUND()`

**Columnas esperadas:**
```
departamento | salario_promedio
```

---

## Consulta 8 — Empleados en más de un proyecto

Encuentra los empleados que están asignados a 2 o más proyectos. Muestra nombre, apellido y cuántos proyectos tienen.

**Conceptos:** `JOIN`, `GROUP BY`, `HAVING COUNT() >= 2`

**Columnas esperadas:**
```
nombre | apellido | total_proyectos
```

---

## Consulta 9 — Departamentos sin proyectos activos

Lista los departamentos que NO tienen ningún proyecto con estado `activo` asignado.

**Conceptos:** `LEFT JOIN`, `WHERE IS NULL` o subconsulta con `NOT IN`

**Columnas esperadas:**
```
departamento | ubicacion
```

---

## Consulta 10 — Resumen completo por departamento

Genera un resumen con, por departamento:
- Cantidad de empleados activos
- Salario mínimo
- Salario máximo
- Salario promedio (redondeado a 2 decimales)
- Total de la nómina (suma de todos los salarios)

Ordena por total de nómina de mayor a menor.

**Conceptos:** `GROUP BY`, `COUNT()`, `MIN()`, `MAX()`, `AVG()`, `SUM()`, `ROUND()`, `ORDER BY`

**Columnas esperadas:**
```
departamento | empleados | salario_min | salario_max | salario_promedio | nomina_total
```

---

## Consultas extra — Procedimientos y Triggers

### Probar el procedimiento `obtener_empleados_por_depto`

```sql
CALL obtener_empleados_por_depto(1);   -- Sistemas
CALL obtener_empleados_por_depto(2);   -- Recursos Humanos
```

### Probar el trigger de historial de salarios

```sql
-- Cambiar el salario del empleado 7 (Javier)
UPDATE empleados SET salario = 40000.00 WHERE id_empleado = 7;

-- Ver que el trigger guardó el cambio automáticamente
SELECT * FROM historial_salarios;
```

### Probar el procedimiento `dar_alta_empleado`

```sql
-- Dar de alta un empleado válido
CALL dar_alta_empleado(
    'Prueba', 'Ejemplo',
    'prueba@empresa.com',
    '2025-06-01',
    18000.00,    -- dentro del rango de Analista Junior ($15k-$20k)
    1,           -- departamento: Sistemas
    1            -- puesto: Analista Junior
);

-- Intentar dar de alta con salario fuera de rango (debe dar error)
CALL dar_alta_empleado(
    'Error', 'Salario',
    'error@empresa.com',
    '2025-06-01',
    60000.00,    -- fuera del rango de Analista Junior
    1,
    1
);
```
