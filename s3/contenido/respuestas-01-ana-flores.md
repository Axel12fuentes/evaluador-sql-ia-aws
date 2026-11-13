Archivo recibido por correo: respuestas_ana_flores.txt

PRUEBA TECNICA SQL - ANALISTA DE DATOS
Nombre: Ana Flores Quiroz
Fecha: 10/11/2026
Motor: SQL Server

-------------------------------------------------
PREGUNTA 1
-------------------------------------------------
SELECT
    e.id_empleado,
    e.nombre,
    e.departamento,
    ISNULL(SUM(v.monto), 0) AS total_vendido
FROM empleados AS e
LEFT JOIN ventas AS v
    ON e.id_empleado = v.id_empleado
GROUP BY
    e.id_empleado,
    e.nombre,
    e.departamento
ORDER BY
    total_vendido DESC;

Comentario: uso LEFT JOIN para no perder a los empleados sin ventas.
El ISNULL convierte el nulo del SUM en 0. Salen las 10 filas.

-------------------------------------------------
PREGUNTA 2
-------------------------------------------------
WITH ventas_por_departamento AS (
    SELECT
        e.departamento,
        SUM(v.monto) AS total_departamento
    FROM empleados AS e
    INNER JOIN ventas AS v
        ON e.id_empleado = v.id_empleado
    GROUP BY
        e.departamento
    HAVING
        SUM(v.monto) > 3000
)
SELECT
    departamento,
    total_departamento
FROM ventas_por_departamento
ORDER BY
    total_departamento DESC;

Comentario: el filtro va en HAVING porque se aplica despues de agrupar.
Con WHERE no se puede usar SUM. Resultado: Ventas 9610.50 y Marketing 3270.00.

-------------------------------------------------
PREGUNTA 3
-------------------------------------------------
CREATE PROCEDURE sp_ventas_por_departamento
    @departamento VARCHAR(50)
AS
BEGIN
    SET NOCOUNT ON;

    SELECT
        e.id_empleado,
        e.nombre,
        ISNULL(SUM(v.monto), 0) AS total_vendido
    FROM empleados AS e
    LEFT JOIN ventas AS v
        ON e.id_empleado = v.id_empleado
    WHERE e.departamento = @departamento
    GROUP BY
        e.id_empleado,
        e.nombre
    ORDER BY
        total_vendido DESC;
END;
GO

Ejemplo de uso:
EXEC sp_ventas_por_departamento @departamento = 'Ventas';

Comentario: el parametro entra en el WHERE para que sirva con cualquier
departamento. Mantengo el LEFT JOIN por si el area no tiene ventas.
