---
layout: default
---
# Las herramientas de consola

Además de la gestión a través de la API REST o cualquiera de los SDKs disponible existe  la opción de emplear las utilidades de línea de comando multiplataforma, también conocida como *Azure Cross-Platform Command-Line Interface (xplat-cli)* 

Esta solución proporciona un nivel de funcionalidad similiar al portal de gestión de Azure. Al igual que los SDKs, su código es abierto y [está disponible en el repositorio de GitHub del equipo de Azure.](https://github.com/Azure/azure-sdk-tools-xplat "") Está implementada en Javascript utilizando el SDK disponible para Node.js por lo que será necesario tenerlo instalado para utilizarlas. 

### Instalación de xplat-cli en Linux
En el caso de Windows u OS X existe un paquete de instalación disponible que facilita el proceso de configuración de las herramientas. En el caso de Linux, será necesario hacer uso del gestor de paquetes de Node.js, **npm**, para realizar el proceso. Si no disponemos de Node.js es posible consultar [las instrucciones de instalación en su página web.](http://nodejs.org/ "Node.js") o a través de los repositorios de nuestra distribucción. 

Tras ello, únicamente necesitaremos ejecutar el siguiente comando:

```bash
sudo npm install azure-cli -g
```

Es posible comprobar que la instalación se ha realizado de forma correcta ejecutando el comando **azure** sin ningún tipo de parámetro. La salida sera algo similar a lo siguiente:
```bash
info:             _    _____   _ ___ ___
info:            /_\  |_  / | | | _ \ __|
info:      _ ___/ _ \__/ /| |_| |   / _|___ _ _
info:    (___  /_/ \_\/___|\___/|_|_\___| _____)
info:       (_______ _ _)         _ ______ _)_ _
info:              (______________ _ )   (___ _ _)
info:
info:    Windows Azure: Microsoft Cloud Platform
info:
info:    Tool version 0.8.0
help:
help:    Display help for a given command
help:      help [options] [command]
help:
help:    Opens the portal in a browser
help:      portal [options]
help:
help:    Commands:
help:      account        Commands to manage your account information and publish settings
help:      config         Commands to manage your local settings
help:      hdinsight      Commands to manage your HDInsight accounts
help:      mobile         Commands to manage your Mobile Services
help:      network        Commands to manage your Networks
help:      sb             Commands to manage your Service Bus configuration
help:      service        Commands to manage your Cloud Services
help:      site           Commands to manage your Web Sites
help:      sql            Commands to manage your SQL Server accounts
help:      storage        Commands to manage your Storage objects
help:      vm             Commands to manage your Virtual Machines
help:
help:    Options:
help:      -h, --help     output usage information
help:      -v, --version  output the application version
```


### Configuración de tu suscripción a Azure

Para poder acceder a los recursos de nuestra suscripción a través de la línea de comandos es necesario que la configuremos en primer lugar. Para ello, utilizaremos el fichero de configuración que proporciona Azure. Este fichero contiene un certificado que se instala y permite trabajar con la suscripción mientras la suscripción siga activa y el certificado no sea revocado desde el panel de gestión de Azure. Esta configuración es necesaria realizarla una única vez.

**Nota:** cualquier persona que acceda al fichero de configuración descargado podrá utilizarlo para conectarse a la suscripción. Por ello, es importante protegerlo para evitar accesos no autorizados. 

Para obtener el fichero de configuración es necesario ejecutar el siguiente comando. Tras ello, se abrirá el navegador y tendrás que iniciar sesión con tus credenciales de acceso a Azure si no estaba la sesión iniciada. Tras ello, un fichero con la extensión *.publishsettings* se descargará a tu ordenador. 

Tras completarse la descarga sera necesario importar el fichero de configuración con los detalles de tu suscripción a través del comando: 

```bash
azure account import [ruta al fichero .publishsettings]
```

Una vez finalizado podremos acceder a nuestros recursos en Azure.

### Trabajando con las herramientas de línea de comandos

Al ejecutar el comando de **azure** sin parámetros se nos muestra una pequeña ayuda con las opciones disponibles. A continuación está un resumen de la salida del mismo:

```bash
help:    Commands:
help:      account        Commands to manage your account information and publish settings
help:      config         Commands to manage your local settings
help:      hdinsight      Commands to manage your HDInsight accounts
help:      mobile         Commands to manage your Mobile Services
help:      network        Commands to manage your Networks
help:      sb             Commands to manage your Service Bus configuration
help:      service        Commands to manage your Cloud Services
help:      site           Commands to manage your Web Sites
help:      sql            Commands to manage your SQL Server accounts
help:      storage        Commands to manage your Storage objects
help:      vm             Commands to manage your Virtual Machines
```

Cada uno de los comandos listados es un comando de alto nivel que nos sirve para seleccionar un área de trabajo. Por ejemplo, el primero de ellos, **account**, nos permite realizar tareas de gestión de nuestra suscripción.

De forma general, los comandos tienen la siguiente estructura:

```bash
azure <comando> <operación> [parámetros]
```

Sin embargo, es posible que algunos comandos tengan subcomnados disponibles por lo que la estructura se veria modificada quedando:

```bash
azure <comando> <subcomando> <operación> [parámetros]
```

Para obtener ayuda sobre las opciones de cada comando es posible utilizar el parámetro *--help* o el comando *azure help [comando].*

Por ejemplo, es posible listar las suscripciones 
azure account list


### Creación de scripts usando las herramientas de consola

Aunque es posible interactuar de forma manual con nuestros recursos en Azure a través de la línea de comandos, la parte más interesante es la de utilizarlo integrado con el resto de herramientas del sistema operativo para crear scripts más complejos. 

Por ejemplo, es posible seleccionar todos los sitios web desplegados en Azure que estén en ejecucción e indicarles que se paren de la siguiente forma. 

```bash
azure site list | grep 'Running' | awk '{system("azure site stop "$2)}'
```

Esto se consigue gracias a que los resultados del primer comando son pasados al siguiente a traves de una tubería. Con *grep* filtramos aquellos que estan en ejecucción para finalmente capturar su bombre e indicarles que se paren. 

Sin ningún parámetro especifico los comandos devolverán los resultados a la salida estandar. Cada línea incluirá un prejijo:

- info: para mensajes de información general
- data: para los resultados devueltos por Azure.

Es posible obtener más detalles haciendo uso del parámetro *--verbose*. 

When creating scripts, you often need to capture the output of a command and either pass this to another command or perform an operation on the output such as checking for a specific value. The xplat-cli generates output to STDOUT and STDERR. Each line is prefixed by the strings info: for informational status messages, or data: for data returned about a service; however, you can instruct the xplat-cli to return more verbose information by using the --verbose or -v parameter. This will return additional information prefixed by the string verbose:.

```bash
info:    Executing command site list
verbose: Subscription ####################################
verbose: Enumerating sites
verbose: [
verbose:     {
verbose:         ComputeMode: 'Shared',
verbose:         HostNameSslStates: {
verbose:             HostNameSslState: [
verbose:                 {
verbose:                     IpBasedSslResult: {
verbose:                         $: { i:nil: 'true' }
verbose:                     },
verbose:                     SslState: 'Disabled',
verbose:                     ToUpdateIpBasedSsl: {
verbose:                         $: { i:nil: 'true' }
verbose:                     },
verbose:                     VirtualIP: {
verbose:                         $: { i:nil: 'true' }
verbose:                     },
verbose:                     Thumbprint: {
verbose:                         $: { i:nil: 'true' }
verbose:                     },
verbose:                     ToUpdate: {
verbose:                         $: { i:nil: 'true' }
verbose:                     },
verbose:                     Name: 'myawesomesite.azurewebsites.net'
verbose:                 },
...
verbose:     }
verbose: ]
data:    Name           Status   Mode  Host names
data:    -------------  -------  ----  -------------------------------
data:    myawesomesite  Running  Free  myawesomesite.azurewebsites.net
info:    site list command OK

```

Las herramientas de línea de consola trabajan por debajo con información en formato JSON. Si estamos interesados en tratar directamente con este tipo de resultado en lugar  del formato a traves de la salida estándar podemos emplear el parámaetro *--json*

Gracias a ello, podemos encadenar la salida con comandos como **jsawk** y procesar de una forma más sencilla los resultados.

```bash
azure site list --json | jsawk -n 'out(this.Name)' | xargs -L 1 azure site delete -q 
```


