# ¿Qué información recopilar?

* Usuarios y cuentas locales
* Grupos locales
* Usuarios y cuentas de dominio
* Equipos en el dominio
* Grupos de dominio
* OUs
* GPOs
* ACLs
* Relaciones de confianza entre bosques y dominios
* Atributos de los objetos del active directory

## Recopilación LOCAL de información

* SAM (Security Account Manager): se corresponde con una base de datos  en windows que almacena informacion sobre usuarios, grupos, contraseñas... del sistema

* La base de datos SAM se comprueba por el LSA (Local Security Authority) para autenticar el acceso de los usuarios al sistema local.

* Las contraseñas de los usuarios se almacenan hasheadas en el registro de windows (HKLM/SAM). El fichero se encuentra en %SystemRoot%/system32/config/SAM

* La SAM se puede enumerar de forma local y remotamente.
* _( A partir de la version Windows 10 1607 y Windows server 2016 esta informacion solo puede obtenerla un administrador del sistema. En versiones anteriores cualquier usuario de domino puede enumerarla)

## LDAP (Lightweight Directory Acccess Protocol)

Es un protocolo de aplicacion  que permite interactuar con servicios de directorio (como active directory) para almacenar, leer o modificar información.

Cualquier usuario de dominio puede consultar NTDS.dit utilizando LDAP, independientemente de sus privilegios.
