#Redundancia de nuestros datos

Una de las partes más importantes de subir nuestros datos a la nube es asegurarnos que se encuentran siempre disponibles y protegidos frente a fallos puntuales del hardware o a situaciones de desastre en el propio datacenter.

Azure ofrece tres niveles de servicio de redundancia de nuestra información:

#####Almacenamiento con redundancia local

En el almacenamiento con redundancia local, por cada dato que es alojado dentro de Azure Storage, se realizan tres copias del mismo dentro del datacenter para evitar que cualquier fallo en el hardware provoque una pérdida de servicio en nuestra aplicación.

En el caso de que se produza un fallo en la copia que se está utilizando, el sistema detecta automáticamente el problema y comienza a hacer uso de una de las otras dos disponibles. Mientras tanto, comienza a crear una nueva réplica en otra parte del datacenter para asegurarse que en todo momento la información se encuentra triplicada.

Los acuerdos de nivel de servicio, *SLA*, aseguran una disponibilidad del 99,9%.

#####Almacenamiento con redundancia geográfica

El almacenamiento con redundancia geografica mejora las características de alta disponibilidad y durabilidad proporcionada por el almacenamiento con redundancia local. Además de las tres copias disponibles dentro del mismo datacenter, de forma automática y transparente se realizan otras tres copias más en otro datacenter dentro de la misma región situado a más de 640Km del principal. 

En el caso de que se produzca un desastre en el datacenter principal en el que nosotros hayamos almacenado nuestra información que provoque que no se pueda dar el servicio, se realizará de forma automática un *failover* o conmutación por error al secundario. De esta manera, nuestra aplicación podrá seguir funcionando aunque el datacenter principal no se encuentre operativo. 

El datacenter secundario es automaticamente seleccionado basándose en el primario, la siguiente tabla muestra un resumen de ello:


| Datacenter principal   |Datacenter Secundario   |
| ---------------------- | ---------------------- |
| EE. UU. Central Norte  | EE. UU. Central Sur    |
| EE. UU. Central Sur    | EE. UU. Central Norte  |
| Este de EE. UU.        | Oeste de EE. UU.       |
| Oeste de EE. UU.       | Este de EE. UU.        |
| Europa del Norte       | Europa Occidental      |
| Europa Occidental      | Europa del Norte       |
| Sudeste de Asia        | Asia Oriental          |
| Asia Oriental          | Sudeste de Asia        |
| China del Este         | China del Norte        |
| China del Norte        | China del Este         |

Los acuerdos de nivel de servicio, *SLA*, aseguran una disponibilidad del 99,9%.

#####Almacenamiento con redundancia geográfica y acceso en modo lectura

Este tipo de almacenamiento, complementa al almacenamiento con redundancia geográfica proporcionando acceso en modo lectura de forma constante a los datos disponibles en el datacenter secundario. Podremos usar tanto el punto de conexión primario como el secundario para leer la información a diferencia del anterior tipo almacenamiento, en el que únicamente es posible leer los datos del punto de conexión primario.

Los acuerdos de nivel de servicio, *SLA*, aseguran también una disponibilidad del 99,9%.

Más detalles: [Opciones de redundancia del Almacenamiento de Azure](http://msdn.microsoft.com/es-es/library/azure/dn727290.aspx "Opciones de redundancia del Almacenamiento de Azure")

