---
title: "Cambiar la zona horaria de datos Timestamp o Detetime en un query de SQL"
date: 2019-10-22T16:23:32-05:00
tags: ["sql"]
draft: false
has_code: true
comments: true
---

Es común que las fechas almacenadas en una tabla no se encuentren en la misma zona horaria que las necesitamos.

Podemos cambiar la zona horaria de nuestros datos al realizar un query, sin necesidad de hacer cambios directamente a la tabla que los contiene.

Para ello necesitamos el código de la la zona horario en la que han sido almacenados los datos y el código de la zona a la que deseamos cambiarlos.

Supongamos que nuestros datos se encuentran en **mi_tabla** en una columna llamada **hora registro**. Han sido almacenados con la zona horaria UTC (*Coordinated Universal Time*, 00:00) y queremos cambiarlos a CDT (*Central Daylight Time*, -05:00), la zona horaria de la Ciudad de México.

Hacemos lo siguiente.

```sql 
SELECT (hora_registro AT time ZONE 'utc') AT time ZONE 'cdt' AS hora_registro
FROM mi_tabla
```

Usamos `AT time Zone` para declarar la zona horaria almacenada, UTC en nuestro ejemplo, y después una vez, para declarar la zona a la que deseamos cambiar los datos, que en este caso es CDT.

En este ejemplo, el resultado será la columna **hora_registro** con un ajuste de -05:00 horas con respecto al original. Una fecha almacenada como 4:00 am del 2 de enero, será devuelta como 11:00 pm del 1 de enero.

El procedimiento anterior funciona en la mayoría de los dialectos de SQL, tanto para datos de tipo Timestamp como Datetime.

Una lista de zonas horarias se encuentra en el siguiente enlace:

* https://www.timeanddate.com/time/zones/
