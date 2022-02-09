# Proyecto_-Final_Reinforcement_Learning


## Definición del problema:

**Portafolio: Conjunto de acciones**

En este proyecto queremos construir la solución al problema de optimización de portafolios mediante aprendizaje reforzado. Para ello, el agente está tomando continuamente la desición de como reubicar los recursos (en este caso el porcentaje de las acciones) dentro del portafolio con el objetivo de maximizar el retorno. En este caso, consideramos tratar de construir un portafolio con acciones de diferentes sectores y de esta manera disminuimos el riesgo.

## Metodología

**Para la solución del problema se plantea la siguiente metodología**

1). Construcción del tensor de precios.

Para cada una de las acciones el precio de apertura, cierre, máximo y mínimo serán escalados empleando el valor del precio de apertura, es decir para cada variable se tendra:

$X_i = \left[\frac{X_{i, t-n-1}}{Open_{t-n-1}},....,\frac{X_{i, t-1}}{Open_{t-1}} \right], \quad i=[Open, Close, High, Low]$


Por lo tanto, al final del periodo $t$, el vector de precio de entrada para nuestra red neuronal) será de la forma $(x, y,z)$, en donde $x$ es el número de características, y es el número de acciones y z el número de periodos a considerar.

2). Definición del ambiente y el agente:

En este caso el mercado financiero es nuestro ambiente, tratamos de hacer lo más real posible este mercado incluyendo acciones de todo tipo. 


3). Definición de la acción

La acción realizada por un agente en el momento $t$ es reasignar el porcentaje de cada acción dentro del portafolio. Por lo tanto, determinar el vector de pesos $w_t$.

$accion_t = w_t = [ w_{acc_1}, w_{acc_2}, ..., w_{acc_n}]$

Adicionalmente, en el proceso de reubicación de las acciones es usual incluir un costo por transacción, este se define como:

$cost_t = Vportafolio_{t-1}* tasa_{trans}* (w_t-w_{t-1})$

Este resultado se emplea cada vez que se actualiza, el costo de la transacción debe tenerse en cuenta:

$vectporta_t = (\sum Vportafolio_{t-1}*w_t)- (cost_t)$


**Estados**

$Estado_t = (Pricetensor_t, w_{t-1}, Vportafolio_{t-1})$

**Recompensa**

$Reward_{t} = (Vportafolio_t/ Vportafolio_{t-1}) - 1$

**Política: Arquitectura de red neuronal CNN**

Esta función orienta al agente sobre qué acción tomar a partir de un estado concreto, es decir cómo reasignar los pesos entre los activos para maximizar
beneficio. Para ello se emplea una red neuronal CNN para diseñar la función de política. La arquitectura de la red se muestra a continuación:

![title](https://raw.githubusercontent.com/ancastillar/Proyecto_Final_Series_Tiempo/main/datos/cnn.png)

**Anotaciones**

* La entrada de la red corresponde al tensor de precios, previamente definido, y la salida de la red es el vector de pesos $w_t$, el valor del portafolio y la recompensa en el tiempo $t$.

* El vector del valor del portafolio producido por la red
en cada paso de tiempo se almacena en una memoria de vectores 
(PVM) para ser utilizado en el siguiente paso temporal.


* Para que la red neuronal minimice el coste de la transacción, el vector del portafolio del paso de tiempo anterior se inserta antes de
la última capa. 
