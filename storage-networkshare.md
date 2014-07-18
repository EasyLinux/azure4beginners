# Almacenamiento en red

Además de los servicios de almacenamiento que hemos comentado en [el apartado de tipos de almacenamiento](storagage-types.md "Tipos de almacenamiento"), actualmente se encuentra en vista previa el servicio de Azure Files.

Este nuevo servicio proporciona a las máquinas virtuales desplegadas en un datacenter montar un sistema de ficheros compartido utilizando el protocolo SMB. De esta manera, las aplicaciones que se ejecuten dentro de las máquinas virtuales podran acceder a los ficheros empleando las APIs estándar de Windows (CreateFile, ReadFile, WriteFile, etc.) de forma simultánea. Además de ello, existe una API REST que permite acceder a los ficheros de forma remota siguiendo un proceso similar a los ficheros almacenados en formato *Blob*

### Cómo crear un carpeta compartida en red

Actualmente la única forma de acceder a esta característica en vista previa es a traves de la línea de comandos. Los pasos necesarios son los siguientes:

1. Activar el servicio **Azure Files** en [el portal de servicios en Vista Previa](http://www.windowsazure.com/en-us/services/preview/ "Azure Vista Previa"). Una vez completado el registro recibirás un correo en el momento en el que tu suscripción esté habilitada para acceder al servicio.
2. [Descargar el módulo de Powershell](http://go.microsoft.com/fwlink/?LinkID=398183 "Modulo Powershell Azure Files") necesario para intereactuar con el servicio.
3. Crear una nueva cuenta de almacenamiento. No es posible realizarlo en una cuenta previamente creada ya que no dispondra del servicio activado durante el periodo de vista previa. Tampoco es posible realizarlo a través del por tal por lo que emplearemos los siguientes comandos de PowerShell.


```cs
 #Importar el módulo e inicializar un nuevo contexto de almacenamiento
 import-module .\AzureStorageFile.psd1
 $ctx=New-AzureStorageContext <account name> <account key>
  
 #Crear una nueva carpeta compartida en red
 $s = New-AzureStorageShare <share name> -Context $ctx
  
 #Crear un directorio de prueba en la carpeta compartida
 New-AzureStorageDirectory -Share $s -Path testdir
```

Podemos aprovechar para probar otras operaciones básicas como listar, crear, descargar o eliminar ficheros a través de la consola:

 
```cs
 #Subir un fichero local al directorio recién creado
 Set-AzureStorageFileContent -Share $s -Source D:\upload\testfile.txt -Path testdir
  
 #Listar los ficheros y directorios existentes
 Get-AzureStorageFile -Share $s -Path testdir
  
 #Descargar ficheros de la carpeta compartida
 Get-AzureStorageFileContent -Share $s -Path testdir/testfile.txt -Destination D:\download
  
 #Eliminar ficheros de la carpeta compartida
 Remove-AzureStorageFile -Share $s -Path testdir/testfile.txt
```

### Cómo montar la unidad compartida en nuestra máquina virtual

Una vez que hemos creado la unidad compartida a través de los pasos anteriores es posble comenzar a utilizarla desde cualquier máquina virtual que tengamos en el mismo datacenter en el que se encuentra nuestra cuenta de almacenamiento. El protocolo empleado para acceder  es SMB 2.1, para ello, emplearemos el comando **net** de la siguiente forma:

```bash
net use z: \\<account name>.file.core.windows.net\<share name> /u:<account name> <account key> 
```

Tras eso, tendremos disponible nuestra unidad en **Z:\** y podremos trabajar directamente con ella como una unidad más.

Más detalles:

- [Introducing Microsoft Azure File Service](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx "Introducción al servicio Microsoft Azure Files")
- [File Services REST API](http://msdn.microsoft.com/en-us/library/azure/dn167006.aspx "File Services REST API")