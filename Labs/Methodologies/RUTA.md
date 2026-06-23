
---

## 🛠️ Fase 1: Reconocimiento Activo (Antes de atacar)

El objetivo aquí es descubrir la superficie de ataque sin romper nada todavía.

### El Paso a Paso:

1. **El Escaneo Inicial (Rápido):** No lances un escaneo lento con scripts al principio. Primero descubre qué puertos están abiertos lo más rápido posible.
* *Tip de HTB:* Usa `--min-rate 5000` para forzar a Nmap a enviar paquetes rápido.


2. **El Escaneo de Versiones (Profundo):** Una vez que sepas qué puertos específicos están abiertos (por ejemplo: 22, 80, 443), lanza un escaneo solo a esos puertos para identificar qué software y qué versión exacta corre ahí.
3. **Fuga de Información en Web:** Si el puerto 80 o 443 está abierto, entra al navegador, pero **revisa siempre el código fuente** (`Ctrl + U`).
* *Tip de HTB:* Busca comentarios de los desarrolladores, tecnologías utilizadas o rutas en las tecnologías de la página.



> 💡 **Tip de Oro:** Si ves un dominio como `empresa.htb` en los certificados SSL o en la página, agrégalo de inmediato a tu archivo `/etc/hosts` en tu máquina Linux para que tu navegador pueda resolver los subdominios.

---

## 💻 Fase 2: Enumeración Enfocada (El embudo)

Aquí es donde aplicas el Módulo 6 (OWASP) o Módulo 5 (Redes). Eliges un puerto del paso anterior y lo exprimes.

### El Paso a Paso:

1. **Si es un servicio Web:** Lanza de inmediato una herramienta de descubrimiento de directorios ocultos (como `Gobuster` o `Feroxbuster`). Busca carpetas como `/admin`, `/backups`, `/uploads` o archivos `.php`, `.txt`, `.config`.
2. **Si es un servicio de Red (SMB/FTP):** Intenta conectarte de forma anónima. ¿Puedes listar carpetas compartidas? ¿Hay algún archivo de texto con notas de los administradores?
3. **Búsqueda de Vulnerabilidades Conocidas:** Si encontraste una versión de software (ej. `Apache Tomcat 9.0.xx`), búscala en Google o en tu terminal usando `searchsploit Apache Tomcat 9.0`.

> 💡 **Tip de Oro:** No saltes de un puerto a otro como loco. Agota las opciones de un servicio antes de moverte al siguiente. Si hay una web, audita sus formularios: prueba inyecciones SQL básicas, mira si puedes subir archivos o si manipulas la URL para causar un LFI.

---

## 🚀 Fase 3: Explotación e Intrusión Inicial (El Hackeo)

El momento de ejecutar el exploit para ganar acceso al sistema (conseguir la flag `user.txt`).

### El Paso a Paso:

1. **Entender el Exploit:** Si descargas un script de internet (Python o Bash), **léelo antes de ejecutarlo**. Asegúrate de cambiar los parámetros de la IP de la víctima (`RHOST`) y tu IP de la VPN (`LHOST`).
2. **Preparar tu Escucha:** Antes de lanzar cualquier exploit que te devuelva una consola (*Reverse Shell*), pon a escuchar tu Netcat en tu máquina Linux (`nc -lvnp <PUERTO>`).
3. **Estabilizar la Consola:** Cuando entres a la máquina víctima, notarás que la terminal es fea, no puedes usar las flechas del teclado ni hacer `Ctrl + C` sin cerrarla. Estabilízala de inmediato.
* *Tip de HTB:* Ejecuta `script /dev/null -c bash`, luego presiona `Ctrl + Z`, escribe `stty raw -echo; fg` y presiona Enter dos veces. Tendrás una terminal perfecta.



---

## 👑 Fase 4: Post-Explotación y Escalada de Privilegios (Después del acceso)

Ya eres un usuario de bajo rango (como `www-data`). Ahora necesitas ser el administrador absoluto (`root` o `SYSTEM`).

### El Paso a Paso:

1. **Enumeración Interna Rápida:** No intentes adivinar. Ejecuta comandos básicos para ver qué está pasando dentro de la máquina:
* ¿Qué comandos puedo ejecutar como root sin contraseña? (`sudo -l`).
* ¿Qué tareas se ejecutan solas en el fondo? (Revisar los archivos *Cron jobs*).
* ¿Hay archivos con permisos SUID? (`find / -perm -u=s -type f 2>/dev/null`).


2. **Automatizar si es necesario:** Si no encuentras nada obvio a mano, transfiere un script de enumeración automática como `LinPEAS.sh` (para Linux) o `WinPEAS.exe` (para Windows) desde tu máquina virtual y revísalo.
3. **El Asalto Final:** Busca en sitios web como *GTFOBins* (para Linux) cómo aprovechar los binarios mal configurados que encontraste para spawnear una shell como `root`.

---

