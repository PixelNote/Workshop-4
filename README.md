# Workshop-4

## Integrantes

- Andres Felipe Jimenez Albornoz
- Miguel Angel Timoté Moya

## Descripción del problema

En un proceso industrial, se desea monitorear el nivel de un tanque de líquidos químicos para reducir el consumo de energía y desperdicio del líquido. 

Existen 3 sensores situados de tal forma que detecten los niveles deseados para su optimización. Además, hay una retroalimentación del nivel del tanque a de 5 luces que representan lo siguiente:

- Error
- Tanque vació 
- Nivel muy alto
- Nivel correcto
- Nivel muy bajo

Para esta solución se debe programar un PLC usando lógica Ladder simulando el proceso del nivel del tanque junto con una interfaz gráfica y validando la solución en un prototipo que evidencie de forma simplificada el proceso del tanque.

## Proceso de simulación

Teniendo en cuenta los 3 sensores que van a detectar el nivel de liquido en el tanque y las 5 luces que representan la respuesta de los sensores a partir de los niveles representados previamente, se realizo una tabla de verdad para tener claro, cuales son los comportamientos correctos de las 5 luces dependiendo de las combinaciones de las entradas de los sensores. Se tomo como referencia la siguiente grafica [1]:

![Tank level](https://github.com/PixelNote/Workshop-4/assets/81392047/64f5f289-7b81-478c-b175-51a1eb1b0721)

Las combinaciones restantes formarían parte del error, por ejemplo, si el sensor 2 se envía señal antes de que el sensor 1 no envié señal, resultaría en un error de detección por parte del sensor 1. Así que, las combinaciones que representan un error de detección serían las siguientes:

- Cuando el sensor 1 no envié señal, el sensor 2 no envié señal y el sensor 3 envié señal.
- Cuando el sensor 1 no envié señal, el sensor 2 envié señal y el sensor 3 no envié señal.
- Cuando el sensor 1 no envié señal, el sensor 2 envié señal y el sensor 3 envié señal.
- Cuando el sensor 1 envié señal, el sensor 2 no envié señal y el sensor 3 envié señal.

La tabla de verdad queda de la siguiente manera:

<p align="center">
  <img src="https://github.com/PixelNote/Workshop-4/assets/81392047/76c94c9b-f0f2-4f37-8d4f-84b8904de334" alt="Tabla de verdad" width="600"/>
</p>

Teniendo la tabla de verdad, se realizó el circuito lógico para los diferentes valores de entradas y salida.

Se puede observar en la tabla que, para las primeras 4 luces de salida, solo hay un valor verdadero o un valor booleano de 1. Por lo tanto, se decidió usar un operador lógico AND por cada salida equivalente según sus valores de entrada. Sin embargo, la última luz de salida presenta 4 valores verdaderos, así que realizando un mapa de Karnaugh mediante un simplificador online[2] se obtuvo la combinación igual a S2'S3 + S1'S2. Esto quiere decir que, la entrada del sensor 2 debe estar negada en una compuerta lógica AND junto con la entrada del sensor 3, la entrada del sensor 1 debe estar negada en una compuerta lógica AND junto con la entrada del sensor 2 y la salida de estas dos compuertas lógicas deben entrar a una compuerta lógica OR.

El circuito lógico queda de la siguiente manera:

<p align="center">
  <img src="https://github.com/PixelNote/Workshop-4/assets/81392047/935a8018-f8b5-4738-b3b4-fa8ad1ecb076" alt="Tabla de verdad" width="800"/>
</p>

## Referencias 

[1] Las diapositivas del cucho

[2] http://www.32x8.com/index.html






