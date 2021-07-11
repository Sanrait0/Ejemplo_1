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
