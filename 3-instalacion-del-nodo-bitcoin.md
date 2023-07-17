Instalación del Nodo Bitcoin
===

Ya que podemos acceder a nuestra instancia EC2 desde terminal, vamos a buscar los binarios del nodo de bitcoin para instalarlo.

Buscamos en internet Bitcoin core y abrimos la [página oficial](https://bitcoincore.org/) seleccionamos [Download Bitcoin Core](https://bitcoincore.org/en/download/), en esa sección podremos encontrar diferentes versiones del core para distintos sistemas operativos, en este caso la opción adecuada es ==Linux (tgz)==.

> ⚠️ Nunca descargues binarios de lugares no oficiales y no guardes versiones en tu computadora para usar después o compartir, siempre descarga la última versión directo de la fuente.

Con clic derecho y "Copy link address", copia la url de descarga de Bitcoin Core y utiliza el comando wget para descargarlo en la instancia EC2 como se muestra a continuación:

```bash
$ wget https://bitcoincore.org/bin/bitcoin-core-25.0/bitcoin-25.0-x86_64-linux-gnu.tar.gz
```

> :warning: No ocupes la url que yo estoy proporcionando, consíguela tú mismo y sustituye aunque sea la misma.

Comenzará a descargar y tomará un tiempo según la conexión, espera a que termine.

## Validar el binario que se descargó (opcional)

Descargamos los siguientes archivos:

```bash
$ wget https://bitcoincore.org/bin/bitcoin-core-25.0/SHA256SUMS

$ wget https://bitcoincore.org/bin/bitcoin-core-25.0/SHA256SUMS.asc

$ sha256sum --ignore-missing --check SHA256SUMS
```

Verificar el archivo de descarga es opcional pero altamente recomendado para asegurarte de que la información descargada no esté corrupta.

El proceso realiza una validación verificando que la firma de los desarrolladores de bitcoin core estén presentes en el release.

Para iniciar el proceso, primero hay que descargar las [llaves de los desarrolladores](https://github.com/bitcoin-core/guix.sigs). Descarguemos el repositorio con las llaves para tenerlas todas:


```bash
$ git clone https://github.com/bitcoin-core/guix.sigs
```

Ahora vamos a importar las llaves a gpg para validarlas:

```bash
$ gpg --import guix.sigs/builder-keys/*
```

Este proceso cargará las firmas de los desarrolladores actuales de bitcoin core de manera local con gpg.

Ahora vamos a verificar que el binario esté correcto:

```bash
$ gpg --verify SHA256SUMS.asc
```

Debería mostrar mensajes como `gpg: Good signature` y otra línea con algo parecido a esto `Primary key fingerprint: E777 299F C265 DD04 7930  70EB 944D 35F9 AC3D B76A`.

La información desplegada podrá traer mensajes como  `key is not certified with a trusted signature`. 

Si en un futuro actualizas los binarios, deberás actualizar las llaves de los desarrolladores antes de validar el archivo con el siguiente comando:

```bash
$ gpg --refresh-keys
```

Con eso terminamos de verificar que los binarios descargados están firmados por los _developers_ de bitcoin core.

## Instalación de Nodo Bitcoin

Ahora para descomprimir el archivo utilizamos el siguiente comando ==Recuerda sustituir el nombre del archivo en los siguientes comandos== 
```bash
$ tar -xvf bitcoin-25.0-x86_64-linux-gnu.tar.gz
```

Ya podemos eliminar el zip con el siguiente comando:

```bash
$ rm -rf bitcoin-25.0-x86_64-linux-gnu.tar.gz
```

Vamos a modificar el archivo `.bashrc` para agregar unos alias y poder correr fácilmente el nodo y el cli de bitcoin:

```bash
$ nano $HOME/.bashrc
```
Y al final del archivo agregaremos las siguientes líneas:

```bash
# Bitcoin Alias
alias bitcoind="$HOME/nodes/bitcoin/bitcoin-25.0/bin/bitcoind"
alias bitcoin-cli="$HOME/nodes/bitcoin/bitcoin-25.0/bin/bitcoin-cli"
```

Cambiemos de ubicación al `$HOME` utilizando el comando change directory:

```bash
$ cd $HOME
```

Para hacer efectivas nuestra configuración correremos el siguiente comando:

```bash
$ . .bashrc
```

Vamos a crear el directorio .bitcoin con permisos 755 donde se guardará toda la información del nodo.

```bash
$ mkdir -m 755 .bitcoin
```
Vamos a crear el archivo bitcoin.conf para configurar las opciones de nuestro nodo

```bash
$ nano .bitcoin/bitcoin.conf
```

Y agrega la siguiente información al archivo y cambia CUSTOM_RCP_USER y CUSTOM_RCP_PASS por información segura ya que serán lo accesos de comunicación entre en nodo de Bitcoin y LND:

```bash
# Accept command line and JSON-RPC commands
server=1
# Enable publish raw block in <address>
zmqpubrawblock=tcp://127.0.0.1:28332
# Enable publish raw transaction in <address>
zmqpubrawtx=tcp://127.0.0.1:28333
# Username for JSON-RPC connections
rpcuser=CUSTOM_RCP_USER
# Password for JSON-RPC connections
rpcpassword=CUSTOM_RCP_PASS
# Allow JSON-RPC connections from, by default only localhost are allowed
rpcallowip=127.0.0.1
# Maintain a full transaction index, used by the getrawtransaction rpc call (default: 0)
txindex=1
```

Donde `CUSTOM_RCP_USER` y `CUSTOM_RCP_PASS` son un usuario y contraseña con que le te podrás conectar al nodo y deben ser personalizados, recuerda cambiarlos por algo seguro.

Actualiza los permisos del archiuvo bitcoin.conf a 400 para que sea más seguro. Al hacer esto el archivo únicamente se podrá modificar con sudo y el servidor será el único capaz de leerlo.

```bash
$ chmod 400 .bitcoin/bitcoin.conf
```

Ahora vamos a probar el comando `bitcoind --help`

Debería arrojarte toda la lista de comandos disponibles de `bitcoind`.

Ya que corroboramos que nuestro binario y el alias funcionan, vamos a comenzar a descargar el blockchain lo cual puede tomar bastante tiempo, así que lo correremos como `-daemon` para que podamos desconectarnos si es necesario.

```bash
$ bitcoind -testnet -daemon
```

Para validar cómo van los procesos del nodo crearemos un `symlink` de los logs en un lugar donde podamos encontrarlo fácilmente.

```bash
$ ln -s ~/.bitcoin/testnet3/debug.log ~/bitcoind-testnet.log
```

Y con el siguiente comando podremos observar el progreso del nodo:

```bash
$ tail -f bitcoind-testnet.log
```

Hasta este punto, sólo hay que esperar para que el nodo sincronice el blockchain y ya tendremos un nodo funcional de Bitcoin.

Sólo que si por alguna razón se apaga o reinicia la instancia de AWS el nodo se apagará, por lo que vamos a agregar un cron que realice un reboot del nodo.

Abre crontab y selecciona el editor de tu preferencia, nosotros elegimos `nano` opción 1:

```bash
$ crontab -e

no crontab for ubuntu - using an empty one

Select an editor.  To change later, run 'select-editor'.
1. /bin/nano        <---- easiest
2. /usr/bin/vim.basic
3. /usr/bin/vim.tiny
4. /bin/ed

Choose 1-4 [1]: 1
```

Y agrega la siguiente información al final del archivo:

```bash
# Start Bitcoin Core on boot
@reboot bitcoind -testnet -daemon
```

Si deseas detener el nodo por cualquier razón, corre el siguiente comando y espera hasta que responda `Bitcoin Core stopping` para detenerlo de forma segura:

```bash
$ bitcoin-cli -testnet stop
Bitcoin Core stopping
```
---

Con esto concluimos la Instalación del Nodo Bitcoin. En el siguiente capítulo realizaremos la [Instalación del Nodo Lightning Network LND](/4-instalacion-del-nodo-lightning-network.md).