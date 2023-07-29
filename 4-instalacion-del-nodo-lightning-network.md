Instalación del Nodo Lightning Network
===

## Descarga de Binarios

Ahora vamos a instalar nuestro nodo de LND. Para empezar, busca la versión más reciente de LND directo del [repositorio oficial](https://github.com/lightningnetwork/lnd/releases). Y de ahí seleccionar el [último release](https://github.com/lightningnetwork/lnd/releases/latest) también llamado `latest`.

> Al momento de escribir este tutorial la versión actual es `v0.16.4-beta` pero considera que esta va a cambiar constantemente y **no recomendamos** copiar y pegar los comandos a continuación, siempre es mejor buscar las últimas versiones y utilizar estas.

Antes de descargar el nodo, vamos a crear su carpeta en el servidor:

```bash
$ mkdir -m 755 -p $HOME/nodes/lnd
```

Y nos movemos a esa ubicación en el servidor:

```bash
cd $HOME/nodes/lnd
```

Ahora tenemos que buscar y copiar la URL de la versión `lnd-linux-amd64-`* para nuestra instancia de Ubuntu. Copiamos el link y lo descargamos en nuestro servidor:

```bash
$ wget https://github.com/lightningnetwork/lnd/releases/download/v0.16.4-beta/lnd-linux-amd64-v0.16.4-beta.tar.gz
```

## Verificar el *release* que se descargó (opcional)

Para validar el binario, vamos a necesitar descargar dos arhivos adicionales `manifest-*.txt` y  `manifest-roasbeef-*.sig`. Considera que dependiendo de la versión que exista actualmente la URL puede cambiar por lo que sugerimos

```bash
$ wget https://github.com/lightningnetwork/lnd/releases/download/v0.16.4-beta/manifest-v0.16.4-beta.txt
```

```bash
$ wget https://github.com/lightningnetwork/lnd/releases/download/v0.16.4-beta/manifest-roasbeef-v0.16.4-beta.sig
```

```bash
gpg --verify manifest-roasbeef-v0.16.4-beta.sig manifest-v0.16.4-beta.txt
```

Debería mostrar algo como lo siguiente:
```bash
gpg: Signature made Thu Jul  6 00:29:05 2023 UTC
gpg:                using RSA key 60A1FA7DA5BFF08BDCBBE7903BBD59E99B280306
gpg: Good signature from "Olaoluwa Osuntokun <laolu32@gmail.com>" [unknown]
gpg: WARNING: This key is not certified with a trusted signature!
gpg:          There is no indication that the signature belongs to the owner.
Primary key fingerprint: E4D8 5299 674B 2D31 FAA1  892E 372C BD76 33C6 1696
     Subkey fingerprint: 60A1 FA7D A5BF F08B DCBB  E790 3BBD 59E9 9B28 0306
```

## Instalación de nodo LND

Ahora vamos a descomprimir el archivo descargado utilizando el comando `tar`

```bash
$ tar -xvf lnd-linux-amd64-v0.16.4-beta.tar.gz
```

Recuerda eliminar el `tar.gz` para no dejar basura:

```bash
$ rm -rf lnd-linux-amd64-v0.16.4-beta.tar.gz
```

Ahora vamos a regresar a la carpeta $HOME para terminar la instalación de nuestro nodo LND y crear los alias de los comandos.

```bash
$ cd $HOME
```

Ahora abrimos el archivo .bashrc que se encuentra en $HOME

```bash
$ nano $HOME/.bashrc
```

Y agregamos un alias con la ruta a los binarios que descargamos:

```bash
# LND Alias
alias lnd="$HOME/nodes/lnd/lnd-linux-amd64-v0.16.4-beta/lnd"
alias lncli="$HOME/nodes/lnd/lnd-linux-amd64-v0.16.4-beta/lncli"
```

Y corremos el siguiente comando para efectuar los cambios:

> Recuerda que el comando sólo surtirá efecto si te encuentras en la misma carpeta que el archivo .bashrc

```bash
$ . .bashrc
```

Ahora vamos a crear la carpeta de configuración para el nodo LND con permisos 755

```bash
$ mkdir -m 755 .lnd
```

Y adentro creamos el archivo de configuración:

```bash
$ nano .lnd/lnd.conf
```

Adentro del archivo escribiremos lo siguiente:

```bash
bitcoin.active=true
bitcoin.testnet=true
bitcoin.node=bitcoind 
bitcoind.rpcuser=CUSTOM_RCP_USER 
bitcoind.rpcpass=CUSTOM_RCP_PASS 
bitcoind.zmqpubrawblock=tcp://127.0.0.1:28332 
bitcoind.zmqpubrawtx=tcp://127.0.0.1:28333
```

Donde `CUSTOM_RCP_USER` y `CUSTOM_RCP_PASS` son el usuario y contraseña que creaste cuando instalaste el nodo de Bitcoin.

Ahora podemos correr nuestro nodo utilizando el siguiente comando:

```bash
$ lnd
```

Al correrlo la última línea que veas debería mostrar algo como lo siguiente:

```bash
[INF] LTND: Waiting for wallet encryption password. Use `lncli create` to create a wallet, `lncli unlock` to unlock an existing wallet, or `lncli changepassword` to change the password of an existing wallet and unlock it.
```

Que nos indica que es necesario crear una nueva wallet o utilizar una ya existente. En este caso vamos a asumir que no tenemos wallet y es necesario crearla.

> Puedes cancelar el comando anterior utilizando el comando `ctrl + c` para seguir avanzando, pero **es necesario dejar el nodo activo** para crear nuestra wallet.

Dejemos corriendo nuestro nodo en la terminar que tenemos abierta y abramos una nueva conexión en otra terminal para continuar con el proceso.

> Antes de crear nuestra wallet, te recomiendo crear una contraseña fuerte, lo puedes hacer con un generador de contraseñas o utilizando el siguiente comando que creará una contraseña random de 21 caracteres en hexadecimal (o base64 cambiando `-hex` por `-base64`) y lo guardará en un archivo en el servidor para poder iniciar nuestro nodo automáticamente.

```bash
$ openssl rand -hex 21 > ~/.lnd/wallet_password
```

Y para proteger el archivo, cambiemos sus permisos a 400 con el siguiente comando:

```bash
$ chmod 400 ~/.lnd/wallet_password
```

Toma nota del password porque lo ocuparemos en el siguiente paso para crear nuestra wallet.

Para crear nuestra wallet corre el siguiente comand:

```bash
$ lncli create
```

Sigue los pasos que se te indican
```bash
Input wallet password: 
Confirm password: 
```

Para crear una nueva seed introduce la opción `n`:

```bash
Do you have an existing cipher seed mnemonic or extended master root key you want to use?
Enter 'y' to use an existing cipher seed mnemonic, 'x' to use an extended master root key 
or 'n' to create a new seed (Enter y/x/n): n
```

En el siguiente paso podrás encriptar tu cipher seed utilizando un passphrase o puedes sólo presionar enter para continuar

```bash
Your cipher seed can optionally be encrypted.
Input your passphrase if you wish to encrypt it (or press enter to proceed without a cipher seed passphrase): 
```

#### Error: no es posible crear una wallet

Si te aparece el siguiente error es porque apagaste o se apagó tu nodo lnd y no es posible crear tu wallet por el momento:

```bash
[lncli] unable to generate seed: rpc error: code = Unavailable desc = connection error: desc = "transport: Error while dialing dial tcp 127.0.0.1:10009: connect: connection refused"
```

#### Success: Haz creado tu wallet

Si todo funcionó correctamente verás algo parecido que **deberás guardar de manera segura** ya que es tu seed para restaurar tu wallet.

```bash
Generating fresh cipher seed...

!!!YOU MUST WRITE DOWN THIS SEED TO BE ABLE TO RESTORE THE WALLET!!!

---------------BEGIN LND CIPHER SEED---------------
 1. abandon   2. odor      3. diamond   4. analyst 
 5. connect   6. jar       7. exile     8. satisfy 
 9. asthma   10. pipe     11. view     12. abstract
13. design   14. permit   15. fiction  16. solar   
17. agent    18. net      19. loyal    20. average 
21. learn    22. dolphin  23. insect   24. afraid  
---------------END LND CIPHER SEED-----------------

!!!YOU MUST WRITE DOWN THIS SEED TO BE ABLE TO RESTORE THE WALLET!!!
```

Ahora vamos a detener un momento nuestro nodo LND simplemente deteniendo el proceso con `ctrl + c` si es que sigue corriendo, y vamos a agregar el password de nuestra wallet al archivo de configuración para que se pueda iniciar automáticamente.

Abre el archivo de configuración para editarlo

```bash
$ nano .lnd/lnd.conf
```

Y ahora vamos a agregar al final del archivo la siguiente línea

```bash
wallet-unlock-password-file=~/.lnd/wallet_password
```

Y para poder observar lo que pasa en nuestro nodo, vamos a crear un symlink al log para tenerlo a la mano.

```bash
$ ln -s ~/.lnd/logs/bitcoin/testnet/lnd.log ~/lnd-testnet.log
```

Y con el siguiente comando podremos observar el proceso del nodo:

```bash
$ tail -f lnd-testnet.log
```

> Recuerda que los archivos .log cambian de ubicación si utilizas `testnet` o `mainnet`.

Ahora si corremos el comando `lnd` correrá nuestro nodo automáticamente ya que tenemos una wallet y el nodo de bitcoin corriendo. 

Es posible que consigas un error como el siguiente si el nodo de `bitcoin` se encuentra apagado:

```bash
2023-07-17 03:59:56.947 [ERR] LTND: unable to create partial chain control: invalid http POST response (nil), method: getblockhash, id: 1, last error=Post "http://localhost:18332": dial tcp 127.0.0.1:18332: connect: connection refused
```

Sólo tienes que iniciar nuevamente el nodo de bitcoin y cuando corras el nodo `lnd` debe funcionar correctamente.

Podemos verificar que nuestro nodo `lnd` está corriendo correctamente solicitando su información utilizando `lncli`

```bash
$ lncli --network=testnet getinfo
```

Deberás ver algo como la siguiente información:

```bash
{
    "version": "0.16.4-beta commit=v0.16.4-beta",
    "commit_hash": "6bd30047c1b1188029e8af6ee8a135cf86e7dc4b",
    "identity_pubkey": "027c2c4c6f08e7a93e694786fd17b3db7719985c9d65f6be38f2d081ed0209e5b4",
    "alias": "027c2c4c6f08e7a93e69",
    "color": "#3399ff",
    "num_pending_channels": 0,
    "num_active_channels": 0,
    "num_inactive_channels": 0,
    "num_peers": 2,
    "block_height": 2442286,
    "block_hash": "0000000000000015008f3181d704b3f862a287fb94bc90bbf244a7b5adf9ca5c",
    "best_header_timestamp": "1689566771",
    "synced_to_chain": true,
    "synced_to_graph": false,
    "testnet": true,
    "chains": [
        {
            "chain": "bitcoin",
            "network": "testnet"
        }
    ],
    "uris": [
    ],
    "features": { … }
}
```

El único problema hasta aquí es que para este momento tenemos cuando menos 2 terminales abiertas, una con el nodo de `lnd` y otra para operarlo con `lncli`. En la siguiente sección aprenderemos a hacer que inicien ambos nodos de manera automática sin necesidad de nuestra terminal.

---

Con esto concluimos la Instalación del Nodo Lightning Network LND. En el siguiente capítulo aprenderemos a crear el [Reboot automático de Nodo Bitcoin y LND](/5-reboot-de-nodos.md).

---

Tabla de contenidos:

1. [Introducción Bitcoin y Lightning Network](/1-introduccion-bitcoin-y-lightning-network.md)
2. [Creando una instancia Ubuntu en AWS EC2](/2-creando-una-instancia-ubuntu-en-aws-ec2.md)
3. [Instalación del Nodo Bitcoin](/3-instalacion-del-nodo-bitcoin.md)
4. Instalación del Nodo Lightning Network LND
5. [Reboot automático de Nodo Bitcoin y LND](/5-reboot-de-nodos.md)