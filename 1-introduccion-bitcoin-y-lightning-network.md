Introducción Bitcoin y Lightning Network
===

Si llegaste a este tutorial, es probable que conozcas las características técnicas de Bitcoin y Lightning Network, sin embargo, nuestra filosofía para enseñar es facilitar los conceptos básicos para que cualquier persona que desee aprender pueda hacerlo sin necesidad de tener un desarrollo como programador; así que vamos tocar rápidamente los puntos más importantes para entender la importancia de ambas tecnologías antes de comenzar la instalación.

Si deseas pasar directo a la parte práctica puedes ir directo al capítulo [Creando una instancia Ubunto en AWS EC2](/2-creando-una-instancia-ubunto-en-aws-ec2.md).

## Resumen de Bitcoin

Bitcoin es un "Sistema de dinero electrónico de persona a persona" según lo define su *[whitepaper](https://bitcoin.org/bitcoin.pdf)* escrito por Satoshi Nakamoto. Aunque es una definición bastante concreta y se explica a detalle en el documento, sigue siendo algo complicado de entender para cualquier persona.

La mejor forma que he encontrado para resumir y explicarlo es la siguiente:

> Bitcoin es una red de pagos. Así como Master Card, Visa, American Express, Switf, etc.

Quizás la definición no le de toda la gloria que merece la tecnología, pero sí es una manera rápida para que cualquier persona con educación financiera básica pueda comprenderlo.

Es una red con capacidad de intercambiar valor entre personas de manera rápida, segura y a nivel global.

La gran diferencia que tiene contra cualquier otra red de pagos tradicional es que **Bitcoin no le pertenece a nadie**, por eso decimos que es una tecnología **descentralizada** ya que **no es necesario un tercero de confianza** como un banco o una empresa privada para validar las transacciones.

Para lograrlo, Satoshi Nakamoto propuso la idea de crear un registro de transacciones en cadena llamado **blockchain**, que en términos simples es un gran libro contable de todos los intercambios realizados. 

Para iniciar este el registro de los intercambios, se estableció un **algoritmo de minado** que se encarga de crear unidades de la moneda virtual que se intercambiará y la cual tiene un límite de 21 millones de bitcoins.

A las computadoras encargadas de procesar este algoritmo de minado se les conoce como **mineros** y son ellos también los que mantienen segura la red, validando cada una de las operaciones. Para realizar todas estas validaciones se utiliza un **mecanismo de consenso** llamado *proof-of-work* (prueba de trabajo) y el cual consiste en solicitar al 51% de los participantes que validen cada una de las transacciones que se unirán al *blockchain*.

Es decir, si tenemos un total de 100 mineros, necesitaremos que 51 de ellos validen individualmente cada transacción, de esta manera si llega un participante deshonesto intentando crear transacciones fraudulentas, mientras el 51% sea honesto se mantendrá seguro y confiable.

Y sí… si el 51% es deshonesto podrían realizar un ataque. A ese concepto se le conoce como el **ataque del 51%** y conforme sigue creciendo Bitcoin, los costos y dificultad del ataque siguen creciendo, haciendo que no sea rentable realizar el ataque. Según [crypto51](https://www.crypto51.app/), una hora de ataque costaría $805,101 USD por hora para el momento en que escribo esto siendo 19 de octubre de 2022.

Esas computadoras que participan en el *blockchain* pueden correr o no el algoritmo de minado, por lo que si minan las llamaremos **mineros** y si sólo participan serán **nodos**.

Así que al colocar un nodo de Bitcoin también estarás ayudando a crear un nodo honesto y fortaleciendo la seguridad. ¡Hurra! :D

Si deseas profundizar en la parte técnica de cómo funciona esta tecnología te recomiendo el libro digital [Mastering Bitcoin](https://github.com/bitcoinbook/bitcoinbook) (Actualmente sólo en inlgés)

## Resumen de Lightning Network

Bitcoin parece una tecnología increíble y todo, pero también tiene algunos retos que estamos intentando resolver para que pueda utilizarse realmente de manera global. 

Por ejemplo… al ser una red descentralizada y necesitar que el 51% del poder de minado valide cada transacción, hace que conforme crecen los participantes, también se incremente el tiempo que tarda en validar cada transacción.

Actualmente bitcoin puede procesar 7 transacciones por segundo (TPS), lo cual es muy inferior a las 5,000 TPS de MasterCard o las 24,000 TPS de Visa.

Otro detalle es que al ser público el blockchain, también lo son todas las transacciones en él y por lo tanto representa un riesgo a la privacidad de cada persona.

Para solucionar estas y otras situaciones, se propuso crear tecnologías de segunda capa (*Layer 2 tecnology*) como Lightning Network, que mantengan los principios de Bitcoin mientras incrementan la cantidad de transacciones por segundo y puedan procesar transacciones más privadas.

Lightning Network es una red de nodos que se comunican entre ellos creando **canales de pago** y procesando pagos de manera segura

Actualmente Lightning Network sigue siendo una tecnología en fase Beta lo que significa que aún tiene muchos retos por superar, pero es una solución innovadora que promete resolver los grandes problemas de Bitcoin para acercar la tecnología a cualquier persona de manera global.

Así que al crear tu propio nodo de Lightning Network además de contribuir al crecimiento de la red, estás siendo parte de los pioneros en esta tecnología para crear un red de pagos que revolucionará al mundo financiero.

Si deseas profundizar en la parte técnica de cómo funciona esta tecnología te recomiendo el libro digital [Mastering the Lightning Network](https://github.com/lnbook/lnbook) (Actualmente sólo en inlgés)

---

Con esto concluimos la Introducción. En el siguiente capítulo veremos paso a paso cómo [Crear una instancia Ubuntu en AWS EC2](/2-creando-una-instancia-ubuntu-en-aws-ec2.md).

---

Tabla de contenidos:

1. Introducción Bitcoin y Lightning Network
2. [Creando una instancia Ubuntu en AWS EC2](/2-creando-una-instancia-ubuntu-en-aws-ec2.md)
3. [Instalación del Nodo Bitcoin](/3-instalacion-del-nodo-bitcoin.md)
4. [Instalación del Nodo Lightning Network LND](/4-instalacion-del-nodo-lightning-network.md)
5. [Reboot automático de Nodo Bitcoin y LND](/5-reboot-de-nodos.md)