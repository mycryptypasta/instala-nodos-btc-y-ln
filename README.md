# Instala tu nodo Bitcoin y Lightning Network

## Acerca de

Guía en español, práctica y fácil para instalar un nodo de Bitcoin y Lightning Network.

Contenido

1. [Introducción Bitcoin y Lightning Network](/1-introduccion-bitcoin-y-lightning-network.md)
2. [Creando una instancia Ubunto en AWS EC2](/2-creando-una-instancia-ubunto-en-aws-ec2.md)
3. [Instalación del Nodo Bitcoin](/3-instalacion-del-nodo-bitcoin.md)
4. [Instalación del Nodo Lightning Network](/4-instalacion-del-nodo-lightning-network.md)
5. [Fondeo y pago de factura con tu nodo](#fund)
6. [Conclusiones](#fund)
7. [Nodo e-clair](#node-e-clair)
8. [Nodo c-lightning](#node-c-lightning)

Puedes realizar la instalación en cualquier tipo de servidor, incluso de manera local, aunque para mantener un estandar y que cualquier persona que lea este tutorial pueda reproducir los pasos utilizaremos el servicio [EC2 de AWS](https://aws.amazon.com/es/ec2/) (Amazon Web Services) en su versión gratuita, por lo que no será necesario invertir dinero para aprender.

## Objetivo

Al finalizar este tutorial habrás **instalado en Testnet un nodo de Bitcoin y Lightning Network**, ambos podrán comunicarse entre sí y tendrás una wallet en cada nodo. También realizaremos un ejercicio completo fondeando nuestro nodo, nos conectaremos con un *peer*, crearemos un canal y finalizaremos realizando una compra para poner a prueba nuestros conocimientos y el nodo recién instalado.