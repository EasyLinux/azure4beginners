#Azure para principiantes
Una guía básica de cómo empezar a utilizar los servicios de Microsoft Azure enfocado inicialmente para usuarios de entornos basados en GNU-Linux.


### Índice de contenidos

1. Introducción a Microsoft Azure
    - ¿Qué es?
    - ¿Qué ofrece?
    - Azure y su modelo de apertura
2. Primeros pasos con Azure
    - Registro
	- El portal de gestión
	- El portal de facturación
3. Elementos básicos de infraestructura:
    - [Configuración del almacenamiento](storage-start.md "Configuración del almacenamiento")
		- [Tipos de almacenamiento] (storage-types.md "Tipos de almacenamiento Azure Storage") 
		- [Opciones de redundancia](storage-redundancy.md "Tipos de redundancia en Azure Storage")
		- [Acceso remoto a los datos almacenados](storage-remoteAccess.md "Acceso remoto a Azure Storage")
		- Accediendo al almacenamiento a través de red con Azure Files
	- [Configuración de la red](networking-start.md "Configuración de la red")
	    - [Tipos de redes virtuales](networking-types.md "Tipos de redes virtuales")
		- [Crear de una red virtual en Azure](networking-create-virtualNetwork-cloud.md "Crear una red virtual en Azure")
		- Creación de redes VPN
			1. Conexión VPN Punto a Sitio
			2. [Conexión VPN Sitio a Sitio con OpenSwan](networking-create-virtualNetwork-site2site.md "Conexión VPN Sitio a Sitio con OpenSwan")
		- Interconectando redes virtuales
			1. Caso práctico en Azure
			2. Caso práctico con otras plataformas: AWS
		- Asignación de IPs estáticas públicas y privadas
4. [Despliegue de máquinas virtuales](virtualmachines-start.md "Despliegue de máquinas virtuales")
    - [Soporte de Linux en máquinas virtualess](virtualmachines-linux-supportedDistros.md "Soporte de Linux en máquinas virtuales") 
	- Tipos de máquinas virtuales
	- [Creación de una máquina virtual](virtualmachines-linux-create-UbuntuGnome.md "Creación de una máquina virtual") 
	- [Gestión de los puertos de la máquina](virtualmachines-create-endpoints.md "Gestión de los puertos de la máquina")
	- Grupos de afinidad
	- Grupos de disponibilidad
		- Dominios de actualización y fallo
	- Extensiones
	- Creación de imágenes personalizadas
		- [Capturar una imagen de una máquina virtual con Linux en Azure](virtualmachines-linux-create-linuxImage.md "Capturar una imagen de una máquina virtual con Linux en Azure")
		- Subir máquinas on-premise
5. Migración de máquinas a Azure
	- Caso práctico desde soluciones VMWare
	- Caso práctico desde Amazon Web Services
6. Automatización de los procesos
	- La API Rest
	- SDKs disponibles 
	- Las herramientas de CLI
	- Integración con Puppet
	- Integración con Chef
7. Evolución de IaaS a PaaS
	- Servicios cloud con Java, PHP, Python o Node.js
	- Sitios Web con despliegue continuo desde Git, GitHub, TFS.
	- Depliegue de soluciones como Wordpress, Umbraco, Joomla, Drupal.
