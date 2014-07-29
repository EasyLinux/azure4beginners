---
layout: default
---

#Interconexión de redes virtuales en Azure

Realizar la interconexión entre dos redes virtuales desplegadas en Azure es similar al proceso de conectar una red virtual en Azure con nuestra red en nuestro centro de datos. En ambos casos se hace uso de un gateway que establece un túnel IPSec/IKE entre ellas. No es necesario que las redes estén dentro de la misma suscripción o dentro de la misma región, es posible conectar redes de dos suscripciones o en dos partes del mundo diferentes permitiéndonos de esta manera establecer canales de comunicación seguros entre los puntos que necesitemos.

En este ejemplo interconectaremos dos redes virtuales dentro de una misma suscripción pero el proceso es similar para los otros escenarios posibles. El proceso contará con los siguientes pasos:

- Creación de las redes virtuales
- Configuracións de las redes locales
- Creación de los gateways con enrutado dinámico entre las redes
- Conexión entre los gateways

Los detalles paso a paso a continuación:

### Creación de las redes virtuales

En primer lugar es importante definir los rangos de direcciones IPs que vamos a emplear en nuestra configuración para las dos redes. Debemos evitar que haya una sobreposición entre ellos ya que provocaria errores de configuración.

En este caso, utilizaremos los siguientes rangos:


| Nombre      | Rango red virtual        | Rango red local         |
|-------------|--------------------------|-------------------------|
| VnetEurope  | VnetEurope (10.1.0.0/16) | VnetUsa (10.2.0.0/16)   |
| VnetUsa     | VnetUsa (10.2.0.0/16)    | VnetEurope (10.1.0.0/16)|
 
El **Rango de red virtual** será el rango de direcciones IPs que será utilizado en esa red concreta; el *Rango de red local* será el rango que emplee la red remota a la que nos conectemos a traves del tunel VPN. Como vemos en la tabla, el rango de red virtual de una coincide con el rango de local de la otra.

Una vez definidas nuestras redes procederemos a crearlas accediendo [al panel de gestión de Azure](http://manage.windowsazure.com "Panel de gestión de Azure") e iniciando la sesión. Una vez dentro, los pasos son los siguientes:

- Seleccionamos la opción de **New > Network Services > Virtual Network > Custom Create** disponibe en la parte inferior izquierda del panel.

![Menú nuevo](../images/networking-create-virtualNetwork-crossVnetAzure-Step1.png)

- Definimos en primer lugar la configuración de nuestra red **VnetEurope** en el datacenter de **West Europe**

![Menú nuevo](../images/networking-create-virtualNetwork-crossVnetAzure-Step2.png)


- En este caso no configuraremos ningun DNS propio y trabajaremos con los que proporciona directamente Azure. Por ello, continuamos con el siguiente paso del asistente.
- Configuramos el rango de IPs que hemos definido para la red: **10.1.0.0/16**

![Menú nuevo](../images/networking-create-virtualNetwork-crossVnetAzure-Step3.png)

- Tras ello, finalizamos el asistente y se creará la red **VnetEurope**. A continuación deberemos repetir los mismos pasos para configurar la red **VnetUSA** con su rango correspondiente y seleccionando el datacenter de **South Central US**

### Configuración de las redes locales

Una vez que las dos redes han terminado de crearse es el momento de configurar las redes locales asociadas a cada una de ellas antes de establecer los gateways.

 ![Menú nuevo](../images/networking-create-virtualNetwork-crossVnetAzure-Step4.png)
 
- Para ello, seleccionamos la opción de **New > Network Services > Virtual Network > Add Local Network** disponibe en la parte inferior izquierda del panel.

 ![Menú nuevo](../images/networking-create-virtualNetwork-crossVnetAzure-Step5.png)

- En primer lugar configuraremos la red local asociada a **VnetEurope** que en este caso es *VnetUsa*. Seleccionamos el nombre de la red como **VnetUsa**. A la hora de especificar la IP de VPN Device en este caso seleccionaremos una IP cualquiera debido a que aún no hemos creado el gateway. Una vez que lo creamos la modificaremos para que apunte a la correcta.

 ![Menú nuevo](../images/networking-create-virtualNetwork-crossVnetAzure-Step6.png)

- En el rango de direcciones IP sí que deberemos configurar el rango correcto de direcciones que hemos seleccionado para la red **VnetUsa**

 ![Menú nuevo](../images/networking-create-virtualNetwork-crossVnetAzure-Step7.png)

- Tras ello, finalizamos el proceso de creación de nuestra red local y repetimos el proceso para la red local asociada a **VnetUsa** que será *VnetEurope* con su rango IP *10.1.0.0/16*.

El siguien paso es asig82nar cada red local que hemos creado a su red correspondiente. Para ello:

- Seleccionamos del panel lateral izquierdo la pestaña de **Networks** y dentro del listado nuestra red **VnetEurope**. En las opciones disponibles en el menú superior elegimos **Configure**.

 ![Menú nuevo](../images/networking-create-virtualNetwork-crossVnetAzure-Step8.png

- En la sección de **Site-to-site Connectivity** marcamos el checkbox *Connect to the local network* y elegimos "VnetUsa".

 ![Menú nuevo](../images/networking-create-virtualNetwork-crossVnetAzure-Step9.png
 
- Tras salvar los cambios deberemos repetir el proceso de configuración para la otra red.

### Creación de los gateways con enrutado dinámico entre las redes

Una vez que hemos asignado las redes locales a cada una de nuestras redes virtuales que hemos creado es el momento de crear los gateways entre ellas para establecer la conexión VPN. 

- Seleccionamos del panel lateral izquierdo la pestaña de **Networks** y dentro del listado nuestra red **VnetEurope**. En las opciones disponibles en el menú inferior elegiremos **Create Gateway > Dynamic Routing**.

 ![Menú nuevo](../images/networking-create-virtualNetwork-crossVnetAzure-Step10.png)
 
Deberemos repetir el proceso para nuestra otra red. El proceso tarda unos 10 minutos aproximadamente en crearse. Podremos continuar con el mensaje cambie de *Creating Gateway* a *Connecting*. 

### Conexión entre los gateways 

Finalmente, cuando ambos gateways estén creados tendremos acceso a sus IPs. Por lo tanto, será necesario actualizar la configuración de nuestras redes locales para que apunten a las direcciones IPs correctas para establecer el túnel.

- Seleccionamos del panel lateral izquierdo la pestaña de **Networks** y en las opciones disponibles en el menú superior elegimos **Local Networks**. A continuación seleccionamos **VnetEurope** y en la parte inferior le damos a **Edit**.

 ![Menú nuevo](../images/networking-create-virtualNetwork-crossVnetAzure-Step11.png)
 
- En el campo de **VPN Device Ip Address** deberemos configurar la dirección IP del gateway correspondiente: el de europa en la red local de europa y el de usa en la red local de usa.

Tras eso, el paso final es configurar la clave compartida entre ambos para la autenticación entre ambas redes. Para ello, es posible realizarlo a [traves de la API Rest](http://msdn.microsoft.com/en-us/library/azure/dn770199.aspx "traves de la API Rest") o a traves de PowerShell.

En este último caso, los comandos necesarios serian los siguientes:

```bash
Set-AzureVNetGatewayKey -VNetName VnetEurope -LocalNetworkSiteName VnetUsa -SharedKey cd0456eff95c5529ea9e918043e19cbe
Set-AzureVNetGatewayKey -VNetName VnetUsa -LocalNetworkSiteName VnetEurope -SharedKey cd0456eff95c5529ea9e918043e19cbe
```

Siendo *cd0456eff95c5529ea9e918043e19cbe* la clave compartida y la misma en ambos casos. Tras ello, únicamente es necesario esperar a que se inicialicen las conexiones. El portal lo reflejará indicando que la conexión se ha establecido correctamente.

 ![Menú nuevo](../images/networking-create-virtualNetwork-crossVnetAzure-Step12.png)


