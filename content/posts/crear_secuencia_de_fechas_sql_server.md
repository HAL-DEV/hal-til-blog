---
title: "Crear una secuencia de fechas en SQL Server"
date: 2020-03-29T22:00:26-06:00
draft: true
tags: ["sql", "sql server"]
draft: false
has_code: true
comments: true
---

Crear una secuencia de fechas durante una consulta es una herramienta útil para crear series de tiempo completas, realizar comparaciones y verificar la integridad de datos, entre otras aplicaciones.

Por ejemplo, podemos pasar de tener datos como los siguientes:

| fecha | valor
|----   |----
|2020-01-01 | 1
|2020-01-03 | 1
|2020-01-05 | 1

A datos como estos:

 fecha | valor
----   |----
2020-01-01 | 1
2020-01-02 | 0
2020-01-03 | 1
2020-01-04 | 0
2020-01-05 | 1

En otras implementaciones de SQL esta es una tarea sencilla. Postgres, por ejemplo, tiene la función `GENERATE_SERIES()` que facilita esta tarea. En SQL Server esto es un poco más elaborado.

Obtenemos una secuencia de fechas del primero al cinco de enero de 2020 con el siguiente bloque de código.

``` sql
WITH calendario As
(
	SELECT DATEADD(day, 1, '2019-12-31') AS fecha
	UNION ALL
	SELECT DATEADD(day, 1, fecha) FROM calendario
	WHERE fecha < '2020-01-05'
)
SELECT * FROM calendario
OPTION (MAXRECURSION 5)
```

Lo que estamos haciendo es crear una vista temporal (Common table expression, CTE) llamada `calendario` con `WITH`, esta tiene una columna "fecha", con un reglón que contiene el primer día después de la fecha que especifiquemos.

Hemos especificado '2019-12-31' para que la primera fecha sea el primero de enero de 2020.

Después, a esta vista, se le agregan renglones de manera recursiva usando `UNION ALL`, cada uno con fechas un día después del anterior, hasta llegar al día que hemos especificado. En este caso '2020-01-05'.

Es muy importante incluir al final de nuestra consulta la declaración `OPTION (MAXRECURSION <n>)`. 

Esta declaración indica el número máximo de recursiones permitidas para generar nuestra secuencia de fecha, protegiéndonos de errores.

El número debe ser mayor o igual a la cantidad de días que deseamos incluir en nuestra secuencia, de lo contrario se nos devolverá un error.

El resultado de la consulta anterior será el siguiente, que podremos usar durante el resto de la consulta.

|fecha
|----
|2020-01-01
|2020-01-02
|2020-01-03
|2020-01-04
|2020-01-05

Si deseamos crear una secuencia de fechas que termine en el día de hoy, podemos usar la función `GETDATE()` para obtener un dato de fecha del día actual.
