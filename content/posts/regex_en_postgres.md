---
title: "regex en postgres"
date: 2019-11-16T10:56:55-06:00
tags: ["postgres", "postgresql"]
draft: false
has_code: true
comments: true
---

Al hacer un query es frecuente que necesitemos encontrar registros de texto que coincidan con algunas reglas. Lo más común es buscar registros que contienen, inician o terminan con una cadena de texto en particular. 

Para esta tarea, generalmente usamos el operador `LIKE` con algunos comodines (wildcards).

Por ejemplo, usamos lo siguiente para buscar los registros en la columna "email" de la tabla "clientes", que terminen con "dominio.com".

```postgresql
SELECT * FROM clientes
WHERE email LIKE '%dominio.com'
```

Sin embargo, hay ocasiones en el texto que buscamos debe coincidir con reglas más complejas. En estos casos usamos expresiones regulares (regex).

En postgres tenemos el operador `SIMILAR TO` para buscar usando regex.

Por ejemplo, supongamos que busco correos electrónicos con un nombre de dominio en particular, pero que puede terminar en "com", "net" o "mx".

Puedo realizar esta tarea de la siguiente manera.

```postgresql
SELECT * FROM clientes
WHERE email SIMILAR TO '%dominio.(com|net|mx)'
```

Notarás que de usamos un wildcard de SQL en la cadena que buscamo. `SIMILAR TO` es un híbrido entre `LIKE` y regex, lo cual puede ser un poco confuso y afecta a su desempeño.

Por fortuna, contamos con el operador `~`. Este operador es más eficiente que `SIMILAR TO` y únicamente acepta regex por lo que es más consistente.

```postgresql
SELECT * FROM clientes
WHERE email ~'dominio.(com|net|mx)$'
```