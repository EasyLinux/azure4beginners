---
layout: default
---

#Extensiones para máquinas virtuales

Las extensiones son componentes que proporcionan una funcionalidad extra a la máquina virtual que se esta ejecutando. Es posible añadir una extensión durante el proceso de creación de la máquina o cuando ya se está ejecutando. Por defecto, se instala la extensión *VMAgent* que es necesario para la interacción de la máquina virtual con el host de Azure.

Además de ella, a dia de hoy tenemos disponible:

- **Chef**: extensión que permite desplegar directamente el último cliente Chef con los ficheros necesarios de configuración RB y PEM. De esta manera, nada más iniciar la máquina se conectará de forma directa al servidor de Chef sin necesidad de iniciar sesión o configurar alguna caracteristica extra. 

    Para configurarla únicamente es necesario seleccionar el checkbox disponible en el último paso de creación de una nueva máquina virtual. Tras ello, se nos pedirán los ficheros **Client.RB** y **Validation Key (PEM)**. Se los proporcionamos subiéndolos desde nuestro ordenador local o desde una cuenta de almacenamiento en Azure y finalizamos el asistentes. 

![Menú nuevo](../images/virtualmachines-extensions-Step1.png)
