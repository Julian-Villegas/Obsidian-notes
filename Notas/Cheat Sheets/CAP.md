
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
```

