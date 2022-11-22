---
title: "Unidad 1: Sistemas de Control de Versiones"
date: "2022-11-22"
description: "Introducción a los sistemas de control de versiones y uso de Git."
tags: ["despliegue"]
---

## Introducción

### Definición:

> Un sistema de control de versiones es un registro de los cambios realizados sobre un archivo o conjunto de archivos a lo largo del tiempo, permitiendo recuperar versiones específicas en cualquier momento.

### Sistemas de control de versiones locales

Un ejemplo es copiar los archivos a otro directorio manualmente, es muy propenso a errores.

### Sistemas de control de versiones centralizados

Se desarrollaron para permitir colaborar con otros desarrolladores desde diferentes equipos. Estos sitemas tienen un único servidor que contiene todos los archivos versionados para que los clientes puedan acceder a ellos. Durante años éste ha sido el estándar para el control de versiones.

Ofrece muchas ventajas frente al control de versiones locales, como el poder saber en qué está trabajando cada colaborador del proyecto.

La principal desventaja de esta configuración es que dependemos del servidor en todo momento, ya que si éste se cae durante una hora, nadie podrá colaborar o guardar cambios versionados, o que si el disco duro que contiene la base de datos central se corrompe y no hay copias de seguridad, todo el trabajo se pierde.

### Sistemas de control de versiones distribuidos

En estos sistemas los clientes no solo descargan la última instantánea de los archivos, sino que también replican por completo el repositorio. De este modo, si un servidor muere, cualquiera de los repositorios de los clientes puede copiarse sobre él para restaurarlo. *(Cada vez que se hace una instantánea, se hace una copia de seguridad de todos los datos).*

## GIT

### Definición:

Git es un sistema de control de versiones distribuído eficiente, rápido, fácil de usar y que cuenta con un increíble sistema de ramificación para desarrollo no lineal.

A diferencia de otros sistemas de control de versiones, almacena copias instantáneas en lugar de diferencias. De este modo cada vez que confirmamos un cambio o almacenamos el estado de un proyecto en Git, hace una captura del estado de todos los archivos en ese momento y guarda una referencia a esa instantánea.

### Casi todas las operaciones son locales

La mayoría de operaciones en Git solo necesitan archivos y recursos locales para oeprar. Al almacenar toda la historia del proyecto en el disco local, no necesita consultar a ningún servidor para obtenerla y mostrarla.

### Integridad

Es imposible modificar el contenido de cualquier archivo o directorio sin que Git lo sepa gracias a que integra el sistema de comprobación **checksum** al más bajo nivel.

### Solo añade información

Prácticamente todos los cambios se pueden deshacer en Git, y más si actualizamos el repositorio con regularidad, ya que todas las acciones añaden información a su base de datos.

### Estados de los archivos

Git tiene tres estados principales en los que se pueden encontrar tus archivos:

- **Commited** _(Confirmado)_: los datos están almacenados en la base de datos **local**.
- **Modified** _(Modificado)_: el archivo ha sido modificado pero los cambios no están confirmados  en la base de datos local.
- **Staged** _(Preparado)_: el archivo modificado ha sido marcado en su versión actual para ser confirmado en el **push**.

Las tres secciones principales de un proyecto de Git son:

- **Directorio de Git** _(Repositorio)_: es donde se almacenan los metadatos y la base de datos con los objetos para el proyecto. Es la parte más importante, y es lo que se copia cuando clonas un repositorio externo.
- **Directorio de trabajo**: es una copia de una versión del proyecto, la cual se extrae de la base de datos comprimida en el directorio de Git.
- **Área de preparación**: es un fichero contenido en el directorio de Git por lo general, que almacena la información sobre lo que va a ser confirmado en el siguiente **push**.

El flujo de trabajo en Git es el siguiente:

1. **Modificas archivos** en tu **directorio de trabajo**.
2. **Preparas** los archivos añadiéndolos al **área de preparación**.
3. **Confirmas los cambios**, almacenando instantáneas de manera permanente en tu **directorio de Git**.

Si una versión concreta de un archivo está almacenada en el **directorio de Git**, se considera **confirmada** _(commited)_. Si ha sufrido cambios desde que se obtuvo la última versión del repositorio pero ha sido añadido al **área de preparación**, está **preparado** _(staged)_, y si ha sido modificado desde que se obtuvo la última versión del repositorio pero no se ha preparado, está **modificado** _(modified)_.

## Flujo de trabajo en Git

### Configuración inicial

Cuando instalamos Git debemos **establecer un nombre de usuario y una dirección de correo**. Para ello ejecutaremos los siguientes comandos:

`$ git config --global user.name "Nombre Usuario"`
`$ git config --global user.email "usuario@gmail.com"`

Con el parámetro `--global`, especificamos que Git utilice esta información para todo lo que hagamos en el sistema. Para modificar esta información en un proyecto concreto, podemos ejecutar de nuevo el comando sin este parámetro.

Podemos escoger el editor que se utilizará cuando tengamos que introducir un mensaje. Por defecto Git utilizará el editor predeterminado en el sistema *(generalmente Vi)*. Para **cambiar de editor** podemos ejecutar los siguientes comandos, dependiendo de nuestro gusto:

~~~
$ git config --global core.editor nano
$ git config --global core.editor gedit
$ git config --global core.editor vscode --wait
$ git config --global core.editor emacs
~~~

Para **listar las propiedades que Git ha configurado** empleamos el siguiete comando:

`$ git config --list`

Si necesitamos leer el manual de algún comando de Git tenemos diferentes opciones:

~~~
$ git help <comando>
$ git <comando> --help
$ man git-<comando>
~~~

### Comandos básicos

Estos son los **comandos básicos de Git** necesarios para hacer la gran mayoría de acciones trabajando con Git:

Hay dos formas para obtener un repositorio Git:

1. **Tomar un proyecto o directorio existente e importarlo en Git** 

Para ello, nos situamos en el directorio raíz del proyecto que queremos importar y ejecutamos:

`$ git init`

Se creará un subdirectorio llamado .git con los archivos necesarios y de este modo, habremos **inicializado un repositorio local**.

2. **Clonar un repositorio existente desde otro servidor**

Para obtener una copia de un repositorio Git existente almacenado en un servidor para, por ejemplo, contribuir en él, necesitaremos el siguiente comando:

`$ git clone https://github.com/repositorio`

Al ejecutarlo, obtendremos un nuevo directorio con una **copia idéntica de casi todos los datos contenidos en el servidor**, incluídas **todas las versiones de los archivos** en la historia del proyecto.

Si queremos **especificar un nombre** para el directorio de destino, añadiremos al final de la sentencia el nombre del mismo:

`$ git clone https://github.com/repositorio Proyecto`

En lugar de utilizar transferencias por `https://` podemos emplear el protocolo `git://` o ssh mediante la sintaxis `usuario@servidor:ruta/repositorio.git`

Para **confirmar los cambios** que hemos realizado y confirmar sus instantáneas en la base de datos local de Git