---
title: "Notas - Obligatorio"
output:
  word_document: default
  pdf_document: default
  html_document: default
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
knitr::opts_knit$set(root_dir = "~/Projects/r/Obligatorio")
```

# Notas

En este documento se incluyen las notas para la creación del obligatorio de la materia "Introducción a la Programación para Analítica".

## Importar datos desde archivos de Excel

Podemos usar el paquete `readxl`. 

Primero debemos instalarlo:

```{r}
install.packages("readxl")
```

Luego cargarlo a nuestra sesión:

```{r}
library(readxl)
```

La librería trae una función llamada `read_excel()` que permite leer archivo `xls` y `xlsx`.

En nuestro caso el nombre del archivo de Excel que queremos importar se llama `Obligatorio.xlsx`.

```{r}
data = read_excel('./Obligatorio.xlsx')
```

## Estudio preliminar de los datos

Primero debemos investigar que tipo tiene la nueva variable `data`.

```{r}
typeof(data)
class(data)
```

Es una lista con una clase que desconosco com trabajar. Sin embargo, la podemos convertir en un `data.frame`:

```{r}
df = as.data.frame(data)
typeof(df)
class(df)
```

Veamos la tabla.

```{r}
head(df)
```

Podemos ver que los clientes pueden tener más de un producto. Tenemos que agrupar por `id` para obtener información de cada cliente. Primero averiguemos cuantos clientes únicos tenemos.

```{r}
unique_clients = length((df %>%
  select(ID, Apellido) %>%
  group_by(ID) %>%
  summarize() %>%
  arrange(desc(ID)))$ID)
unique_clients
```

Ahora hagamoslo por genero

```{r}
df %>%
  select(ID, sexo) %>%
  group_by(ID) %>%
  mutate(s = dplyr::first(sexo))
```

---

## Notas adicionales

### Configurar la carpeta de trabajo

Para obtener la carpeta de trabajo donde estamos trabajando utilizamos la función global `getwd()`.

```{r}
getwd()
```

Mientras que para configurarla utilizamos la función global `setwd()`.

```{r}
setwd('~/Projects/r/Obligatorio')
```

**OBS: Este documento de "R markdown" esta configurado para correr por defecto en la carpeta `~/Projects/r/Obligatorio`. Esto se configura en el primer `chunk` con el resto de la configuración de `knit`. Knit es la librería utilizada para compilar el codigo `r` dentro de documento de "R Markdown".**

### Importar datos desde archivos CSV

Para cargar archivos `csv` podemos utilizar la función global `read.csv`. En el repositorio se puede encontrar una carpeta llamada `the-simpsons-by-the-data` que cuenta con múltiples archivos `csv` con información de "Los Simpsons" obtenida de [Kagle](https://www.kaggle.com/wcukierski/the-simpsons-by-the-data/downloads/the-simpsons-by-the-data.zip/1). Para cargar la lista con los episodios debemos realizar las siguiente acciones.

```{r}
# La opción `quote` es para indicarle al parser como identificar
# a los datos.
episodes = read.csv('./the-simpsons-by-the-data/simpsons_episodes.csv')
# Para ver las primeras lineas
head(episodes)
```

### Filtrar listas

Una forma de filtar listas es utilizando la función global `lapply`.

Por ejemplo:

```{r}
l = list(n1=numeric(0), n2="foo", n3=numeric(0), n4=1:5)
# Filtrar los elementos que tengan un largo igual a cero
l[lapply(l, length) > 0]
# Podemos tambien obtener el nombre de los elementos que cumplen
# con una determinada condición
names(l)[lapply(l, length) == 0]
```

### Agrupar valores de listas

Existe una librería llamada `rlist` que extiende el tipo `list` con varios metodos utiles que extienden su funcionamiento. En particular, cuenta con una gran lista de metodos que podemos utilizar para agrupar la información de las listas.

Primero tenemos que instalar la librería y cargarla a la sesión.

```{r}
install.packages("rlist")
library(rlist)
```

Ahora podemos empezar a utilizarla. Uno de los metodos utiles que agrega es la función `list.group`. Permite colocar los elementos de la lista en subgrupos al evaluar una expresión. 

En el siguiente ejemplo, partimos de una lista de 10 elementos y la dividimos en dos subgrupos: Pares e Impares.

```{r}
list.group(1:10, . %% 2 == 0)
```

El resultado de este método es una nueva lista pero cuyos elementos corresponden a los subgrupos de nuestros datos.

### Agrupar valores en `data.frames`

No tengo claro que es un `data.frame`. Lo que si se es que es una clase de `list`. Es fácil de ver haciendo: 

```{r}
class(episodes)
```

Una librería interesante para manipular `data.frames` es `dplyr`.

```{r}
library(dplyr)
```


Podemos utilizar esta librería en conjunto con el operador `%>%` (pipe) para manipular el `data.frame`. En el siguiente ejemplo se muestra como se puede agrupar la información por `season` para contar la cantidad de capitulos existentes por temporada.

```{r}
episodes %>%
  select(season) %>%
  group_by(season) %>%
  summarise(n = n())
```











