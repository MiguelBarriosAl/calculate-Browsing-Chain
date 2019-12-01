# CalculateBrowsingChain
El reto consiste en desarrollar un script que simule una navegación de usuario, se parte de un conjunto situaciones de navegación para las que se dispone de su probabilidad de ocurrencia. Se pretende conseguir un desarrollo eficiente en python que simule la navegación más probable desde un estado inicial hasta los siguientes 8 estados.


## Secuencias de navegación

Una secuencia de navegación se compone de una lista de estados codificados como números, en este caso:

```
0,2,99
```
En este flujo de navegación la sesión se inicia con el estado 0, pasa al 2 y termina en el 99.

El modelo de navegación es un modelo probabilistico para el que se tiene una probabilidad de que suceda un cambio de estado. La simulación
debe estimar cuales son los flujos de navegacion más probables a partir de una secuencia, para ello se proporcionan una serie de transiciones entre secuencias de estados y un nuevo estado. Para cada transición se incluye una probabilidad de que suceda en el atributo **PROB**.
Como en el ejemplo siguiente dónde se indica cuál la probabilidad (en tanto por uno) de pasar del estado 5,5 al estado 5,5,7 es de 0.004089979550102249 .

```
PARENT;PATTERN;PROB
5,5;5,5,7;0.004089979550102249
```

El modelo de navegación a desarrollar debe ejecutarse de forma recursiva explorando las transiciones que se pueden producir en cada uno de los niveles, por ejemplo si disponemos solamente estas transiciones:

```
PARENT;PATTERN;PROB
1,1;1,1,0;0.004
1,0;1,0,1;0.014
1,0;1,0,0;0.003
0,0;0,0,1;0.010
0,0;0,0,1;0.003
0,1;0,1,1;0.012
0,1;0,0,0;0.007
```

Podríamos construir una serie de escenarios a partir de una navegación inicial "1,1", estos escenarios sería los siguientes (definido como un árbol de navegación) para una profundidad de navegación de 3:

```

Inicio - Nivel 0: cadena:"1,1" prob: 1
 Situación Nivel 1 - caso 1: cadena:"1,1,0" prob: 0.004 - Transition used: 1,1 -> 1,1,0
    Situación Nivel 2 - caso 1: cadena:"1,1,0,0" prob: 0.004 x 0.003 - Transition used: 1,0 -> 1,0,0
        Situación Nivel 3 - caso 1: cadena:"1,1,0,0,1" prob: 0.004 x 0.003 x 0.010 - Transition used: 0,0 -> 0,0,1
        Situación Nivel 3 - caso 1: cadena:"1,1,0,0,0" prob: 0.004 x 0.003 x 0.003 - Transition used: 0,0 -> 0,0,0
    Situación Nivel 2 - caso 1: cadena:"1,1,0,1" prob: 0.004 x 0.014  - Transition used: 1,0 -> 1,0,1
        Situación Nivel 3 - caso 1: cadena:"1,1,0,1,1" prob: 0.004 x 0.014 x 0.012 - Transition used: 0,1 -> 0,1,1
        Situación Nivel 3 - caso 1: cadena:"1,1,0,1,0" prob: 0.004 x 0.014 x 0.007 - Transition used: 0,1 -> 0,1,

El script debe invocarse así:

```
$ ./calculate-browsing-chain.py current-session="0,2" browsing-depth=10 output=../data/out/output.file.csv.gz
```

Cuyos parámetros serían:
* current-session : Estado inicial en formato cadena
* browsing-depth : Profundidad de navegación a simular
* output : Nombre del fichero donde vamos a cuardar el resultado en csv comprimido
