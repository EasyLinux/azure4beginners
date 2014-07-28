---
layout: default
---

#Tipos de máquinas virtuales

A la hora de trabajar con máquinas virtuales es necesario conocer los diferentes tipos disponibles y sus caracteristicas para escoger la que mejor se adecúe a las necesidades de la carga de trabajo que queremos desplegar. 

Acutalmente Azure cuenta con dos capas de servicio para máquinas virtuales a la fecha de escribir este artículo:

- Estándar: ofrece 10 tipos diferentes de máquinas virtuales que van desde la A0 hasta la A9. Cada máquina virtual tiene una configuración especifica de procesador, memoria RAM, máximo número de discos que se pueden adjuntar o máximo número de IOPs disponible. Además de las propias características de cada máquina, también cuenta con los servicios del balanceador de carga y de autoescalado dentro del precio por hora.  
- Básica: ofrece 5 configuraciones únicamente que coincide con las caracteristicas de rendimiento de las máquinas A0 - A4. Su principal diferencia con la capa estándar es que estas máquinas no incluyen los servicios del balanceador de carga y de autoescalado por lo que su precio es inferior. De esta manera, aquellas aplicaciones que no necesiten de estos serivicios pueden beneficiarse de un ahorro en costes.

En ambos casos la **facturación se realiza por minuto** por lo que realmente el pago se realiza únicamente por el uso real que realizamos. La información de las características y los precios se encuentra disponible en [el portal de precios de Azure](https://azure.microsoft.com/es-es/pricing/details/virtual-machines/ "el portal de precios de Azure"). A continuación tienes un resumen de las configuraciones disponibles:

#### Instancias de uso general

Enfocadas a aplicaciones de uso general como aplicaciones web, que no requieren altas necesidades de memoria RAM o procesamiento.

| Nombre de la instancia | Procesadores virtuales | Memoria |
|:----------------------:|:----------------------:|:-------:|
| A0                     | Compartido             | 768 MB  |
| A1                     | 1                      | 1.75 GB |
| A2                     | 2                      | 3.50 GB |
| A3                     | 2                      | 7.00 GB |
| A4                     | 4                      | 14.00 GB|

#### Instancias de memoria intensiva

Proporcionan más cantidad de memoria, óptima para ejecutar aplicaciones de alto rendimiento, como las bases de datos o granjas de servidores de SharePoint

| Nombre de la instancia | Procesadores virtuales | Memoria |
|:----------------------:|:----------------------:|:-------:|
| A5                     | 2                      | 14 GB   |
| A6                     | 4                      | 28 GB   |
| A7                     | 8                      | 56 GB   |


#### Instancias de proceso intensivo

Proporcionan procesadores más rápidos, interconexión más rápida, más procesadores virtuales para una capacidad de proceso superior y más memoria. Estas instancias incluyen una red InfiniBand adicional de 40 Gbits por segundo que proporciona tecnología de acceso directo a memoria remota (RDMA) para la máxima eficacia de las aplicaciones MPI en paralelo. Esta combinación de características hace que estas instancias sean óptimas para la ejecución de aplicaciones con un uso intensivo de recursos de proceso y red, como las aplicaciones de clústeres de alto rendimiento o las aplicaciones de modelado, simulación y análisis, codificación de vídeo, etc.


| Nombre de la instancia | Procesadores virtuales | Memoria |
|:----------------------:|:----------------------:|:-------:|
| A8                     | 8                      | 56 GB   |
| A9                     | 16                     | 112 GB  |
