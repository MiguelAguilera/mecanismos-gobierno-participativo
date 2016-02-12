
# Algoritmos de filtrado colaborativo con decaimiento temporal

### Algoritmo de Reddit

El algoritmo de reddit, tal y como está definido en su código fuente [0] sigue la siguiente fórmula:
```
def hot(ups, downs, date):
    s = score(ups, downs)
    order = log10(max(abs(s), 1))
    if s > 0:
        sign = 1
    elif s < 0:
        sign = -1
    else:
        sign = 0
    seconds = date - 1134028003
    return round(sign * order + seconds / 45000, 7)
    
 ```
 El algorítmo de Reddit está diseñado para que el contenido se renueve de forma más o menos diaria, 
 de forma que el número de puntos de cada propuesta se divide entre 10 cada 12 horas aproximadamente 
 (es decir, en 1 día su valor se divide entre 100). Esta dinámica está controlada por un parámetro de decaimiento $T$ que en el código de arriba es igual a 45000.
 Para procesos con dinámicas más largas (p.ej. procesos más deliberativos), simpemente habría que modificar el parámetro con valor 45000 en el algoritmo por un valor mayor.
 Por ejemplo, multiplicar el valor de T por 5, sustituyendo la fórmula final por `return round(sign * order + seconds / 225000, 7)`, conseguimos que una propuesta con 5 días de antiguedad necesitaría 5 días para que su valor se dividiera entre 100.
 
 ![](decay.png)
 Figura 1. Comparación del decaimiento de la puntuación para diferentes parametros de $T$. En azul, la configuración del algoritmo tal y como la usa Reddit, ajustado para un periodo de unas 12 horas, y en rojo, el algorimto ajustado para una temporalidad más larga de 60 horas.
 
### Decaimiento multiescala

Como hemos visto arriba, el algoritmo de reddit es equivalente a un decaimiento exponencial, en el que el valor de un post es igual a $$s = v·e^(-h·t)$$, donde $$v$$ es el número de votos y $$h$$ es una constante. El comportamiento de este algoritmo filtra la actividad en una escala temporal concreta.

Podemos plantearnos como objetivo un algoritmo que equilibre la actividad a todas las escalas del sistema. Por ejemplo, podríamos querer que el algoritmo trate de tener en portada las mejores propuestas a diferentes escalas temporales (p.ej. la mejor propuesta del día, la mejor de la semana, la mejor del mes, etc.).

Una forma para conseguir esto sería tener en cuenta el decaimiento a diferentes escalas, siendo $$s = v·\sum_i e^(-h_i·t)$$ y $$h_i$$ diferentes constantes que ponderan la presencia relativa de una escala temporal concreta.

Para equilibrar la presencia de diferentes escalas temporales, es necesario optimizar el valor de las variables $$h_i$$ para tener la mayor diversidad de escalas temporales entre las propuestas de la portada. Para ello, podemos utilizar una medida de entropía $$H(X) = -\sum_x{p(x)log_2(x)}$$, en la que x son las escalas temporales del tiempo de vida de las propuestas en la portada. Estas escalas deberían estar definidas de forma logarítmica, p.ej. de forma que $$x$$ es igual a 0 si su duración es menor de un día, 1 si es menor que 2 días, 2 si es menor que 4, etc.

Un ejemplo de optimización de los valores de $$h_i$$ para valores reales utilizando un algoritmo genético microbiano [1] puede encontrarse [aquí](https://github.com/MiguelAguilera/mecanismos-gobierno-participativo/tree/master/code/multiscale-sorting-algorithm).
 
#Altoritmos de ordenación de propuestas según consenso

### Positivos menos negativos

Es el más simple. Simplemente resta el número de votos positivos y negativos y ordenar de mayor a menor.
El problema es que no captura exactamente el consenso de una propuesta. 
Una propuesta con 10 positivos y 1 negativo tiene el mismo número de puntos que una propuesta con 100 positivos y 99 negativos.

![](postivos-menos-negativos.png)
Figura 2. Distribución de puntuación según el número de votos de cada tipo calculando el número de votos positivos menos negativos.

### Porcentaje de positivos

Otra opción simple es calcular el porcentaje de positivos de cada propuesta y ordenar de mayor a menor.
El problema es que no captura la cantidad de apoyo de cada propuesta.
Una propuesta con 5 positivos tendría más puntos que una propuesta con 100 positivos y 10 negativos.

![](porcentaje.png)
Figura 3. Distribución de puntuación según el número de votos de cada tipo calculando el porcentaje de votos positivos.

### Consenso aproximado

Una opción que recoge lo mejor de las dos anteriores es calcular tanto la cantidad de apoyo como el porcentaje de positivos.
En plataformas para encontrar propuestas de consenso como las utilizadas por Zaragoza en Común o Ahora Madrid se utilizó la siguiente fórmula:
```
def consenso_aproximado(ups, downs):
  return log2(ups+downs) * ((ups-downs)/(ups+downs))
```
La fórmula que hace es calcular la fracción de votos positivos, y multliplicarlo por el logaritmo base dos del número de votos. El log2(ups+downs) hace que cada vez que la propuesta tiene el doble de votos que otra se suma un punto. Así, si tenemos una propuesta con 100 votos y 90% de positivos, se considera más de consenso que p.ej. una propuesta con 5 votos y 100% de positivos.

![](consenso-aproximado.png)
Figura 2. Distribución de puntuación según el número de votos de cada tipo calculando la fórmula de 'consenso aproximado'.
  
  [0] https://github.com/reddit/reddit/blob/c6f959504466333c0d7d51c131240473aaf78b04/r2/r2/lib/db/_sorts.pyx
  [1] Harvey, I. (2009). The microbial genetic algorithm. In Advances in artificial life. Darwin Meets von Neumann (pp. 126-133). Springer Berlin Heidelberg.
