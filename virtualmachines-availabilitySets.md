---
layout: default
---
# Grupos de disponibilidad

Uno de los factores que debemos tener en cuenta a la hora de desplegar servicios, ya sea en la nube o en nuestras propias instalaciones, es la disponibilidad. Debemos de poner en práctica los procesos necesarios para que ante un posible fallo puntual del sistema éste pueda mantenerse en funcionamiento dando servicio.

En Azure existen dos tipos de sucesos que pueden tener un impacto directo en la disponibilidad de tus máquinas virtuales: las paradas de mantenimiento planeadas y las no planeadas.

- **Paradas de mantenimiento planeadas**: paradas realizas por Microsoft para aplicar actualizaciones dentro de la infraestructura de Azure con el objetivo de mejorar la seguridad, la disponibilidad o el rendimiento de la plataforma que aloja tus máquinas virtuales. El proceso suele realizarse de forma transparente para el usuario salvo en ocasiones en los que es necesario un reinicio de la máquina para finalizar el proceso de actualización. 

- **Paradas de mantenimiento no planeadas**: paradas provocadas por un fallo de los sitemas hardware o las instalaciones sobre las que se ejecutan tus máquinas virtuales. Estos fallos pueden producirse desde fallos a nivel local en los discos duros, a nivel de rack o fallos más generales de red o alimentación. En estos casos, la infraestructura es capaz de detectar el problema y realizar una migración automática de tu máquina virtual a otro host sin problemas. Aunque son eventos que suceden de forma extraña también pueden provocar el reinicio de tu instancia.

Azure proporciona los medios adecuados para asegurar la disponibilidad de tus servicios frente a los fallos del hardware y cumplir de esta manera el acuerdo de nivel de servicio del 99,5%. Para ello, es necesario que tus soluciones estén desplegadas en un grupo de disponibilidad y que al menos existan dos instancias que presten el mismo servicio.

El por qué de esta condicición se deba a que cada máquina virtual configurada dentro de un grupo de disponibilidad se le asigna un **Dominio de actualización** y un **Dominio de fallo.**

- **Dominio de actualización**: grupo de máquinas desplegadas dentro de Azure que pueden ser reiniciadas al mismo tiempo para aplicar una actualización. Existen cinco dominios de actualización y se asignan de forma secuencial. En el caso de que existieran más de cinco máquinas desplegadas dentro del grupo de disponibilidad habría máquinas que estarian dentro del mismo dominio de actualización.
- **Dominio de fallo**: grupo de máquinas desplegadas dentro de Azure que comparten una misma fuente de alimentación y conmutador de red. Ante un posible fallo de alguno de estos sistemas todas se verán afectadas. Por defecto, las máquinas dentro de un grupo de disponibilidad están asignadas a dos dominios de fallo diferentes.

En el caso de que se necesitara reiniciar una de las máquinas, al tener como mínimo dos situadas en diferentes dominios de actualización nos aseguramos que una continúe dando el servicio mientras la otra es reiniciada. Del mismo modo, en el caso de que por un fallo de  hardware dentro de un dominio de fallo una de las máquinas deje de estar operativa, el servicio se mantendrá con la otra instancia desplegada en un dominio de fallo diferente mientras Azure despliega una nueva máquina en un host sin problemas.

Por lo tanto, es recomendable que aplicaciones que tienen varias capas como frontales web y bases de datos, configuren cada capa dentro de un grupo de disponibilidad para asegurarse siempre al menos una instanciaa en funcionamiento. Nunca se debe de desplegar un servicio con una sola máquina virtual si la disponibilidad del mismo de forma constante es un requisito. El añadir una máquina virtual a un grupo de disponibilidad no evitará que esta se reinicie durante los periodos de mantenimiento planeados, es más, no recibirás ninguna notificación de que se va a producir dicho mantenimiento y tu instancia puede ser reiniciada.



