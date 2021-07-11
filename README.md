# Postwork 01 Introducción a R y Software

Comenzamos importando los datos de soccer de la temporada 2019/2020 de la primera división de la liga española que se puede encontrar en el enlace: https://www.football-data.co.uk/spainm.php

``` R
futbolData <- read.csv("https://www.football-data.co.uk/mmz4281/1920/SP1.csv")
```
De los datos obtenidos, tomamos los goles anotados de cada equipo local (`FTHG`) y visitante (`FTAG`).

```R
homeGoals <- futbolData$FTHG  #Goles anotados por los equipos que jugaron en casa

awayGoals <- futbolData$FTAG  #Goles anotados por los equipos que jugaron como visitante
```

Utilizando la función `table()`, por medio de clasificación cruzada construimos una “tabla de contigencia”, la cual determina en cuántas ocasiones de las distintas anotaciones realizadas (0,1,2,…), se tuvo el mismo resultado, es decir, en cuántos encuentros el marcados fue 0, 1, 2, y así
sucesivamente.

```R
table(homeGoals)

#homeGoals
# 0   1   2   3   4   5   6 
#88 132  99  38  14   8   1 
```

Con las tablas de contigencias obtenidas para las anotaciones de los equipos locales y los equipos visitantes, calculamos la probabilidad marginal en cada caso, al dividirlas entre el total de partidos que se jugaron. La probabilidad simple o marginal se refiere a la probabilidad de ocurrencia de un suceso.
Matemáticamente, si <img src="https://latex.codecogs.com/svg.latex?\small&space;X"/> es la variable aleatoria que cuenta el número de goles anotados (ya sea local o visitante), esta probabilidad la calculamos como:

<p align="center">
  <img src="https://latex.codecogs.com/svg.latex?\small&space;P(X=n)=\frac{\textrm{Partidos\hspace{.1cm}con\hspace{.1cm}marcador\hspace{.1cm}igual\hspace{.1cm}a\hspace{.1cm}n}}{\textrm{Partidos\hspace{.1cm}totales}}"/>
 </p>

``` R
table(homeGoals)/length(homeGoals) # Probabilidad marginal equipo de casa

table(awayGoals)/length(awayGoals) # Probabilidad marginal equipo visitante
```
Cuando se está interesado en conocer la probabilidad de que dos sucesos se verifiquen simultáneamente, se habla de probabilidad conjunta. Para el calculo de la probabilidad conjunta se llaman ambos vectores `homeGoals` y  `awayGoals` dentro el comando `table`.

```R

table(homeGoals, awayGoals) # Tabla conjunta

#         awayGoals
#homeGoals  0  1  2  3  4  5
#        0 33 28 15  8  2  2
#        1 43 49 32  5  3  0
#        2 39 35 20  3  2  0
#        3 14 14  7  2  1  0
#        4  4  5  4  0  1  0
#        5  2  3  3  0  0  0
#        6  1  0  0  0  0  0

```

Dividimos entre la cantidad de casos para obtener la probabilidad conjunta. En este caso, si <img src="https://latex.codecogs.com/svg.latex?\small&space;X"/> es la variable aleatoria que denota el marcador local y <img src="https://latex.codecogs.com/svg.latex?\small&space;Y"/> el marcador visitante, el cálculo de tales probabilidades viene dado por:

<p align="center">
  <img src="https://latex.codecogs.com/svg.latex?\small&space;P(X=n;Y=m)=\frac{\textrm{Partidos\hspace{.1cm}cuyo\hspace{.1cm}marcador\hspace{.1cm}fue\hspace{.1cm}n-m\hspace{.1cm}}}{\textrm{Partidos\hspace{.1cm}totales}}"/>
 </p>

```R

table(homeGoals, awayGoals) / length(homeGoals) # Probabilidad conjunta

#         awayGoals
#homeGoals           0           1           2           3           4           5
#        0 0.086842105 0.073684211 0.039473684 0.021052632 0.005263158 0.005263158
#        1 0.113157895 0.128947368 0.084210526 0.013157895 0.007894737 0.000000000
#        2 0.102631579 0.092105263 0.052631579 0.007894737 0.005263158 0.000000000
#        3 0.036842105 0.036842105 0.018421053 0.005263158 0.002631579 0.000000000
#        4 0.010526316 0.013157895 0.010526316 0.000000000 0.002631579 0.000000000
#        5 0.005263158 0.007894737 0.007894737 0.000000000 0.000000000 0.000000000
#        6 0.002631579 0.000000000 0.000000000 0.000000000 0.000000000 0.000000000

```

Por último, como comprobación podemos sumar todas las probabilidades y el resultado debería ser igual a 1

```R
m <- table(homeGoals, awayGoals)/length(homeGoals)
sum(m)
```
# Postwork 02 Programación y manipulación de datos en R

Antes de comenzar este postwork se debe designar el espacio de trabajo con el comando `setwd()`, de preferencia una carpeta que contenga solo los archivos con los que se van a trabajar.

Importamos los datos de soccer de las temporadas 2017/2018, 2018/2019 y 2019/2020 de la primera división de la liga española a R, los datos los puedes encontrar en el siguiente enlace: https://www.football-data.co.uk/spainm.php .

```R
le1718 <- "https://www.football-data.co.uk/mmz4281/1718/SP1.csv"
le1819 <- "https://www.football-data.co.uk/mmz4281/1819/SP1.csv"
le1920 <- "https://www.football-data.co.uk/mmz4281/1920/SP1.csv"
```
Con ayuda de la función `download.file` descargamos los archivos en el directorio previamente elegido 

```R
download.file(url = le1718, destfile = "le1718.csv", mode = "wb")
download.file(url = le1819, destfile = "le1819.csv", mode = "wb")
download.file(url = le1920, destfile = "le1920.csv", mode = "wb")
```

Leemos cada uno de los `.csv` y los almacenamos en una lista `ligaEsp` en R para facilitar su manipulación. 

```R
ligaEsp <- lapply(dir(), read.csv)
```
Para garantizar que únicamente se lean los archivos con terminación `.csv` en  `dir()`, podemos hacer:

```R
ligaEsp <- lapply(list.files(pattern ="*.csv"), read.csv)
```
Con ayuda de los comandos `str()` , `summary()`, `head()` y `View()` conocemos un poco más la estructura de los datos, de esta manera podemos encontrar irregularidades, las cuales procesamos para dejar los datos de una manera “limpia” y optima, seleccionando solo aquellos datos necesarios para el análisis. 

```R
str(ligaEsp[[1]]); str(ligaEsp[[2]]); str(ligaEsp[[3]])

head(ligaEsp[[1]]); head(ligaEsp[[2]]); head(ligaEsp[[3]])

summary(ligaEsp[[1]]); summary(ligaEsp[[2]]); summary(ligaEsp[[3]])

View(ligaEsp[[1]]); View(ligaEsp[[2]]); View(ligaEsp[[3]])
```
En este caso en específico, observamos que los datos cuentan con un campo de fecha, pero no todos tienen el mismo formato, esto deberá ser modificado, pero antes seleccionaremos las columnas de interés (Date, HomeTeam, AwayTeam, FTHG, FTAG y FTR) utilizando la librería `dplyr`.

```R
library(dplyr)

ligaEsp <- lapply(ligaEsp, select, Date, HomeTeam:FTR)

str(ligaEsp)
```

Cambiamos el forrmato de la fecha de `chr` a `Date` con el comando `mutate`. En este paso se debe tener claro el formato en el que se encuentra escritas las fechas. Con el comando `str` verificamos que se haya realizado de forma correcta el cambio.

```R
ligaEsp[[1]] <- mutate(ligaEsp[[1]], Date = as.Date(Date, "%d/%m/%y"))
ligaEsp[[2]] <- mutate(ligaEsp[[2]], Date = as.Date(Date, "%d/%m/%Y"))
ligaEsp[[3]] <- mutate(ligaEsp[[3]], Date = as.Date(Date, "%d/%m/%Y"))

str(ligaEsp)
View(ligaEsp[[1]]); View(ligaEsp[[2]]); View(ligaEsp[[3]])
```
Por último unimos los tres dataframes en uno solo. Con el comando ``do.call()`` combinamos todos los datos en un solo dataframe. Utilizando el argumento ``rbind`` indicamos que cada dataframe individual de la lista se una en un nuevo renglon al finalizar anterior. 

Y nuevamente observamos la estructura de nuestros datos. 

```R
data <- do.call(rbind, ligaEsp)
dim(data)

View(data)
str(data)
head(data)
class(data)
``` 

Como paso extra para el postwork 03, vamos a almacenar el dataframe resultante `data` en el archivo [`dataPostwork2.csv`](https://github.com/edsatan/Proyecto-R/blob/main/Postwork-02/dataPostwork2.csv)

```R
write.csv(data, "dataPostwork2.csv", row.names = FALSE)
``` 

# Postwork 03 Análisis Exploratorio de Datos con R

Cargamos el dataframe obtenido en el [Postwork 02](https://github.com/edsatan/Proyecto-R/tree/main/Postwork-02) y modificamos el formato de las fechas (utilizando el comando `mutate()`) para facilitar más adelante su manipulación.
 ```R
data <- read.csv("https://raw.githubusercontent.com/edsatan/Proyecto-R/main/Postwork-02/dataPostwork2.csv")

data <- mutate(data, Date = as.Date(Date, "%Y-%m-%d"))
```

Seleccionamos nuestras columnas de interés (Goles de casa `FTHG` y goles de visitante `FTAG`) para realizar el cálculo de las probabilidades marginales y conjunta como en el [Postwork 01](https://github.com/edsatan/Proyecto-R/tree/main/Postwork-01)

```R
homeGoals2 <- data$FTHG
awayGoals2 <- data$FTAG
```

Calculamos las probabilidades marginales y la conjunta

* Probabilidad marginal equipo de casa

```R
probCasa <- table(homeGoals2)/length(homeGoals2)
probCasa

#homeGoals2
#          0           1           2           3           4           5           6           7           8 
#0.232456140 0.327192982 0.266666667 0.112280702 0.035087719 0.019298246 0.005263158 0.000877193 0.000877193 
```

* Probabilidad marginal equipo visitante

```R
probVisitante <- table(awayGoals2)/length(awayGoals2)
probVisitante

#awayGoals2
#          0           1           2           3           4           5           6 
#0.351754386 0.340350877 0.212280702 0.054385965 0.028947368 0.009649123 0.002631579 
```

* Probabilidad conjunta

```R
probConjunta <- table(homeGoals2, awayGoals2)/length(homeGoals2)
probConjunta

#          awayGoals2
#homeGoals2           0           1           2           3           4           5           6
#         0 0.078070175 0.080701754 0.045614035 0.018421053 0.005263158 0.004385965 0.000000000
#         1 0.115789474 0.114912281 0.068421053 0.017543860 0.008771930 0.001754386 0.000000000
#         2 0.087719298 0.093859649 0.061403509 0.011403509 0.008771930 0.001754386 0.001754386
#         3 0.044736842 0.032456140 0.024561404 0.006140351 0.001754386 0.001754386 0.000877193
#         4 0.014035088 0.010526316 0.007017544 0.000000000 0.003508772 0.000000000 0.000000000
#         5 0.008771930 0.005263158 0.004385965 0.000000000 0.000877193 0.000000000 0.000000000
#         6 0.002631579 0.001754386 0.000000000 0.000877193 0.000000000 0.000000000 0.000000000
#         7 0.000000000 0.000877193 0.000000000 0.000000000 0.000000000 0.000000000 0.000000000
#         8 0.000000000 0.000000000 0.000877193 0.000000000 0.000000000 0.000000000 0.000000000

```

Para entender mejor los resultados de las operaciones realizadas, desplegaremos unos gráficos utilizando la librería `ggplot2`. 

```R
library(ggplot2)
```

Para el caso de las probabilidades marginales, se optó por utilizar un gráfico de barras, permitiendonos observar la distribución y comportamiento de las distintas probabilidades de una manera más rápida y sencilla.

```R
probCasaPlot <- ggplot() + 
                geom_col(aes(x=0:8, y=probCasa), color="black", fill="blue")+
                ggtitle("Probabilidades Marginales FTHG") +
                ylab("Porcentaje") +
                xlab("Goles") + 
                theme_light()

probCasaPlot
```

<p align="center">
  <img src="home_prob.png" />
</p>

```R
probVisitantePlot <- ggplot() + 
                     geom_col(aes(x=0:6, y=probVisitante), color="black", fill="green")+
                     ggtitle("Probabilidades Marginales FTAG") +
                     ylab("Porcentaje") +
                     xlab("Goles") + 
                     theme_light()
probVisitantePlot
```

<p align="center">
  <img src="away_prob.png" />
</p>

Para la probabilidad conjunta, en donde interactuan dos variables, goles de locales y goles de visitantes, obtamos por un mapa de calor para ilustrar los resultados. Por medio de un mapa de color podemos determinar los disntintos valores de probabilidad obtenidos dependiendo de la tonalidad que adquieran, permitiendo un análisis visual sencillo.

```R
install.packages("reshape2")
library(reshape2)

probConjunta <- melt(probConjunta)

probConjuntaPlot <- ggplot()+
  geom_tile(aes(x=probConjunta$homeGoals2, y=probConjunta$awayGoals2, fill=probConjunta$value))+ 
  labs(x="Goles Local", y="Goles Visitante", fill="Porcentaje")+
  scale_x_continuous(breaks = unique(probConjunta[,1]))+ ##Determinamos los valores unicos para X
  scale_y_continuous(breaks = unique(probConjunta[,2]))+ ##Determinamos los valores unicos para y
  scale_fill_gradient2(high = "green", mid = "black")
probConjuntaPlot
```

<p align="center">
  <img src="conjunta.png" />
</p>


