---
layout: default
---

#Interconexión de redes virtuales entre AWS y Azure

Realizar la interconexión entre una red virtual desplegada en AWS y otra red virtual en Azure es muy similiara los escenarios de conectar dos redes virtuales en Azure, o una red virtual en Azure y otra en nuestro datacenter. En ambos casos se hace uso de un gateway que establece un túnel IPSec/IKE entre ellas.

En este ejemplo interconectaremos una red privada virtual desplegada en AWS y otra desplegada en Azure. Los detalles paso a paso a continuación:

### Creación de la VPC en AWS

En primer lugar es importante definir los rangos de direcciones IPs que vamos a emplear en nuestra configuración para las dos redes. Debemos evitar que haya una sobreposición entre ellos ya que provocaria errores de configuración.

En este caso, utilizaremos los siguientes rangos:


| Nombre      | Rango red virtual | 
|-------------|-------------------|
| VnetAWS     | 10.0.0.0/16       | 
| VnetAzure   | 10.1.0.0/16       | 


Una vez definidas nuestras redes procederemos a crearlas. En primer lugar, crearemos nuestra VPC a traves del portal de AWS. Seleccionaremos una red del tipo **VPC with a single public subnet only**.  

![Menú nuevo](../images/networking-create-virtualNetwork-crossVnetAWS-Step1.png)

Seleccionaremos el rango **10.0.0.0/16** para la red.

![Menú nuevo](../images/networking-create-virtualNetwork-crossVnetAWS-Step2.png)
 

### Despliegue de una instancia linux en EC2

Emplearemos OpenSwan para crear el túnel VPN entre las redes ques desplegaremos entre los dos proveedores. Para ello, será necesario acceder al panel de control de AWS y seleccionar el servicio de EC2

- En primer lugar, elegimos la imagen que queremos usar. En este caso una máquina virtual con Ubuntu Server.

![Menú nuevo](../images/networking-create-virtualNetwork-crossVnetAWS-Step3.png)

- A continuación establecemos el tamaño de la máquina virtual. Con una instancia pequeña es suficiente.

![Menú nuevo](../images/networking-create-virtualNetwork-crossVnetAWS-Step4.png)

- Tras finalizar el proceso de creación de la máquina virtual será necesario asignarle una **Elastic IP** que será la que empleemos para establecer la conexión con la red que crearemos en Azure.

![Menú nuevo](../images/networking-create-virtualNetwork-crossVnetAWS-Step5.png)

- Asociamos la IP a la instancia que acabmos de crear.

![Menú nuevo](../images/networking-create-virtualNetwork-crossVnetAWS-Step6.png)

- Será necesario que habilitemos los puertos necesarios dentro de la máquina virtual para poder establecer la conexión VPN. Será necesario tener disponible el puerto **500** y el **4500** UDP.

Tras ello, continuamos con la creación de la máquina virtual en Azure.

### Creación de la red privada en Azure

En el caso de Azure, podremos crearla accediendo [al panel de gestión de Azure](http://manage.windowsazure.com "Panel de gestión de Azure") e iniciando la sesión. Una vez dentro, los pasos son los siguientes:

- Seleccionamos la opción de **New > Network Services > Virtual Network > Custom Create** disponible en la parte inferior izquierda del panel. El proceso es idéntico [al mencionado para conectar dos redes virtuales en Azure.](networking-create-virtualNetwork-crossVnetAzure "al mencionado para conectar dos redes virtuales en Azure.")

- Definimos en primer lugar la configuración de nuestra red **VnetAzure** en el datacenter de **West Europe**. Continuamos con el asistente.

- En este caso no configuraremos ningun DNS propio y trabajaremos con los que proporciona directamente Azure. Lo que sí seleccionaremos es la opción de **Site-to-site connectivity** y la especificación de una nueva red local.

![Menú nuevo](../images/networking-create-virtualNetwork-crossVnetAzure-Step7.png)

- Configuramos en este punto el rango de IPs de la red desplegada en AWS  y de la IP de la máquina que acabamos de crear en AWS.

![Menú nuevo](../images/networking-create-virtualNetwork-crossVnetAzure-Step8.png)

- Ahora configuramos  el rango de IPs de la red que usaremos en Azure y de las subredes que necesitemos.

![Menú nuevo](../images/networking-create-virtualNetwork-crossVnetAzure-Step9.png)

- Tras ello, finalizamos el asistente y se creará la red **VnetAzure**. Una vez que se haya terminado de crear la red, necesitaremos seleccionarla y en el menú inferior seleccionar **Create Gateway > Static Routing**.
- El proceso llevará unos minutos tras los cuales el gateway se creará y tendremos la IP disponible, dato que necesitaremos a continuación.

### Configuración Open Swan en la máquina virtual

En este proceso será necesario realizar la configuración de OpenSwan dentro de la máquina virtual que hemos creado dentro de EC2 en AWS. Para ello, el proceso es similar al disponible en **Configuración de OpenSwan** en el artículo de [crear una red Site to Site](networking-create-virtualNetwork-site2site "crear una red Site to Site").

En este caso, el lado izquierdo del túnel VPN se referirá a la parte de AWS y el lado derecho al de Azure. Deberemos configurar los parámetros *left, leftsubnet, right y righsubnet* con los valores correspondientes:

- left: dirección IP del servidor OpenSwan 
- leftsubnet:  rango de direcciones IP definido en la VPC
- right: dirección IP del gateway de Azure obtenida en el apartado anterior
- rightsubnet: rango de direcciones IP definido en la red virtual en Azure

La prueba de que todo funciona correctamente es similar a lo que se menciona en dicho artículo. Es importante tener en cuenta que necesitaremos configurar las reglas de enrutado y activar dicha funcionalidad dentro de la máquina virtual utilizando **iptables** y la variable **net.ipv4.ip_forward**. 





