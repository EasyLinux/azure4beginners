# Tipos de almacenamiento

Azure Storage incluye tres tipos de servicio como ya hemos mencioando: blobs, tablas y colas. A continuación, veremos un resumen de qué ofrecen y cómo trabajar con cada uno de ellos.

### Almacenamiento basado en *Blobs*

Este tipo de almacenamiento proporciona un servicio para almacenar grandes cantidades de información no estructurada como documentos o imágenes que pueden ser accedidads desde cualquier parte del mundo a través de una conexión HTTTP o HTTTPs. Cada fichero blob puede tener un tamaño desde pocos bytes hasta cientos de gigabytes. 

Los usos más habituales son los siguientes:

- Acceso a imágenes y ficheros directamente a traves de nuestro navegador web
- Almacenar ficheros para acceder a ellos de forma distribuida
- Almacenar ficheros de video o audio para realizar streaming.
- Realización de copias de seguridad y recuperación ante desastres de nuestra información.

Dicha información puede ser accedida de forma pública o privada segun la configuración que establezcamos. 

A la hora de trabajar con este tipo de almacenamiento es necesario conocer los siguientes conceptos: 

- **Contenedor**: Los contenedores proporciona una forma sencilla de agrupar los diferentes ficheros *blob* alojados en nuestra cuenta. Cada cuenta puede tener un número ilimitado de contenedores y cada contenedor, un número ilimitado de ficheros *blob*. 

- **Fichero blob**: Cualquier tipo de fichero de cualquier tamaño. Existen dos tipos que pueden ser almacenados en nuestra cuenta: los ficheros de tipo **página** y tipo **bloque**. Los primeros permiten ficheros de hasta 1TB mientras que los segundos de hasta 200GB. En esta guía trabajaremos con los ficheros de tipo bloque. En [la documentación oficial](http://msdn.microsoft.com/en-us/library/azure/ee691964.aspx "Blobs de tipo bloque y página") se pueden encontrar más detalles sobre estos dos tipos.

- **URL de acceso**: para poder acceder a nuestra información, el formato de la dirección URL es el siguiente: http://**\<cuenta>**.blob.core.windows.net/**\<contenedor>**/**\<blob>** 

Es posible realizar una comparación con un sistema tradicional de ficheror. Nuestra cuenta de almacenamiento seria equivalente a nuestra unidad de disco; un contenedor a una carpeta y los *blobs* a los ficheros.

## Almacenamiento basado en tablas

El servicio de almacenamiento en Azure basado en Tablas permite almacenar grandes cantidades de información estructurada que escalen según lo haga la demanda. Se basa en un almacenamiento NoSQL que permite acceder a la información desde dentro o fuera de Azure a traves de llamadas autenticadas. Es el servicio ideal para almacenar datos de forma no-relacional y estructurada. 

Sus usos más habituales son los siguientes:

- Almacenar terabytes de información para ser utilizada en servicios web.
- Almacenar conjuntos de datos que no requieran claves externas, procedimientos almacenados o *joins* complejos y puedan ser desnormalizadas para mejorar la rapidez del acceso.
- Realizar consultas rápidas basadas en un índice clusterizado.
- Acceder a información empleando el protocolo OData y consultas LINQ sobre las librerías WFC Data Services de .Net.

Al igual que con el almacenamiento en formato *Blob* es necesario conocer una serie de conceptos. 

- **Tabla**: Una tabla es una colección de entidades sin esquema; es decir, cada entidad puede tener un conjunto diferente de propiedades dentro de una misma table. El número de tablas que se pueden almacenar dentro de una cuenta de almacenamiento viene únicamente limitado por la propia capacidad de almacenamiento de la cuenta. 

- **Entidades**: Una entidad es un conjutno de propiedades similar a una fila dentro de una base de datos que tiene un tamaño máximo de 1MB. 

- **Propiedades**: Una propiedad es un conjunto clave-valor. Cada entidad puede contener hasta 252 propiedades. Además de esas, todas las entidades tienen tres propiedades suyas que son la clave de la partición, la clave de la fila y una marca de tiempo. Aquellas entidades que tienen la misma clave de partición pueden ser consultadas de una forma más rápida además de ser actualizadas o insertadas en operaciones atómicas. La clave de fila de la entidad es única dentro de una partición.

- **URL de acceso**: para poder acceder a nuestra información, el formato de la dirección URL es el siguiente: http://**\<cuenta>**.table.core.windows.net/**\<tabla>** 


## Almacenamiento basado en colas

El servicio de almacenamiento en Azure basado en Colas permite almacenar un gran número de mensajes que pueden ser accedidos desde cualquier parte del mundo mediante peticiones HTTP o HTTPs autenticadas. 

Sus usos más habituales son los siguientes:

- Crear una cola de trabajo para procesar tareas de forma asíncrona.
- Enviar mensajes entre diferentes servicios en los que uno sea el productor y el otro el consumidor.

Al igual que con los dos otros tipos de almacenamiento es necesario conocer una serie de conceptos. 

- **Cola**: una cola es una colección de mensajes. Cada cola puede contener millones de mensajes, ese número únicamente estará limitado por la propia capacidad de almacenamiento de la cuenta.

- **Mensaje**: información almacenada en cualquier tipo de formato con un tamaño máxim ode 64KB. 

- **URL de acceso**: para poder acceder a los mensajes, el formato de la dirección URL es el siguiente: http://**\<cuenta>**.queue.core.windows.net/**\<cola>** 

