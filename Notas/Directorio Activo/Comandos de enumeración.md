## PowerShell 
__$env:UserName__ Nombre del usuario que ejecuta la PowerShell
__$env:UserDomain__ Nombre del dominio al que pertenece el usuario
__$env:ComputerName__ Nombre del computador Windows
__whoami__ Nombre del dominio con el nombre del usuario
__Get-LocalGroup | Select Name, Objectclass, Principalsource, sid__ Nos permite saber el nombre de todos los grupos que tenemos en el equipo local
__Get-LocalGroupMember -Group Administrators__ Tener informacion sobre los administradores del dominio
__Invoke-Command -ScriptBlock { Get-LocalGroupMember -Group Administrators} -ComputerName WS02__ Podemos enumerar la informacion de los grupos locales dentro del dominio de manera remota (Debemos de tener privilegios)


## PowerView
__Get-NetLocalGroup__ Nos permite saber el nombre de todos los grupos que tenemos en el equipo local
__Get-NetGroup -UserName "empleado1" | select name__ Grupos a los que pertenece el usuario (si pertenece al grupo "Domain Users" no tiene privilegios)
__Get-NetLocalGroupMember -GroupName Administrators | Select-Object MemberName, IsGroup, IsDomain__ Tener informacion sobre los administradores del dominio
__Get-NetSession__: Se usa para enumerar las sesiones activas en un equipo remoto.
__Get-NetDomain__: Se usa para listar informacion del dominio
__Get-DomainPolicy__: Informacion relativa a la politica del dominio
__Get-NetDomainController__: Obtenemos dirección IP del servidor, y tambien su sistema operativo
__Get-NetUser__: Obtenemos la informacion de todos los usuarios que pertenecen al directorio activo
__Get-NetComputer__: Obtenemos la informacion de los equipos que están en el directorio. 
__Get-DomainGroup__: Obtenemos la información de todos los grupos del dominio
__Get-NetGroupMember -Identity "Administrators"__: Obtenemos la informacion de los usuarios que pertenecen al grupo de administradores
__Get-NetOU__: Unidades organizativas del dominio
#### _Acceso remoto_
__Get-NetLocalGroup -ComputerName WS02__: Podemos enumerar la informacion de los grupos locales dentro del dominio de manera remota (Debemos de tener privilegios)


## KALI LINUX

#### _Enumerar la SAM_
__rpcclient -U "corp\empleado1%p@ssword1" WS01.corp.local__: Nos permite iniciar sesion a una maquina remota, necesitamos nombre de dominio, usuario y contraseña.
__impacket-samrdump corp/administrator:P@ssword@WS01.corp.local__: Podemos enumerar objetos también, donde hay una cantidad de comandos dentro de samrdump
__impacket-__: Lista todos los comandos que se puedan utilizar.
__impacket-netview__: Nos permite ver las sesiones de red que se establezcan a la maquina de manera remota


## AD | NTDS.DIT
__Import-Module .\Microsoft.ActiveDirectory.Management.dll__: Como primer paso debemos importar el modulo a nuestra bash de powershell con este comando y el nombre del archivo.
__Get-ADDomain__: Nos lista información general del dominio
__Get-ADDomainController__: Obtenemos direccion IP del servidor
____Get-ADUser -Filter *___: Obtenemos la informacion de todos los usuarios que pertenecen al directorio activo. 
____Get-ADComputer -Filter *___: Obtenemos la informacion de los equipos que están en el directorio. 


