###### **Bosques, Dominios, Unidades Organizativas** 
Son elementos principales de la estructura de Active Directory.

* Un Bosque es el nivel organizativo mas alto dentro de un Active Directory. Cada bosque comparte un único directorio y representa un limite de seguridad.
* Los Bosques pueden contener uno o varios Dominios.
* Los Dominios pueden contener una o varias OU´s

La información de los Dominios y de las OU´s se almacena en un controlador de dominio para el dominio especifico. Este almacén se denomina Data Store.

##### **¿Qué es un Dominio?**
Es una partición lógica de objetos dentro de un Bosque que comparten configuraciones comunes de administración, seguridad y replicación. Concretamente:

* Garantiza la identidad de un usuario en toda la red
* Proporciona servicios de autenticación
* Proporciona servicios de autorización
* Permiten replicar la información entre diferentes controladores de Dominio (DC) y gestionarlos como una única unidad
* Permite la creación de relaciones de confianza

##### **¿Qué es una Unidad Organizativa (OU´s) ?**
Es un objeto contenedor que permite organizar otros objetos dentro de un dominio. Tiene tres funciones principales:

* Permite una visualización organizada de los objetos del dominio
* Agrupa diferentes objetos a los que aplicar Políticas de Grupo
* Agrupa diferentes objetos de manera que se puedan delegar permisos de gestión a otros usuarios y grupos dentro del dominio

## **Controladores de Dominio**

#### **Schema**

* Todo lo que se almacena en el Active Directory se almacena en forma de objeto. El esquema define los atributos para cada tipo de objeto.
* Se define un esquema por bosque
* Una copia del esquema reside en todos los controladores de dominio del bosque de manera que la definición de los objetos sea la misma
* El Data Store utiliza el esquema para forzar la integridad de la información
* El resultado es que todos los objetos se crean de manera uniforme, independientemente del controlador de dominio que los cree o modifique.

#### **Data Store**

*  El Data Store, normalmente referido como directorio, almacena toda la información sobre los objetos del Bosque (usuarios, grupos, equipos, dominios, ous...)
* El directorio se alacena en los controladores de dominio y puede ser accedido por aplicaciones y servicios de la red.
* Si hay mas de un controlador de dominio, cada controlador de dominio tiene una copia del directorio con toda la información del dominio.