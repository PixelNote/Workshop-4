# Workshop-4

## Integrantes

- Andres Felipe Jimenez Albornoz
- Miguel Angel Timoté Moya

## Descripción del problema

En un proceso industrial, se desea monitorear el nivel de un tanque de líquidos químicos para reducir el consumo de energía y desperdicio del líquido. 

Existen 3 sensores situados de tal forma que detecten los niveles deseados para su optimización. Además, hay una retroalimentación del nivel del tanque a de 5 luces que representan lo siguiente:

- Error
- Tanque vacio
- Nivel muy alto
- Nivel correcto
- Nivel muy bajo

Para esta solución se debe programar un PLC usando lógica Ladder simulando el proceso del nivel del tanque junto con una interfaz gráfica y validando la solución en un prototipo que evidencie de forma simplificada el proceso del tanque.

## Proceso de simulación

Teniendo en cuenta los 3 sensores que van a detectar el nivel de liquido en el tanque y las 5 luces que representan la respuesta de los sensores a partir de los niveles representados previamente, se realizo una tabla de verdad para tener claro, cuales son los comportamientos correctos de las 5 luces dependiendo de las combinaciones de las entradas de los sensores. Se tomo como referencia la siguiente grafica:

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

Se puede observar en la tabla que, para las primeras 4 luces de salida, solo hay un valor verdadero o un valor booleano de 1. Por lo tanto, se decidió usar un operador lógico AND por cada salida equivalente según sus valores de entrada. Sin embargo, la última luz de salida presenta 4 valores verdaderos, así que realizando un mapa de Karnaugh mediante un simplificador online[1] se obtuvo la combinación igual a S2'S3 + S1'S2. Esto quiere decir que, la entrada del sensor 2 debe estar negada en una compuerta lógica AND junto con la entrada del sensor 3, la entrada del sensor 1 debe estar negada en una compuerta lógica AND junto con la entrada del sensor 2 y la salida de estas dos compuertas lógicas deben entrar a una compuerta lógica OR.

El circuito lógico queda de la siguiente manera:

<p align="center">
  <img src="https://github.com/PixelNote/Workshop-4/assets/81392047/935a8018-f8b5-4738-b3b4-fa8ad1ecb076" alt="Tabla de verdad" width="800"/>
</p>

A partir del circuito lógico, se hizo uso del software CODESYS para la simulación de un controlador lógico programable (PLC) del proceso de monitoreo del tanque con líquido químico. Para la programación del controlador PLC se hizo uso del lenguaje ladder. Este lenguaje permite la adaptación del circuito lógico [2] para realizar la simulación junto con su HMI.

El diagrama de escalera o diagrama ladder representa las entradas necesarias para adaptar el circuito de la siguiente manera:

<p align="center">
    <img src="https://github.com/PixelNote/Workshop-4/assets/81392047/6e7d164b-1f30-4383-8867-e859c2ac5c06" alt="Entradas ladder" width="350"/>
  <img src="https://github.com/PixelNote/Workshop-4/assets/81392047/648174d8-4509-4bb4-b34c-9e4f61ca62e6" alt="Salida ladder" width="100"/>
</p>
La primera entrada es un contacto normalmente abierto que representa una la entrada de un sensor, la segunda entrada es un contacto normalmente cerrado que representa una entrada negada de un sensor en el circuito lógico, y la salida representa las luces del sistema de monitoreo [2].

Adicionalmente, para representar las compuertas lógicas, se tienen las siguientes configuraciones [2]:

- Compuerta AND: Las entradas se deben posicionar en serie.
- Compuerta OR: Las entradas se deben posicionar en paralelo.

Teniendo en cuenta estos estándares junto con el circuito lógico, se crearon las siguientes variables de entrada y salida booleana para todos los los sensores y luces. Con las variables establecidad, se diseño el correspondiente diagrama ladder:

<p align="center">
  <img src="https://github.com/PixelNote/Workshop-4/assets/81392047/2a8023ec-1dd5-48a8-9b4c-9dbfdbac1b20" alt="Variables del sistema" width="195"/>
    <img src="https://github.com/PixelNote/Workshop-4/assets/81392047/cdf9ddad-cb95-40db-b744-d213363d4de8" alt="Diagrama ladder" width="600"/>
</p>

Para la interfaz gráfica, se uso un cilindro en el centro representando el tanque y sus diferentes niveles de llenado. Los sensores fueron representados con switches, cada uno con su etiqueta y puestos de forma ascendente. Las luces que representan los estados fueron representados con lamparas de diferente color según la gráfica de referencia. Sin embargo, se cambio el posicionamiento de las luces en la interfaz para tener una mayor comprensión de los estados del sistema. Entonces, el orden de las luces led de manera ascendente es igual a H4, H2, H1, H3 y H5.

El HMI quedó de la siguiente manera:

<p align="center">
  <img src="https://github.com/PixelNote/Workshop-4/assets/81392047/2a7af819-05bd-4cda-b6e2-8a8e250d1ce4" alt="HMI" width="600"/>
</p>

Ejecutando la simulación observamos la siguiente serie de comportamientos:

- Ningún sensor activo
  
<p align="center">
  <img src="https://github.com/PixelNote/Workshop-4/assets/81392047/8a856203-7584-4b87-84cc-63812dbae218" alt="HMI H4" width="375"/>
    <img src="https://github.com/PixelNote/Workshop-4/assets/81392047/484a3b32-c8fb-401e-abd4-7806981353c7" alt="Ladder H4" width="350"/>
</p>

- Un sensor activo

<p align="center">
  <img src="https://github.com/PixelNote/Workshop-4/assets/81392047/b112b118-822f-493f-84fe-f43a72d4acc4" alt="HMI H2" width="375"/>
  <img src="https://github.com/PixelNote/Workshop-4/assets/81392047/71e27215-6e76-4f97-9c01-7637c6f0dccd" alt="Ladder H2" width="350"/>
</p>

- Dos sensores activos

<p align="center">
  <img src="https://github.com/PixelNote/Workshop-4/assets/81392047/00239ef4-4b80-4e0e-8775-e5c29ff4db07" alt="HMI H1" width="375"/>
  <img src="https://github.com/PixelNote/Workshop-4/assets/81392047/495f8f34-919d-41e9-b471-6452d888490f" alt="Ladder H1" width="350"/>
</p>

- Tres sensores activos

<p align="center">
  <img src="https://github.com/PixelNote/Workshop-4/assets/81392047/d0d188a6-28ec-472c-8c36-902c2ef87f1d" alt="HMI H3" width="375"/>
  <img src="https://github.com/PixelNote/Workshop-4/assets/81392047/8aff6964-79fe-4a50-bba8-98667f60427c" alt="Ladder H3" width="350"/>
</p>

- Solo uno y sensor 3 activo (Error)

<p align="center">
  <img src="https://github.com/PixelNote/Workshop-4/assets/81392047/8524b012-ab11-42b9-8be5-80b3c5cef961" alt="HMI H3" width="375"/>
  <img src="https://github.com/PixelNote/Workshop-4/assets/81392047/a0b102f2-f476-448b-a4a5-19a7fbeadd0f" alt="Ladder H3" width="350"/>
</p>

Si comparamos estos comportamientos con la tabla de verdad, podemos observar que los cambios de salida de los respectivos sensores efectivamente corresponden a las diferentes salidas de las distintas luces que se tiene en el sistema. Por lo tanto, la solución simulada satisface el requerimento de la automatización del monitoreo del tanque de líquido químico.

Además de la validación del sistema por medio de la simulación del HMI realizada en CodeSys también se decidió realizar la representación del sistema en la vida real por medio de elementos reales. Para esto se utilizó el editor de OpenPLC con el propósito de luego implementar el código ladder en una placa de Arduino Uno.
<p align="center">
  <img src="https://github.com/PixelNote/Workshop-4/assets/97065279/4e7b3167-235a-481e-909d-b2f4b9d6a06c" alt="OpenPlc Ladder" width="375"/>
</p>
Como se puede observar en la anterior imagen, el código es exactamente el mismo al que se desarrolló en CodeSys, sin embargo antes de hacer la implementación física las variables a utilizar deben estar correctamente definidas, declarando como tipo BOOL para las entradas (B1, B2, B3) y para las salidas (H1, H2, H3, H3, H5), pero también se requiere establecer los pines que corresponden a la placa de Arduino Uno, tanto salidas y entradas digitales como salidas análogas en base a la guía obtenida en [3].

<p align="center">
  <img src="https://github.com/PixelNote/Workshop-4/assets/97065279/9cbb9b88-1afd-48a8-a7d5-c07d30dbff54" alt="OpenPlc Ladder" width="590"/>
</p>

La ubicación mencionada en la tabla anterior representa a los pines de la placa Arduino Uno, pero un problema que se encontró fue que no se tenían las suficientes salidas digitales para representar los cinco estados del sistema, afortunadamente el programa de OpenPLC Editor nos otorga la posibilidad de configurar de que manera se quieren utilizar los pines del Arduino al que se va a implementar el código, por esa razón antes de implementar el código se realizó la configuración necesaria para utilizar una salida análoga como salida digital y así encender un led por cada estado:

<p align="center">
  <img src="https://github.com/PixelNote/Workshop-4/assets/97065279/39d9ac6c-e95b-4a22-98c1-17983d1ae0a6" alt="OpenPlc Ladder" width="590"/>
</p>

Ya con todo preparado se procedió a transferir el código a la placa Arduino para empezar el montaje físico del sistema, el cual está formado de tres switches que simulan el nivel de líquido del tanque, los cuales funcionan de manera secuencial, impidiendo que se pueda activar uno sin activar el o los anteriores, lo cual es considerado un error en el sistema. Además de interruptores se instalan cinco leds de diferentes colores para representar los cinco estados, dos de ellos de color rojo representando el tanque vacío y el error, dos de color amarillo representando un nivel bajo y un nivel demasiado alto de agua, y por último un led verde representando un llenado correcto.

- Estado de tanque vacío:
<p align="center">
  <img src="https://github.com/PixelNote/Workshop-4/assets/97065279/6b9b26a4-9856-4ed8-9651-db58472e9451" alt="OpenPlc Ladder" width="590"/>
</p>

- Estado de tanque muy bajo:
<p align="center">
  <img src="https://github.com/PixelNote/Workshop-4/assets/97065279/394985f5-0011-4ff6-86b9-61a0e711d42c" alt="OpenPlc Ladder" width="590"/>
</p>

- Estado de tanque correcto:
<p align="center">
  <img src="https://github.com/PixelNote/Workshop-4/assets/97065279/8c52be45-e714-4c46-a1e0-0ad7174173c8" alt="OpenPlc Ladder" width="590"/>
</p>

- Estado de tanque demasiado lleno:
<p align="center">
  <img src="https://github.com/PixelNote/Workshop-4/assets/97065279/fff9cdfd-757f-4f31-a6f4-cad66febdb19" alt="OpenPlc Ladder" width="590"/>
</p>

- Estado de error de tanque:
<p align="center">
  <img src="https://github.com/PixelNote/Workshop-4/assets/97065279/131b574c-f49d-4268-b26d-0dd683ae2eb2" alt="OpenPlc Ladder" width="590"/>
</p>

Gracias a la validación por medio de una HMI en CodeSys además de la implementación del sistema con elementos físicos del sistema de monitoreo del tanque se pudo confirmar la posibilidad de un sistema de este estilo por medio de la programación ladder y conocimientos previos de electrónica, ambos conocimientos fueron correctamente adoptados por el equipo de trabajo y como se evidenció, puestos en práctica de la mejor manera logrando una integración entre el software y el hardware realizado con elementos de prototipado rápido, además este proyecto ha abierto posibilidades de proyectos que pueden ser implementados con mayor complejidad y exactitud para desarrollar un sistema que requiera el monitoreo de algún tanque de líquidos.

## Referencias 

[1] http://www.32x8.com/index.html

[2] https://bookdown.org/alberto_brunete/intro_automatica/diagrama-de-escalera-kop.html
[3] https://autonomylogic.com/docs/2-4-physical-addressing/






