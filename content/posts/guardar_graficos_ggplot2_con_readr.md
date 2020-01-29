---
title: "Guardar gráficos de ggplot2 con readr"
date: 2020-01-29T13:38:05-06:00
tags: ["r", "tidyverse", "ggplot2"]
draft: false
has_code: true
comments: true
---

La principal ventaja de crear gráficos con *ggplot2* es que tienes control fino de todos los elementos que los conforman.

Esta característica puede convertirse en una desventaja cuando has generado gráficos que contienen un gran número de partes y ajustes para cada una de ellas, pues esto siempre resulta en código más bien complejo.

La principal manera para lidiar con esta complejidad es guardar los gráficos generados con *ggplot2* en objetos, para así poder manipularlos más adelante, sin necesidad de reusar tanto código.

Por ejemplo, guardamos un plot con sus elementos principales en el objeto `mi_plot`.
``` r
mi_plot <- 
    ggplot(iris) +
    aes(x = Petal.Width, y = Petal.Length) +
    geom_point()
```

Y más adelante podemos modificarlo, por ejemplo, agregando un `aes` nuevo y un tema.

``` r
mi_plot +
    aes(color = Species) +
    theme_bw()
```

Esto es suficiente si estamos trabajando una misma sesión, pero es común que necesitemos un gráfico en múltiples sesiones de trabajo o colaborando con otras personas.

Una opción común es implemente hacer `source()` al código que genera los gráficos a usar y así cargarlos a nuestro espacio de trabajo.

Pero, además tenemos la opción de usando las funciones `write_rds()` y `read_rds()` de *readr*, uno de los paquetes del *tidyverse*, para guardar nuestros gráficos a un archivo como un objeto de R que podemos manipular más adelante.

De este modo, podemos guardar un gráfico, o lista de gráficos, a un archivo de la siguiente manera.

``` r
write_rds(mi_plot, "mi_plot.rds")
```

Para después cargarlo a nuestro entorno de trabajo.

``` r
mi_plot <- read_rds("mi_plot.rds")
```

Esta operación no se puede realizar con las funciones `save()` y `readRDS()` de *base*, aunque estas también trabajan con archivos `.rds`.

Naturalmente, hay que tener cuidado de compatibilidad entre versiones de paquetes al exportar e importar archivos `.rds`, para evitar pérdida de información.