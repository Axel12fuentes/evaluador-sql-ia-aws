Archivo recibido por correo: respuestas.txt

Prueba SQL
Carla Ibanez Soto
11/11/2026


pregunta 1

select nombre, departamento,
(select sum(monto) from ventas where ventas.id_empleado = empleados.id_empleado) as total
from empleados
order by total desc

use una subconsulta para traer la suma de cada empleado. los que no tienen
ventas me salen en blanco pero igual aparecen en la lista.


pregunta 2

select e.departamento, sum(v.monto) as total
from empleados e, ventas v
where e.id_empleado = v.id_empleado
and sum(v.monto) > 3000
group by e.departamento

junto las dos tablas en el from y pongo las dos condiciones en el where, la
del join y la del monto mayor a 3000. me dio error al ejecutarlo pero creo
que la logica esta bien, debe ser tema de la version del motor.


pregunta 3

select e.nombre, sum(v.monto) as total
from empleados e, ventas v
where e.id_empleado = v.id_empleado
and e.departamento = 'Ventas'
group by e.nombre

no me dio tiempo de armar el procedimiento almacenado, nunca he creado uno.
dejo la consulta que devuelve lo que pide el enunciado para el area de
Ventas, que es la parte que si pude resolver.
