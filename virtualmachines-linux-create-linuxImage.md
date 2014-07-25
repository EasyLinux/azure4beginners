---
layout: default
---
# Crear una imagen de una máquina virtual Linux

Dentro de la galería podemos encontrar diferentes imágenes de sistemas operativos Linux listas para usar. Sin embargo, éstas no suelen incorporar el software que necesitamos haciendo que cada vez que despleguemos una nueva imagen sea necesario personalizarla antes de ponerla en producción. 

Para evitar esta situación podemos hacer uso de la imágenes de máquinas virtuales. Estas nos permiten realizar las labores de configuración una vez y reutilizar esa imagen el número de veces que deseemos de forma similiar a las predefinidas de la galeria.

- En primer lugar, sera necesario conectarnos de forma remota a la máquina virtual a través de SSH.

![Terminal SSH](../images/virtualmachines-linux-create-linuxImage-Step1.png)

- Una vez dentro, ejecutamos el siguiente comando. Gracias a él, cualquier información específica de la máquina como su nombre u otra información sera eliminada. De esta manera la imagen quedará limpia para cuando aprovisionemos nuevas a partir de ella.

```bash
sudo waagent -deprovision
```
- Una vez que haya terminado cerramos la sesión SSH.

![Terminal SSH](../images/virtualmachines-linux-create-linuxImage-Step2.png)

- A continuación, para la creación de la imagen es necesario acceder [al panel de gestión de Azure](http://manage.windowsazure.com "Panel de gestión de Azure") e iniciar la sesión. 

- Una vez dentro, eleccionamos de nuestro panel de control la opción de **Virtual Machines** y dentro de ella nuestra máquina virtual Linux ya configurada.

![Menu del portal](../images/virtualmachines-linux-create-linuxImage-Step3.png)

- En la parte inferior de la pantalla encontraremos las diferentes acciones que podemos realizar con la máquina. Para capturar la imagen en primer lugar será necesario pararla. Si no lo hiciéramos la copia podria realizarse de una forma no consistente al perderse información que estuviera en la memoria en ese momento. Por lo tanto, selecciamos la opción de **Shutdown**

![Apagar la máquina](../images/virtualmachines-linux-create-linuxImage-Step4.png)

- Cuando la máquina haya terminado de apagarse, el siguiente paso es seleccionar la opción de **Capture**. Se nos abrirá la siguiente ventana para completar la información de la imagen que vamos a crear. Será necesario que completemos el campo de **Image Name** con el nombre que queramos asignar a nuestra imagen y el campo de **Image Description** con una pequeña descripción de la misma. Marcaremos el checkbox indicando que hemos ejecutado el comando de desaprovisionamiento y tras eso finalizaremos haciendo click en el tick.

![Capturar la imagen](../images/virtualmachines-linux-create-linuxImage-Step5.png)

Es importante tener en cuenta que, como indica la captura anterior, cuando creamos una nueva imagen esta es eliminada. Por lo tanto, no es recomendable capturar imágenes con servicios que esten ya directamente en producción dado que nos quedariamos sin servicio hasta que desplegaramos de nuevo la imagen.

Cuando haya terminado el proceso de creación de la imagen esta aparecerá disponible en nuestro panel como *Available*

![Capturar la imagen](../images/virtualmachines-linux-create-linuxImage-Step6.png)

A partir de este momento, cada vez que vayamos a crear una nueva imagen estará disponible en la pestaña de **My Images.**

![Capturar la imagen](../images/virtualmachines-linux-create-linuxImage-Step7.png)
