---
layout: default
---
### Creación de una red virtual solo en Azure

Para crear una nueva red virtual que vaya a ser usada para desplegar servicios que esten únicamente en Azure es necesario acceder [al panel de gestión de Azure](http://manage.windowsazure.com "Panel de gestión de Azure") e iniciar la sesión. Una vez dentro, los pasos son los siguientes:

- Seleccionamos el botón de **New** en la parte inferior izquierda de la página.

![Menú nuevo](../images/networking-create-virtualNetwork-cloud-Step1.png)

- Dentro de la nueva hoja que se despliega, buscamos la opción de **Network Services > Virtual Network** y hacemos click en **Custom Create**.

![Menú crear red virtual](../images/networking-create-virtualNetwork-cloud-Step2.png)

- Elegimos un nombre para nuestra red y el datacenter sobre la que la vamos a desplegar. 

![Menú crear red virtual](../images/networking-create-virtualNetwork-cloud-Step3.png)

- A continuación podemos definir un servidor de DNS privado. En este caso, dado que no vamos a desplegar un servidor propio dentro de la red, lo dejaremos en blanco para que emplee losde Azure. Por otra parte, dado que nuestra red va a usar únicamente servicios alojados en Azure no necesitamos configurar ninguno de los apartados relacionados con la conectividad con nuestra propia red. Por lo tanto, dejaremos también los dos checkboxes sin marcar.

![Menú crear red virtual](../images/networking-create-virtualNetwork-cloud-Step4.png)

- En el último paso seleccionaremos el rango de direcciones IP que queremos usar en nuestra red privada virtual. Seleccionamos **10.0.0.1** en el campo **Starting IP** y elegimos uno. Además de ello es posible modificar el valor del **CIDR** y agregrar sub redes dentro de nuestra red.

![Menú crear red virtual](../images/networking-create-virtualNetwork-cloud-Step5.png)

- Tras ello, finalizamos el proceso y nuestra red quedará configurada.

![Menú crear red virtual](../images/networking-create-virtualNetwork-cloud-Step6.png)
