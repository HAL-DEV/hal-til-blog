---
title: "pip-check para encontrar actualizaciones de paquetes de Python"
date: 2019-10-28T15:33:34-06:00
tags: ["python"]
draft: false
has_code: true
comments: true
---

Generalmente es buena verificar que la versión de los paquetes de Python que estamos usando en un proyecto sean las apropiadas.

Con `pip list` podemos ver una lista de todos los paquetes de python que tenemos instalados, así como la versión de cada uno de ellos. Si agregamos el flag `--outdated` obtenemos una lista de los paquetes para los cuales existe una versión más reciente.

```shell
pip list --outdated
```

Una vez identificado un paquete con una nueva versión que deseamos actualizar, usamos `pip install --upgrade *nombre del paquete*`.

Como una alternativa al proceso anterior, tenemos `pip-check`, una herramienta que nos devuelve la siguiente información con una presentación más amigable para el usuario:

* Listado de paquetes instalados
* Paquetes que tienen una versión nueva disponible
* El tipo de versión nueva disponible (minor o major release)
* Ligas a las páginas de cada paquete.

Podemos instalar `pip-check` usando `pip`.

```shell
pip install pip-check
```

Ya instalado, simplemente usamos el comando `pip-check` en la terminal.

```shell
pip-check
```

Si lo deseamos, podemos podemos usar el flag `-H` para que sólo se muestren los paquetes que tienen una nueva versión disponible.

```shell
pip-check -H
```

Y finalmente, instalamos las actualizaciones que nos interesan con `pip install --upgrade`