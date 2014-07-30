---
layout: index
---

# Índice de contenidos

1. [Introducción a Microsoft Azure](azure-whatitis "Introducción a Microsoft Azure")
2. Primeros pasos con Azure
    - [Registro](azure-register "Registro")
	- [El portal de gestión](azure-managementPortal "El portal de gestión")
	- [El portal de facturación](azure-billingPortal "El portal de facturación")
3. Elementos básicos de infraestructura:
    - [Configuración del almacenamiento](storage-start "Configuración del almacenamiento")
		- [Tipos de almacenamiento](storage-types "Tipos de almacenamiento Azure Storage") 
		- [Opciones de redundancia](storage-redundancy "Tipos de redundancia en Azure Storage")
		- [Acceso remoto a los datos almacenados](storage-remoteAccess "Acceso remoto a Azure Storage")
	- [Configuración de la red](networking-start "Configuración de la red")
	    - [Tipos de redes virtuales](networking-types "Tipos de redes virtuales")
		- [Crear de una red virtual en Azure](networking-create-virtualNetwork-cloud "Crear una red virtual en Azure")
		- Creación de redes VPN
			- [Conexión VPN Sitio a Sitio con OpenSwan / StrongSwan](networking-create-virtualNetwork-site2site "Conexión VPN Sitio a Sitio con OpenSwan")
		- Interconectando redes virtuales
			- [Caso práctico en Azure](networking-create-virtualNetwork-crossVnetAzure "Caso práctico en Azure")
			- [Caso práctico con otras plataformas: AWS](networking-create-virtualNetwork-crossVnetAWS "Caso práctico con otras plataformas: AWS")
		- [Asignación de IPs estáticas públicas y privadas](networking-ipAddressing "Asignación de IPs estáticas públicas y privadas")
4. [Despliegue de máquinas virtuales](virtualmachines-start "Despliegue de máquinas virtuales")
    - [Soporte de Linux en máquinas virtualess](virtualmachines-linux-supportedDistros "Soporte de Linux en máquinas virtuales") 
	- [Tipos de máquinas virtuales](virtualmachines-types "Tipos de máquinas virtuales")
	- [Creación de una máquina virtual](virtualmachines-linux-create-UbuntuGnome "Creación de una máquina virtual") 
	- [Gestión de los puertos de la máquina](virtualmachines-create-endpoints "Gestión de los puertos de la máquina")
	- [Grupos de afinidad](virtualmachines-affinityGroups "Grupos de afinidad")
	- [Grupos de disponibilidad](virtualmachines-availabilitySets "Grupos de disponibilidad")
	- [Extensiones](virtualmachines-extensions "Extensiones")
	- Creación de imágenes personalizadas
		- [Capturar una imagen de una máquina virtual con Linux en Azure](virtualmachines-linux-create-linuxImage "Capturar una imagen de una máquina virtual con Linux en Azure")
		- Subir máquinas on-premise
5. Migración de máquinas a Azure
	- Caso práctico desde soluciones VMWare
	- Caso práctico desde Amazon Web Services
6. Automatización de los procesos
	- [La API Rest](automation-api-rest "La API Rest")
	- [SDKs disponibles ](automation-sdks "SDKs disponibles ")
	- [Las herramientas de CLI](automation-cli "Las herramientas de CLI")
	- [Integración con Chef](automation-chef "Integración con Chef")
7. Colaboraciones
    - Carlos Milán Figuerado, *Enterprise Team Lead*, [PlainConcepts](http://enterprise.plainconcepts.com/ "PlainConcepts"): Conexión VPN Sitio a Sitio con StrongSwan
