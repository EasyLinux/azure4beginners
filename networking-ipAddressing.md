---
layout: default
---

# Asignación de IPs estáticas públicas y privadas

Azure por defecto asigna las IPs a los servicios cloud y las máquinas virtuales seleccionándolas de forma aleatoria del conjunto de ellas que tiene disponibles. A la hora de conectar a nuestros servicios podemos utilizar dichas IPs, *Virtual IP (VIP),* de forma directa o la URL del servicio. Sin embargo, el primero es un escenario desaconsejado ya que si el servicio es parado o reiniciado la IP puede verse modificada.

Hay escenarios en los que por motivos de negocio se debe de mantener la IP siempre de forma permanente; por ejemplo, para autorizar las conexiones a ella a través del firewall y la situación anterior no es deseable. Para ello, Azure ofrece actualmente en vista previa los servicios de reserva de IP. Hay que diferenciar tres tipos de direcciones:

- Direcciones públicas reservadas a nivel de servicio
- Direcciones públicas reservadas a nivel de instancia
- Direcciones privadas estáticas reservadas

A continuación entraremos en detalle en ellas:

### Direcciones públicas reservadas a nivel de servicio

Las direcciones públicas reservadas, conocidas también como *Reserved Ip (RIP)*, permiten reservar una VIP y asignarla directamente al servicio que deseemos. A diferencia del caso general, las RIPs permanencen asociadas hasta que le indiquemos lo contrario por lo que parando o reiniciando no provocará su pérdida. 

Escenarios en los que puede resultar interesante el uso de RIP son por ejemplo:

- Si tus servicios necesitan usar direcciones IPs que no cambien bajo ninguna circunstancia hasta que no lo decidas. Aunque el servicio sea eliminado la IP sigue asignada a tu suscripción por lo que puede ser reutilizada.

- Si necesitas garantizar que todo el tráfico saliente desde Azure emplee una IP predecible. De esta forma es posible configurar el firewall para permir el tráfico desde y hacia ellas.


Es posible realizar la reserva de una IP de una región en particular. Azure de forma transparente completará la solicitud y la asignará a tu suscipción. A partir de ese momento será posible emplear esa IP en cualquier de los servicios en la nube desplegados en dicha región.


A dia de hoy es posible configurar el uso de RIPs a traves de la API REST o con comandos en PowerShell.

- Más detalles: [Reserved IP Addresses](http://msdn.microsoft.com/en-us/library/azure/dn690120.aspx "Reserved IP Addresses")

### Direcciones públicas reservadas a nivel de instancia

Las direcciones públicas reservas a nivel de instancia, conocidas también como *Public IP (PIP)*, permiten asignar una dirección directamente a una máquina virtual concreta en lugar de al servicio en la nube dentro del que se aloja. Esto no quiere decir que la IP pública asignada al servicio en la nube se vea modificada, al contrario, es una IP extra que permite establecer una conexión directa con esa máquina especifica. Es posible asignar una PIP por máquina virtual hasta un máximo de dos por suscripción durante el periodo de vista previa.

Escenarios en los que puede resultar interesante el uso de PIP son por ejemplo:

- FTP Pasivo: si empleamos una PIP para una máquina virtual que tiene un servidor FTP pasivo ejecutándose dentro de ella podemos establecer conexiones directamente con él evitando la necesidad de abrir un extremo específico para ello dado que en modo pasivo los puertos se eligen de forma dinámica.
- Trafico saliente: el tráfico saliente que tiene como origen una máquina virtual configurada con una PIP emplea dicha dirección como origen por lo que es posible identificar esa máquina de forma unívoca frente a servicios externos.

A dia de hoy es posible configurar el uso de PIPs a traves de la API REST o con comandos en PowerShell.

- Más detalles: [Instance-Level Public IP Addresses](http://msdn.microsoft.com/en-us/library/azure/dn690118.aspx "Instance-Level Public IP Addresses")

### Direcciones privadas estáticas reservadas

En los casos anteriores hemos estado cubriendo los escenarios en los que es importante disponer de una IP pública fija para nuestro servicio en la nube o nuestra instancia. Sin embargo, hay escenarios en los que nos interesa disponer de una IP fija asignada a una máquina como puede ser la base de datos, el servidor de log, etc.

De forma predefinida, las máquinas virtuales reciben una IP interna del rango configurado en la red virtual creada previamente. Esta asignación se realiza a traves de DHCP con un periodo de cesión lo suficientemente largo que podria definirse como una IP estática; sin embargo, si la máquina es parada puede perder dicha IP interna. Si necesitamos que esto no suceda deberemos asignar una IP interna estática. 

A dia de hoy es posible configurar el uso de direcciones privadas estáticas a traves de la API REST o con comandos en PowerShell.

- Más detalles: [Configure a Static Internal IP Address (DIP) for a VM](http://msdn.microsoft.com/en-us/library/azure/dn630228.aspx "Configure a Static Internal IP Address for a VM")


