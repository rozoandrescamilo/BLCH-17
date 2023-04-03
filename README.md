[![1](https://github.com/rozoandrescamilo/Smart-Contract-para-consultar-estados-de-una-Purchase-Order/blob/main/img/1.jpg?raw=true "1")](https://github.com/Smart-Contract-para-consultar-estados-de-una-Purchase-Order/blob/main/img/1.jpg?raw=true "1")

# Documentación de despliegue de smart contract DoTrackStatusValidator con LacChain

- [Despliege de nodo local con LacChain](#despliege-de-nodo-local-con-lacchain)
  - [Configuración de nodo](#configuración-de-nodo)
    - [Iniciar nodo](#iniciar-nodo) 
    - [Comprobando conexión](#comprobando-conexión)
- [Implementar contratos inteligentes](#implementar-contratos-inteligentes)
  - [Ejecutar el componente de retransmisor](#ejecutar-el-componente-de-retransmisor)
  - [Truffle como entorno de desarrollo](#truffle-como-entorno-de-desarrollo)
    - [Compilación de contratos](#compilación-de-contratos)
    - [Compilación de contratos](#compilación-de-contratos)
- [Consideraciones para red principal y Firefly](#consideraciones-para-red-principal-y-firefly)



# Despliege de nodo local con LacChain

## Configuración de nodo

### Iniciar nodo

Una vez que el nodo esté listo, se puede iniciar con este comando en máquina remota:

`$ service pantheon start`

Verificar el estado de pantheon:

`$ service pantheon status`

[![11](https://github.com/rozoandrescamilo/Despliege-de-nodo-local-con-Lacchain/blob/main/img/11.png?raw=true "11")](https://github.com/Despliege-de-nodo-local-con-Lacchain/blob/main/img/11.png?raw=true "11")

Si se necesita reiniciar los servicios, se puede ejecutar los siguientes comandos:

`$ service pantheon restart`

### Comprobando conexión

Para verificar si el nodo está conectado a la red correctamente. Verifique que el nodo haya establecido las conexiones con los pares:

`$ sudo -i`

`$ curl -X POST --data '{"jsonrpc":"2.0","method":"net_peerCount","params":[],"id":1}' localhost:4545`


[![12](https://github.com/rozoandrescamilo/Despliege-de-nodo-local-con-Lacchain/blob/main/img/12.png?raw=true "12")](https://github.com/Despliege-de-nodo-local-con-Lacchain/blob/main/img/12.png?raw=true "12")

Ahora puede verificar si el nodo está sincronizando bloques obteniendo el registro de los últimos 100 bloques:

`$ tail -100 /root/lacchain/logs/pantheon_info.log`

[![13](https://github.com/rozoandrescamilo/Despliege-de-nodo-local-con-Lacchain/blob/main/img/13.png?raw=true "13")](https://github.com/Despliege-de-nodo-local-con-Lacchain/blob/main/img/13.png?raw=true "13")


# Implementar contratos inteligentes

Para este proceso se tiene en cuenta la Guía oficial de LACNet para Implementar smarts contracts con el nodo escritor desplegado: 
[https://lacnet.lacchain.net/smart-contracts-overview/](https://lacnet.lacchain.net/smart-contracts-overview/ "https://lacnet.lacchain.net/smart-contracts-overview/")

## Ejecutar el componente de retransmisor

Se inicializa en entorno de trabajo con el siguiente comando y se debe tener configurada la variable WRITER_KEY.

En caso de que no se tenga la variable WRITER_KEY en el entorno, configure esta variable con el contenido del archivo/lacchain/data/key

`$ env`

`$ export WRITER_KEY=PRIVATE_KEY  //where PRIVATE_KEY is content of /lacchain/data/key`

[![16](https://github.com/rozoandrescamilo/Despliege-de-nodo-local-con-Lacchain/blob/main/img/16.png?raw=true "16")](https://github.com/Despliege-de-nodo-local-con-Lacchain/blob/main/img/16.png?raw=true "16")

Ingresar a la consola del nodo y ejecutar los siguientes comandos:

`cd /root/lacchain/gas-relay-signer`

`systemctl import-environment WRITER_KEY`

`service relaysigner start`

[![17](https://github.com/rozoandrescamilo/Despliege-de-nodo-local-con-Lacchain/blob/main/img/17.png?raw=true "17")](https://github.com/Despliege-de-nodo-local-con-Lacchain/blob/main/img/17.png?raw=true "17")

Verificar que el retransmisor funcione bien:

`cd /root/lacchain/gas-relay-signer/log`

`tail -100 idbServiceLog.log`

Las primeras líneas deberían ser algo como esto:

[![18](https://github.com/rozoandrescamilo/Despliege-de-nodo-local-con-Lacchain/blob/main/img/18.png?raw=true "18")](https://github.com/Despliege-de-nodo-local-con-Lacchain/blob/main/img/18.png?raw=true "18")

Eso significa que el retransmisor está configurado y funciona bien.

## Truffle como entorno de desarrollo

Truffle es un entorno de desarrollo donde puede desarrollar fácilmente contratos inteligentes con su marco de prueba incorporado, compilación e implementación de contratos inteligentes, consola interactiva y muchas más funciones. Hemos creado un proveedor personalizado de código abierto de truffles para trabajar con el nuevo modelo de gas para permitirle usar Truffle en LACChain Networks.

Como primer paso se requiere instalar Truffle:

`npm install -g truffle`

`truffle version`

[![19](https://github.com/rozoandrescamilo/Despliege-de-nodo-local-con-Lacchain/blob/main/img/19.png?raw=true "19")](https://github.com/Despliege-de-nodo-local-con-Lacchain/blob/main/img/19.png?raw=true "19")

[![20](https://github.com/rozoandrescamilo/Despliege-de-nodo-local-con-Lacchain/blob/main/img/20.png?raw=true "20")](https://github.com/Despliege-de-nodo-local-con-Lacchain/blob/main/img/20.png?raw=true "20")

Se debe crear la carpeta de proyecto, por ejemplo MyDapp.

`mkdir MyDapp`

`cd MyDapp`

Para iniciar el servicio de truffle se ejecuta el siguiente comando en el directorio MyApp:

`truffle init`

[![1](https://github.com/rozoandrescamilo/BLCH-17/blob/main/img/1.png?raw=true "1")](https://github.com/BLCH-17/blob/main/img/1.png?raw=true "1")

Este comando creará un proyecto de truffle nuevo. Después de hacerlo, debe tener los siguientes archivos y carpetas:

contracts/: Directorio para contratos de Solidity
migrations/: Directorio para implementación con scriptable
test/: Directorio para archivos de prueba para probar su aplicación y contratos
truffle-config.js: archivo de configuración de truffle

### Compilación de contratos

Para iniciar se crea el contrato inteligente llamado DoTrackStatusValidator.sol y almacenarlo en la carpeta de contratos. Todos los contratos inteligentes que se creen deben almacenarse allí. 

[![2](https://github.com/rozoandrescamilo/BLCH-17/blob/main/img/2.png?raw=true "2")](https://github.com/BLCH-17/blob/main/img/2.png?raw=true "2")

[![3](https://github.com/rozoandrescamilo/BLCH-17/blob/main/img/3.png?raw=true "3")](https://github.com/BLCH-17/blob/main/img/3.png?raw=true "3")

Para compilar el contrato inteligente, se ejecuta el comando:

`truffle compile`

[![4](https://github.com/rozoandrescamilo/BLCH-17/blob/main/img/4.png?raw=true "4")](https://github.com/BLCH-17/blob/main/img/4.png?raw=true "4")

Se requiere instalar el gas-model-provider de truffle según Using Hyperledger Besu with Truffle para poder desplegar contratos y enviar transacciones con truffle:

`npm install -g @lacchain/truffle-gas-model-provider`

[![5](https://github.com/rozoandrescamilo/BLCH-17/blob/main/img/5.png?raw=true "5")](https://github.com/BLCH-17/blob/main/img/5.png?raw=true "5")

Crear un nuevo archivo llamado 1_deploy_contracts.js, y escribe el siguiente código:

[![6](https://github.com/rozoandrescamilo/BLCH-17/blob/main/img/6.png?raw=true "6")](https://github.com/BLCH-17/blob/main/img/6.png?raw=true "6")

A continuación, dado que [LACChain Networks requiere incluir dos parámetros(nodeAddress, expiration)](link), es necesario añadirlos como parámetros a la ABI del contrato. Por favor, añada estos dos parámetros como entradas en la función constructora.

[![7](https://github.com/rozoandrescamilo/BLCH-17/blob/main/img/7.png?raw=true "7")](https://github.com/BLCH-17/blob/main/img/7.png?raw=true "7")

A continuación, debemos editar la configuración de Truffle (truffle-config.js).

Para describir brevemente las partes que componen la configuración:

- networks: Contendrá la configuración de nuestro cliente Ethereum donde desplegaremos nuestros contratos
- compilers: Contendrá la configuración del compilador Solc

Se debe escribir la clave privada, el nodo IP de la dirección de red y el puerto RPC en la parte de redes:

[![8](https://github.com/rozoandrescamilo/BLCH-17/blob/main/img/8.png?raw=true "8")](https://github.com/BLCH-17/blob/main/img/8.png?raw=true "8")

Finalmente se obtiene el informe de despliegue donde se puede ver el contrato de dirección similar al siguiente usando el comando:

`truffle migrate -network lacchain`

```
Deploying 'DoTrackStatusValidator'
--------------------
transaction hash:0x31d91fa2524953e49cfc4c433ac939b56df8d9371fdde74c56a75634efcf823d
Blocks: 0            Seconds: 0
contract address:    0xFA3F403BeC6D3dd2eF9008cf8D21e3CA0FD1B9C4
block number:        4006082
block timestamp:     1574190784
account:             0xbcEda2Ba9aF65c18C7992849C312d1Db77cF008E
balance:             0
gas used:            340697
gas price:           0 gwei
value sent:          0 ETH
total cost:          0 ETH

```

# Consideraciones para red principal y Firefly

LACChain Networks son redes blockchain desarrolladas por LACChain Alliance y orquestadas por LACNet. Estas redes están clasificadas como redes públicas de blockchain autorizadas, tal como se define en la norma ISO TC307 WG5 TS23635 y han sido diseñadas con un enfoque especial en América Latina y el Caribe. Como redes públicas de blockchain, las Redes LACChain están abiertas a cualquier entidad en el mundo. Como redes permisionadas, todos los operadores de nodos, excepto los nodos observadores, deben autenticarse y comprometerse a cumplir con la regulación para poder ser permisionados y enviar transacciones o participar en el protocolo de consenso.

Las redes LACChain no tienen costes de transacción, lo que permite la escalabilidad de las aplicaciones. Existen dos tipos de Redes LACChain orquestadas por LACNet, las ProTestnets y las Mainnets. Las ProTestnets distribuyen los recursos necesarios para emitir transacciones de forma equitativa entre todos los nodos escritores y su uso es totalmente gratuito. Las Mainnets tienen un modelo basado en membresías para cubrir los costes del equipo de soporte de LACNet. Las membresías de nivel superior incluyen más tx por segundo y una respuesta más rápida a los tickets de soporte. En la actualidad, las redes LACChain son:

- Mainnet Omega network 
- Pro-Testnet network 

[![29](https://github.com/rozoandrescamilo/Despliege-de-nodo-local-con-Lacchain/blob/main/img/29.png?raw=true "29")](https://github.com/Despliege-de-nodo-local-con-Lacchain/blob/main/img/29.png?raw=true "29")

Membresias anuales: [https://lacnet.lacchain.net/get-your-membership/](https://lacnet.lacchain.net/get-your-membership/ "https://lacnet.lacchain.net/get-your-membership/")

En cuanto al stack de Firefly, que proporciona herramientas para gestionar transacciones, desplegar contratos inteligentes y APIs, y escalar aplicaciones web3 seguras. Es necesario desplegarlo sobre la Mainnet Omega network, siendo necesario contar con alguna de las membresías para cubrir los costes del equipo de soporte de LACNet.

Para este proceso se tendría de referencia la Guía oficial de LACNet para Desarrollo con la herramienta Firefly: [https://lacnet.lacchain.net/firefly/](https://lacnet.lacchain.net/firefly/ "https://lacnet.lacchain.net/firefly/")




















