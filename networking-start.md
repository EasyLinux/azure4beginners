# Redes virtuales en Azure

A la hora de trabajar con nuestras máquinas virtuales o servicios en la nube, es necesario que estos tengan asociada una red en la que se desplieguen y podamos interactuar con ellos. Por defecto, Azure los despliega usando una configuración propia de red y les asigna dos direcciones IPv4:

- **Dirección IP Privada Virtual** *(VIP)*: dirección aleatoria asociada al servicio dentro de los rangos públicos asignados por Microsoft a Azure. Es la dirección que emplearemos para acceder al recurso a traves de Internet. 
- **Dirección IP Interna**: dirección aleatoria asociada al servicio dentro del rango interno de direccionamiento del datacenter. Se usa para la comunicación interna entre servicios o con la propia infraestructura de Azure. No es direccionable desde el exterior.

Para algunas aplicaciones es posible que con la configuración por defecto de red que proporciona Azure sea suficiente; sin embargo, es posible que en otras ocasiones deseemos tener un mayor control de la configuración de red. 

Es en este punto donde entran en juego las **redes virtuales** de Microsoft Azure que te permiten que:

- **Crear un rango privado de direccionamiento** en Azure entre los disponibles (10.x, 172.x, 192.x)
- **Configurar la conectividad de estas redes virtuales con nuestra propia red empresarial** a través de una conexión IPSec VPN. De esta manera es posible extener nuestra red hasta Azure y trabajar con los recursos disponibles de forma similar a como si estuvieran en nuestro propio datacenter.
- **Configurar un DNS propio** para la resolución de nombres dentro de la red virtual. Es posible que ese DNS se encuentre dentro de Azure o en un servidor propio en nuestro datacenter.

Gracias a ello logramos beneficios extras que la configuración por defecto no nos permite asegurar:

- Mejoras en la seguridad y aislamiento de nuestros recursos: el uso de un red virtual añade una capa extra de protección ya que solo las máquina virtuales o servicios que formen parte de la misma red pueden acceder los unos a los otros de forma directa. 
- Uso de una IP privada de forma persistente: si desplegamos nuestros servicios dentro de la red virtual podremos tener un direccionamiento interno estable. Azure asigna una IP del rango configurado a través de DHCP con una cesión infinita a las máquinas; sin embargo, también es posible asignar de forma manual IPs especificas para evitar que cambien cuando el servicio es parado o desaprovisionado.
- Conectividad a Internet: a pesar de configurar una red privada virtual, los servicios dentro de ella podran continuar accediendo a Internet a través de la dirección *VIP* asingada al servicio. 

Una vez vistos los principios básicos de la configuración de red puedes continuar con los siguientes apartados o guías. 

### Siguientes apartados: 


### Guías paso a paso 
- [Crear de una red virtual solo en Azure](networking-create-virtualNetwork-cloud.md "Crear una red virtual solo en Azure") 
