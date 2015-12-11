# Indicadores



## Entropía
El concepto básico de entropía en teoría de la información tiene mucho que ver con la incertidumbre que existe en cualquier experimento o señal aleatoria. Es también la cantidad de "ruido" o "desorden" que contiene o libera un sistema. De esta forma, podremos hablar de la cantidad de información que lleva una señal [0]

La fórmula de la entropía de Shannon es: 

$$H(X) = -\sum{p(x_i)log_2(x_i)}$$

Donde $$x_i$$ son los estados de una variable aleatoria $$X$$

El resultado es la cantidad de información contenida en una varaible medida en "bits" (si utilizamos una base binaria al emplear un logaritmo en base 2).


### Ejemplo 1: resultados de procesos de votación o encuesta ciudadana

Basar la evaluación de un proceso de consulta en indicadores cuantitativos simples (p.ej. número de participantes) puede desvirtuar el impacto de un proceso de consulta y hacer pasar por participación genuina procesos de participación puramente "cosméticos" en los que no se está tomando una decisión real (p.ej. si sólo hay una opción con posibilidades reales de ser elegida).

Siendo $$X$$ el conjunto de opciones en un proceso de consulta, la entropía $$H(X)$$ mide la cantidad de información recogida en el proceso de consulta. Por ejemplo, una consulta con tres posibles respuestas en las que el 99% opta por la respuesta $$A$$, el 1% por la respuesta $$B$$, y el 0% por la $$C$$, tendrá una entropía de 0.081 bits, mientras que una consulta en la que el voto entre $$A$$, $$B$$ y $$C$$ se reparta con el 50, 30 y 20% tiene una entropía de 1.49 bits, y por tanto recoge mucha más información.

Para consultas con diferentes preguntas con opciones de respuesta $$X_n$$, $$n \in \{1, 2, ... , N\}$$, puede obtenerse la entropía total como $$\sum_{n=1}^N H(X_n)$$.

## TSE Complexity

The Tononi-Sporns-Edelmans (TSE) complexity [TSE94] is a complexity measure for distributions. It is designed so that it maximized by distributions where small subsets of random variables are loosely coupled but the overall distribution is tightly coupled.

$$TSE(X) = \sum_{k=1}^{|X|} \left( {N \choose k}^{-1} \sum_{\substack{y \subseteq X \\ |y|=k}} \left( H(y) \right) - \frac{k}{|X|}H(X) \right) $$

### Ejemplo 1: resultados de procesos de votación o encuesta ciudadana

En un caso de un proceso de consultas, la máxima Entropía correspondería con una situación en la que cada participante elige respuestas al azar.Para que un proceso de consulta capture la complejidad social de una situación, no sólo debe ser capaz de recoger mucha información sino de recoger información estructurada.

En un proceso con varias preguntas $$X = {y_1, y_2, ... , y_N}$$, la complejidad TSE(X) permite medir la riqueza de la estructura de respuestas recogidas. Esto es, si las respuestas obtenidas permiten identificar diferentes grupos con preferencias similares, clasificar diferenets posiciones políticas y sociales coherentes, etc. el valor de complejidad será alto. Si las respuestas no son más que "ruido" estadístico, los valores de complejidad tenderán a cero. 

[0] https://es.wikipedia.org/wiki/Entrop%C3%ADa_%28informaci%C3%B3n%29