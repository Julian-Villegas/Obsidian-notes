*Tecnologías*

*Nginx versión: 1.24.0*

---

## Write-up

El dia 28/06/2026 me encuentro realizando la maquina ENIGMA, la cual está basada en linux y está categorizada como una maquina "facil"

Empezamos con nuestro escaneo habitual, esta vez añadiendo la flag de *--min-rate 5000* para que el escaneo vaya mas rapido.

```bash
sudo nmap -p- -sV -sC -sS --min-rate 5000 --open -Pn -n 10.129.33.250 -oN scan.txt
Starting Nmap 7.99 ( https://nmap.org ) at 2026-06-28 19:55 -0500
Nmap scan report for 10.129.33.250
Host is up (0.16s latency).
Not shown: 65490 closed tcp ports (reset), 32 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT      STATE SERVICE  VERSION
22/tcp    open  ssh      OpenSSH 9.6p1 Ubuntu 3ubuntu13.16 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 0c:4b:d2:76:ab:10:06:92:05:dc:f7:55:94:7f:18:df (ECDSA)
|_  256 2d:6d:4a:4c:ee:2e:11:b6:c8:90:e6:83:e9:df:38:b0 (ED25519)
80/tcp    open  http     nginx 1.24.0 (Ubuntu)
|_http-server-header: nginx/1.24.0 (Ubuntu)
|_http-title: Did not follow redirect to http://enigma.htb/
110/tcp   open  pop3     Dovecot pop3d
| ssl-cert: Subject: commonName=enigma
| Subject Alternative Name: DNS:enigma
| Not valid before: 2026-02-18T20:33:33
|_Not valid after:  2036-02-16T20:33:33
|_ssl-date: TLS randomness does not represent time
|_pop3-capabilities: CAPA UIDL PIPELINING RESP-CODES SASL TOP STLS AUTH-RESP-CODE
111/tcp   open  rpcbind  2-4 (RPC #100000)
|_rpcinfo: ERROR: Script execution failed (use -d to debug)
143/tcp   open  imap     Dovecot imapd (Ubuntu)
|_imap-capabilities: more SASL-IR IMAP4rev1 ID capabilities ENABLE OK STARTTLS Pre-login post-login listed LOGIN-REFERRALS LITERAL+ LOGINDISABLEDA0001 IDLE have
| ssl-cert: Subject: commonName=enigma
| Subject Alternative Name: DNS:enigma
| Not valid before: 2026-02-18T20:33:33
|_Not valid after:  2036-02-16T20:33:33
|_ssl-date: TLS randomness does not represent time
993/tcp   open  ssl/imap Dovecot imapd (Ubuntu)
| ssl-cert: Subject: commonName=enigma
| Subject Alternative Name: DNS:enigma
| Not valid before: 2026-02-18T20:33:33
|_Not valid after:  2036-02-16T20:33:33
|_imap-capabilities: more AUTH=PLAINA0001 IMAP4rev1 capabilities ENABLE OK SASL-IR LOGIN-REFERRALS post-login listed ID LITERAL+ Pre-login IDLE have
|_ssl-date: TLS randomness does not represent time
995/tcp   open  ssl/pop3 Dovecot pop3d
|_ssl-date: TLS randomness does not represent time
|_pop3-capabilities: CAPA UIDL PIPELINING RESP-CODES SASL(PLAIN) TOP USER AUTH-RESP-CODE
| ssl-cert: Subject: commonName=enigma
| Subject Alternative Name: DNS:enigma
| Not valid before: 2026-02-18T20:33:33
|_Not valid after:  2036-02-16T20:33:33
2049/tcp  open  nfs      3-4 (RPC #100003)
36049/tcp open  status   1 (RPC #100024)
39087/tcp open  nlockmgr 1-4 (RPC #100021)
40865/tcp open  mountd   1-3 (RPC #100005)
47573/tcp open  mountd   1-3 (RPC #100005)
49337/tcp open  mountd   1-3 (RPC #100005)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 38.09 seconds
```

Analizando los servicios nos damos cuenta que si hacemos portforwarding a la direccion ip hacia el puerto 80, nos redirige a un dominio llamado *enigma.htb* el problema es que le navegador no logra encontrar dicho dominio, lo que nos fuerza a que tengamos que analizar los demas puertos.

Pude darme cuenta que en el puerto *2049* podemos observar que corre un servicio *nfs (network file system)*  este servicio permite compartir carpetas alojadas en el servidor con demas usuarios utilizando una comunicacion cliente-servidor, investigando más sobre este puerto y servicio podemos enumerar una información bastante importante con el siguiente comando:

```bash
showmount -e <DireccionIP>
/srv/nfs/onboarding *
```

La salida de este comando nos permite ver la carpeta que el servidor está compartiendo, y para poder montar el disco o carpeta que contiene la informacion que se está compartiendo debemos ejecutar el siguiente comando:

```bash
sudo mount -t nfs <DireccionIP>:/srv/nfs/onboarding /ruta/local
```

Luego de la ejecución de este comando podemos revisar la ruta local para ver los archivos que se están compartiendo:

```bash
ls -l /mnt/htb/New_Employee_Access.pdf
.rw-r--r-- root root 1.7 KB Thu Feb 19 14:53:57 2026  /mnt/htb/New_Employee_Access.pdf
```

Viendo los permisos nos damos cuenta que solo root puede leer y modificarlo.

Cuando abrimos el pdf con la herramienta *pdftotext* podremos ver la información alojada en el pdf.

```bash
pdftotext New_Employee_Access.pdf - | less
```

Guardo la informacion en un archivo llamado *crendeciales.txt*:

```bash
cat credenciales.txt
Employee: 
Kevin Mitchell 
Webmail Access 
URL:http://mail001.enigma.htb 
Username: kevin 
Password: Enigma2024!
```

Mientras avanzaba me dí cuenta que la solucion para que el navegador encontrara el dominio era editar el archivo de /etc/hosts para que pueda buscarlo y encontrar el sitio:

```bash
sudo nano /etc/hosts

10.129.33.250 enigma.htb mail001.enigma.htb
```

Gracias a las credenciales obtenidas pudimos acceder al 