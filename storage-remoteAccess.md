---
layout: default
---
#Acceso remoto a nuestros datos en Azure Storage

En el momento de creación de nuestra cuenta de almacenamiento, Azure genera dos claves de acceso de 512 bits cada una. Estas claves son usadas para autenticarse con Azure cuando deseamos acceder a nuestros datos. 

Es muy importante proteger dichas claves de acceso ya que cualquier persona que tenga el nombre de la cuenta de almacenamiento y la clave de acceso podria tomar control de la información que en ella esté almacenada. Por ello, Azure proporciona una clave primaria y otra secundaria para que puedas mantener el acceso a los datos mientras regeneras la otra clave.

Una regeneración de las claves puede afectar a cualquier servicio que haga uso del almacenamiento como puede ser las máquinas virtuales, los servicios multimedia u otras aplicaciones. Por ello, es importante planear qué servicios son necesario actualizar cuando se vaya a producir un cambio de las claves.

### Cómo consultar las claves de almacenamiento

Para consultar nuestras claves de acceso de  nuestra cuenta de almacenamiento es necesario acceder [al panel de gestión de Azure](http://portal.azure.com "Panel de gestión de Azure") e iniciar la sesión. Una vez dentro, los pasos son los siguientes:

- Seleccionamos nuestra cuenta dentro del panel principal.

![Tesela de la cuenta de almacenamiento](../images/storage-remoteAccess-getKey-Step1.png)

- Dentro de la nueva hoja que se despliega, seleccionamos la opción de **Keys** haciendo click.

![Claves de almacenamiento](../images/storage-remoteAccess-getKey-Step2.png)

- Obtenemos nuestras claves primarias y secundarias. Si queremos copiarlas lo podemos hacer directamente pulsando el botón situado a la derecha de la clave.

![Claves de almacenamiento](../images/storage-remoteAccess-getKey-Step3.png)

### Cómo regenerar las claves ya existentes

A la hora de realizar un cambio de las claves existentes es necesario realizar los siguientes pasos:

- Actualizar las cadenas de conexión de las aplicaciones que hagan uso de la cuenta de almacenamiento para que usen la clave secundaria.
- Seguir los pasos del apartado anterior y seleccionar en la parte superior **Regenerate Primary** y confirmarlo.

![Claves de almacenamiento](../images/storage-remoteAccess-regenerateKey-Step1.png)

- Actualizar de nuevo las cadenas de conexión de las aplicaciones que hagan uso de la cuenta para que empleen la nueva clave primaria que hemos generado. 
- Una vez que este completado el paso anterior, repetimos el proceso para actualizar la clave secundaria seleccionando en este caso **Regenerate Secondary** y confirmarmos.

![Claves de almacenamiento](../images/storage-remoteAccess-regenerateKey-Step1.png)

Una vez que conocemos cómo trabajar con las claves de nuestra cuenta de almacenamiento ya podemos acceder a ella de forma remota.
