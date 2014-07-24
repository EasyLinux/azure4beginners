# La API REST

El portal de gestión de Azure permite realizar la mayor parte de las tareas relacionadas con la creación, modificación y mantenimiento de los servicios que tenemos desplegados. Todo ello implica en acceder a través del navegador e interactuar directamente con él.  Esto puede resultar suficiente cuando se trabaja a pequeña escala en escenarios de desarrollo o de pruebas; sin embargo, segun aumenta el número de servicios es necesario empezar a utilizar herramientas que permitan automatizar los procesos.

Para facilitar este trabajo Azure expone la capacidad de gestionar estos servicios a través de la API REST. Todas las peticiones se realizan a través del protocolo SSL [autenticadas usando certificados de gestión o a traves de Azure Active Directory](http://msdn.microsoft.com/en-us/library/azure/ee460782.aspx "Auntenticación de peticiones").

Para conocer con más detalle las operaciones disponibles a través de la API existe una referencia pública de consulta ordenada por el área o categoría del servicio:

- [Gestión del servicio](http://msdn.microsoft.com/en-us/library/azure/dd179380.aspx#BKMK_ServiceManagement "Gestión del servicio")
- [Servicios de computación](http://msdn.microsoft.com/en-us/library/azure/dd179380.aspx#BKMK_Compute "Servicios de computación")
- [Servicios de datos](http://msdn.microsoft.com/en-us/library/azure/dd179380.aspx#BKMK_DataServices "Servicios de datos")
- [Servicios de aplicaciones](http://msdn.microsoft.com/en-us/library/azure/dd179380.aspx#BKMK_AppServices "Servicios de aplicaciones")
- [Servicios de red](http://msdn.microsoft.com/en-us/library/azure/dd179380.aspx#BKMK_Networking "Servicios de red")