Archivo recibido por correo: prueba_sql_bruno.txt

PRUEBA TECNICA SQL - ANALISTA DE DATOS
Nombre: Bruno Caceres Delgado
Fecha: 10/11/2026

PREGUNTA 1

SELECT e.nombre, e.departamento, SUM(v.monto) AS total_vendido
FROM empleados e
INNER JOIN ventas v ON e.id_empleado = v.id_empleado
GROUP BY e.nombre, e.departamento
ORDER BY total_vendido DESC;

uno las dos tablas por id_empleado, sumo el monto y ordeno de mayor a menor.


PREGUNTA 2

WITH total_depto AS (
    SELECT e.departamento, SUM(v.monto) AS total
    FROM empleados e
    INNER JOIN ventas v ON e.id_empleado = v.id_empleado
    GROUP BY e.departamento
    HAVING SUM(v.monto) > 3000
)
SELECT departamento, total
FROM total_depto
ORDER BY total DESC;

armo la CTE con el total por departamento y filtro con HAVING los que pasan
de 3000. me salen dos departamentos.


PREGUNTA 3

CREATE PROCEDURE sp_ventas_por_departamento
AS
BEGIN
    SELECT e.nombre, SUM(v.monto) AS total_vendido
    FROM empleados e
    INNER JOIN ventas v ON e.id_empleado = v.id_empleado
    WHERE e.departamento = 'Ventas'
    GROUP BY e.nombre;
END;

se ejecuta asi:
EXEC sp_ventas_por_departamento;

devuelve los empleados de Ventas con su total. si se necesita otro
departamento se edita la consulta y se cambia el valor del WHERE.
