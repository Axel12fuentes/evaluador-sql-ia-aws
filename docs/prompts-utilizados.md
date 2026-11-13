# Prompt utilizado — Ejercicio 2 (correccion de pruebas tecnicas de SQL)

Creado con asistencia de IA. Modelo LLM usado: **Claude Opus 5 (Anthropic)**.
Fecha de creacion: **2026-11-13**.

---

## Prompt 1 — Archivo de instrucciones para el Agente de IA

```md
## Rol / Persona
Actua como el evaluador tecnico de una empresa que toma pruebas de SQL a sus postulantes.

## Contexto
Tengo un workflow serverless en AWS (API Gateway + Step Functions + Lambda + DynamoDB + S3)
que llama a un API generica de Agente de IA (Groq, modelo openai/gpt-oss-120b). El workflow
recibe un archivo markdown de instrucciones y una carpeta S3 con archivos markdown de
contenido; evalua cada archivo y consolida los resultados en un CSV comparativo delimitado
por punto y coma.

Quiero reutilizar el mismo workflow en un contexto distinto al de evaluar Curriculums Vitae:
CORRECCION AUTOMATICA DE PRUEBAS TECNICAS DE SQL.

El caso es este: la empresa envia una prueba de SQL a los postulantes al puesto de Analista
de Datos. Son tres preguntas que el candidato resuelve en vivo escribiendo las sentencias,
sin ejecutarlas contra una base de datos. Cada uno escribe sus respuestas en un archivo de
texto del bloc de notas y lo manda por correo. El reclutador recibe decenas de esos archivos
y necesita corregirlos con el mismo criterio.

## Tarea / Objetivo
Elabora el archivo de instrucciones en formato markdown para que el Agente de IA corrija la
prueba de un candidato por vez.

Debe contener:
- El enunciado completo de la prueba, con dos tablas pequenias de 10 filas cada una
  (empleados y ventas) escritas dentro del propio archivo, para que el agente conozca el
  resultado esperado sin consultar ninguna base de datos.
- Tres preguntas de dificultad baja a media, una por tecnica: LEFT JOIN con agregacion y
  manejo de nulos, CTE combinada con GROUP BY y HAVING, y un procedimiento almacenado con
  parametro de entrada.
- Las respuestas correctas de referencia, con los valores exactos que debe devolver cada
  consulta.
- Una rubrica de 100 puntos: 30 por pregunta y 10 por estilo y claridad.
- Una regla dura: si la tecnica pedida no se usa, esa pregunta no puede pasar del 40% de su
  puntaje aunque el resultado se parezca al esperado.
- Una escala que convierta el puntaje en nivel y en resultado del proceso.

Los datos de las tablas deben estar armados para que las tres tecnicas importen de verdad:
algunos empleados sin ninguna venta, para que el INNER JOIN de un resultado incompleto, y un
departamento en cero, para que el HAVING tenga que dejarlo fuera.

## Requisitos de la respuesta
- El agente debe responder UNICAMENTE con un objeto JSON PLANO, porque las claves de primer
  nivel se convierten en las columnas del CSV comparativo del workflow.
- Las claves deben ir en minusculas, sin tildes y sin enie.
- Prohibe el punto y coma (;), las comillas dobles y los saltos de linea dentro de los
  valores de texto, porque el CSV usa punto y coma como delimitador.
- Incluye un campo de estado y un comentario de retroalimentacion por cada pregunta, para
  que el CSV sirva como devolucion al candidato y no solo como nota.
- El contenido no debe superar 3 paginas A4 (limite de tokens de la capa gratuita de Groq).
```

---

## Prompt 2 — Archivos de contenido (respuestas de los candidatos)

```md
## Rol / Persona
Actua como un generador de casos de prueba para una herramienta de correccion automatica.

## Contexto
Necesito validar que el archivo de instrucciones corrige bien cuando se ejecuta con el
workflow serverless de IA.

## Tarea / Objetivo
Genera 3 archivos con las respuestas de 3 candidatos ficticios a la prueba de SQL.

Importante: los candidatos responden en vivo y envian su archivo por correo escrito en el
bloc de notas. Los documentos deben verse como TEXTO PLANO, sin formato markdown, sin
titulos con almohadilla y sin bloques de codigo. Solo el encabezado con el nombre, la
etiqueta de cada pregunta, la sentencia SQL tal como la tipeo la persona y algun comentario
suelto al final de cada respuesta.

1. Candidata solida: las tres respuestas correctas. LEFT JOIN con ISNULL, CTE con HAVING y
   procedimiento con parametro de entrada. Escritura prolija. Debe superar los 85 puntos.
2. Candidato intermedio: usa INNER JOIN en la pregunta 1, por lo que pierde a los empleados
   sin ventas. La pregunta 2 la resuelve bien. En la pregunta 3 crea el procedimiento pero
   sin parametro, con el departamento escrito fijo en el WHERE. Debe quedar entre 45 y 70.
3. Candidata insuficiente: usa una subconsulta en el SELECT en vez de LEFT JOIN, en la
   pregunta 2 pone SUM dentro del WHERE y junta las tablas con una coma en el FROM, y en la
   pregunta 3 admite que no sabe crear procedimientos y entrega una consulta suelta. Escribe
   todo en minusculas y sin sangria. Debe quedar por debajo de 45.

## Requisitos de la respuesta
- Nombres ficticios. No usar datos de personas reales.
- Cada archivo no debe superar 1 pagina A4.
- Los errores deben estar realmente en el codigo, no solo descritos en los comentarios.
```
