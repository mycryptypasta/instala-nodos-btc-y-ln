# Instala tu nodo Bitcoin y Lightning Network

## Acerca de

Guía en español, práctica y fácil para instalar un nodo de Bitcoin y Lightning Network en AWS EC2.

Tabla de contenidos:

1. [Introducción Bitcoin y Lightning Network](/1-introduccion-bitcoin-y-lightning-network.md)
2. [Creando una instancia Ubuntu en AWS EC2](/2-creando-una-instancia-ubuntu-en-aws-ec2.md)
3. [Instalación del Nodo Bitcoin](/3-instalacion-del-nodo-bitcoin.md)
4. [Instalación del Nodo Lightning Network LND](/4-instalacion-del-nodo-lightning-network.md)
5. [Reboot automático de Nodo Bitcoin y LND](/5-reboot-de-nodos.md)
<!-- 6. [Fondeo y pago de factura con tu nodo LND](#fund) -->
<!-- 7. [Conclusiones](#fund) -->

Puedes realizar la instalación en cualquier tipo de nube o servidor, incluso de manera local, aunque para mantener un estandar y que cualquier persona que lea este tutorial pueda reproducir los pasos utilizaremos el servicio [EC2 de AWS](https://aws.amazon.com/es/ec2/) (Amazon Web Services)

> Considera que AWS EC2 es un servicio de pago y la versión con las características mínimas 2vCPU y 4 GiB RAM tiene un costo aproximado de $30 USD/mes

## Objetivo

Al finalizar este tutorial habrás **instalado en Testnet un nodo de Bitcoin y Lightning Network**, ambos podrán comunicarse entre sí y tendrás una wallet.

También realizaremos un ejercicio completo fondeando nuestro nodo LND, nos conectaremos con un *peer*, crearemos un canal y finalizaremos realizando una compra para poner a prueba nuestros conocimientos y el nodo recién instalado.

## Autor
Me puedes llamar Carlos o WaZzo (Gauzu o Guazo). Escíbeme por Twitter para dudas, comentarios y/o sugerencias: [@xwazzo](https://twitter.com/xWazzo)