---
layout: default
---
#Cómo gestionar los puertos de una máquina virtual

A la hora de crear una máquina virtual en Azure cualquier tipo de de tráfico entrante, ya sea iniciado por un equipo fuera de Azure u otras máquinas configuradas en redes virtuales o servicios cloud diferentes a la de destino, será bloqueado por motivos de seguridad. En el caso de que necesitemos habilitar estas comunicaciones deberemos habilitar un extremo o *endpoint*. Proceso similar a la apertur de puertos de un firewall.

Existen dos tipos de extremos que podemos configurar para una máquina virtual:

- **Normal** (*Stand-Alone*): es el extremo por defecto y al que estamos más acostumbrados a trabajar. Nos permite establecer una conexión directa con un servicio ejecutándose en ese puerto dentro de la máquina virtual como por ejemplo SSH. Es comparable a los puertos abiertos en un firewall para acceder a recursos especificos.

- **Balanceado**: este tipo de extremo no esta asociados directamente a una máquina si no a un conjunto de ellas y permite la distribucción del tráfico entrante de forma aleatoria entre ellas de forma transparente. Su utilidad reside en escenarios con picos de carga puntuales ya que permite que añadamos nuevas máquinas dando servicio en el mismo puerto de forma transparente para el usuario.


### Crear un extremo normal


Para crear un nuevo extremo sera necesario contar con una máquina virtual previamente en ejecucción. Si es asi, entramos [al panel de gestión de Azure](http://manage.windowsazure.com "Panel de gestión de Azure") e iniciamos la sesión. Una vez dentro, los pasos son los siguientes:

- Seleccionamos en el menú lateral la pestaña de **Virtual Machines** y dentro de ella la máquina virtual a la que le queramos añadir el extremo. No es necesario que esté en ejecucción para ello.

![Menú nuevo](../images/virtualmachines-create-endpoints-Step1.png)

- En el menu superior seleccionamos **Endpoints**

![Menú nuevo](../images/virtualmachines-create-endpoints-Step2.png)

- Dentro de esa página seleccionaremos la opción de **Add** disponible en el menú inferior para añadir nuestro nuevo extremo.

![Menú nuevo](../images/virtualmachines-create-endpoints-Step3.png)

- Seleccionamos la opción de añadir un extremo **Add a stand-alone endpoint**

![Menú nuevo](../images/virtualmachines-create-endpoints-Step4.png)

- A continuación, podemos seleccionar alguno de los extremos estándar disponibles en el desplegable del campo **Name** o añadir uno nuevo.Como ejemplo, seleccionaremos el puerto **HTTP** y se autocompletarán el resto de campos.

![Menú nuevo](../images/virtualmachines-create-endpoints-Step5.png)

- Tras eso, finalizamos el asistente y nuestra máquina sera accesible a través del puerto TCP 80 para resolver nuestras peticiones web. 

### Crear un extremo balanceado

Para crear un nuevo extremo sera necesario contar con una máquina virtual previamente en ejecucción. Si es asi, entramos [al panel de gestión de Azure](http://manage.windowsazure.com "Panel de gestión de Azure") e iniciamos la sesión. Una vez dentro, los pasos son los siguientes:

- Seleccionamos en el menú lateral la pestaña de **Virtual Machines** y dentro de ella la máquina virtual a la que le queramos añadir el extremo. No es necesario que esté en ejecucción para ello.

![Menú nuevo](../images/virtualmachines-create-endpoints-Step1.png)

- En el menu superior seleccionamos **Endpoints**

![Menú nuevo](../images/virtualmachines-create-endpoints-Step2.png)

- Dentro de esa página seleccionaremos la opción de **Add** disponible en el menú inferior para añadir nuestro nuevo extremo.

![Menú nuevo](../images/virtualmachines-create-endpoints-Step3.png)

- Seleccionamos la opción de añadir un extremo **Add a stand-alone endpoint** y continuaremos igual que antes.

![Menú nuevo](../images/virtualmachines-create-endpoints-Step4.png)

- A continuación, podemos seleccionar alguno de los extremos estándar disponibles en el desplegable del campo **Name** o añadir uno nuevo.Como ejemplo, seleccionaremos el puerto **HTTP** y se autocompletarán el resto de campos. Para que el puerto este balanceado será necesario marcar la casilla de **Create a load-balanced set**

![Menú nuevo](../images/virtualmachines-create-endpoints-Step6.png)

- En el siguiente punto definiremos el nombre del conjunto balanceado. En este caso, **ServidorWebHTTP**. El puerto ya nos vendrá configurado por lo que hemos seleccionado en el paso anterior y los otros dos campos tienen los valores por defecto de comprobación para detectar la disponibilidad de las máquinas.

![Menú nuevo](../images/virtualmachines-create-endpoints-Step7.png)

- Aceptamos el asistente y tendremos ya disponible el puerto balanceado.
