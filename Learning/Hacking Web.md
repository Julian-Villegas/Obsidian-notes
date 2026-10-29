---
tags: [ciberseguridad, hacking-web, htb, portswigger, owasp, ruta-aprendizaje]
tipo: temario
nivel: principiante-a-intermedio
creado: 2026-10-28
estado: en-progreso
---

# 🎓 Temario de Hacking Web — Ruta Autónoma

> **Para qué es esto:** una ruta ordenada, estilo universidad, para pasar de "me estanco en las máquinas de HTB porque no sé hacking web" a tener una base sólida de web pentesting. Está pensada para estudiar a tu ritmo: cada vez que tengas un rato, abres el módulo que sigue y avanzas.
>
> **Cómo usarlo:** los módulos van en orden. No saltes de módulo hasta cerrar los `[ ]` del anterior. Cada módulo tiene: **objetivo**, **conceptos clave** (con definiciones), **recursos** (video + lectura + laboratorio) y una **mini-evaluación** para comprobar que de verdad lo entendiste.
>
> **Recursos en inglés** (tu preferencia): los autores elegidos son todos angloparlantes.

---

## 📖 Antes de empezar: definiciones base

Si vas a estudiar hacking web, conviene tener claros unos términos que se repetirán en todos los módulos. No te saltes esta sección aunque quieras ir directo a lo entretenido.

- **Hacking web (web application pentesting):** buscar y explotar fallos de seguridad en aplicaciones web (las que corren en un navegador y hablan con un servidor). Es el tipo de ataque más común en las máquinas de HTB porque casi toda aplicación moderna tiene una cara web.
- **Vulnerabilidad:** un fallo en el diseño o la implementación de un sistema que permite a un atacante hacer algo que no debería (leer datos ajenos, ejecutar comandos, saltarse el login, etc.).
- **Exploit:** la técnica o el código concreto que aprovecha una vulnerabilidad.
- **Payload:** el dato malicioso que envías para disparar la vulnerabilidad (por ejemplo, un fragmento de JavaScript en un campo de texto).
- **OWASP (Open Worldwide Application Security Project):** organización sin ánimo de lucro que publica material de referencia sobre seguridad web. Su documento estrella es el **OWASP Top 10**.
- **OWASP Top 10:** lista, actualizada cada pocos años, de las 10 categorías de riesgo más críticas en aplicaciones web. Es el "mapa" del territorio: no es un curso, sino la taxonomía que todos usan.
- **Proxy de intercepción:** una herramienta que se pone entre tu navegador y el servidor para que puedas ver y modificar cada petición HTTP antes de que llegue. La herramienta estándar es **Burp Suite**.
- **Laboratorio (lab):** un entorno controlado, legal y deliberadamente vulnerable donde practicar sin meterte en problemas. **Nunca** se practica sobre sistemas ajenos sin permiso escrito.

---

## 🗺️ Mapa de recursos (tus tres pilares)

Esta ruta se apoya en tres fuentes principales, todas en inglés. Entiende qué aporta cada una:

| Recurso                                       | Qué es                                                                                                    | Coste                                                | Para qué lo usas aquí                                                                                           |
| --------------------------------------------- | --------------------------------------------------------------------------------------------------------- | ---------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| **PortSwigger Web Security Academy**          | Plataforma gratuita de los creadores de Burp Suite. Teoría + laboratorios interactivos reales online.     | Gratis                                               | Es la **columna vertebral** de la ruta. Lees la teoría y haces los labs.                                        |
| **OWASP Top 10 (2025)**                       | Documento de referencia con las 10 categorías de riesgo.                                                  | Gratis                                               | Es el **mapa**. Lo lees al inicio para entender el panorama; vuelves a él para situar cada tema.                |
| **HTB Academy — Web Penetration Tester path** | Ruta estructurada de Hack The Box (la que antes se llamaba *Bug Bounty Hunter* y lleva al cert **CWES**). | De pago (plan Student ~$10/mes con correo académico) | **Opcional pero recomendado** más adelante: te da metodología de pentester y práctica con objetivos sin pistas. |

> **Nota sobre el canal de YouTube clave:** **Rana Khalil** tiene una serie en la que resuelve los laboratorios de PortSwigger uno por uno, explicando el porqué. La técnica de oro es: intenta el lab tú primero, y si te atascas, ves su video de ese lab. No veas el video antes de intentarlo.

---

## 🧭 Estructura de la ruta

La ruta tiene **4 fases**. Cada fase agrupa varios módulos.

- **Fase 0 — Fundamentos:** cómo funciona la web por dentro y cómo manejar Burp. Sin esto, lo demás no se entiende.
- **Fase 1 — Vulnerabilidades del lado del servidor (server-side):** las más fáciles de entender para empezar. Solo tienes que razonar qué pasa en el servidor.
- **Fase 2 — Vulnerabilidades del lado del cliente (client-side):** añaden la capa del navegador. Un pelín más complejas.
- **Fase 3 — Temas avanzados y metodología:** para cuando ya tienes base. Aquí se encadena todo y se piensa como pentester.

> **Tiempo estimado:** no te pongas plazos rígidos. Como referencia realista estudiando a ratos: Fase 0 (~1-2 semanas), Fase 1 (~4-6 semanas), Fase 2 (~3-4 semanas), Fase 3 (en adelante). Lo importante es la constancia, no la velocidad.

---

# 🟢 FASE 0 — Fundamentos

## Módulo 0.1 — Cómo funciona la web (HTTP, peticiones y respuestas)

**Objetivo:** entender qué viaja entre tu navegador y el servidor. Todo el hacking web es manipular ese intercambio.

**Conceptos clave:**
- **HTTP (HyperText Transfer Protocol):** el "idioma" con el que el navegador pide cosas al servidor y este responde. Cada acción (abrir una página, enviar un formulario) es una **petición** HTTP que recibe una **respuesta**.
- **Petición (request):** tiene un **método** (GET para pedir datos, POST para enviarlos), una **URL**, **cabeceras** (headers: metadatos como el tipo de navegador o las cookies) y a veces un **cuerpo** (body: los datos que mandas).
- **Respuesta (response):** trae un **código de estado** (200 = OK, 404 = no encontrado, 302 = redirección, 500 = error del servidor), cabeceras y el contenido (HTML, JSON, etc.).
- **Cookie:** un pequeño dato que el servidor le da al navegador para reconocerte en peticiones siguientes (p. ej., para mantener tu sesión iniciada).
- **Parámetro:** un valor que envías al servidor, ya sea en la URL (`?id=5`) o en el cuerpo. **Casi toda vulnerabilidad web entra por un parámetro.**

**Recursos:**
- 📄 Lectura: PortSwigger — *HTTP request smuggling* no, empieza por lo básico → https://portswigger.net/web-security/all-topics (familiarízate con el panel). Para los fundamentos de HTTP, el módulo **Web Requests** de HTB Academy es excelente si ya tienes la suscripción.
- 🎥 Video: busca en YouTube "HTTP explained for hackers" o la introducción de la serie de **Rana Khalil** → https://www.youtube.com/@RanaKhalil101
- 🧪 Práctica: por ahora solo explora. Abre las DevTools de tu navegador (tecla F12), pestaña **Network**, y mira las peticiones que se generan al navegar.

**Mini-evaluación:**
- [x] Sé explicar con mis palabras la diferencia entre GET y POST.
- [x] Sé qué significan los códigos 200, 302, 403, 404 y 500.
- [x] Sé dónde viven las cookies en una petición y para qué sirven.
- [x] Puedo abrir las DevTools y encontrar la petición que se envía al pulsar un botón.

---

## Módulo 0.2 — Burp Suite (tu herramienta principal)

**Objetivo:** aprender a interceptar, modificar y reenviar peticiones. Esto es el 80% del trabajo manual en hacking web.

**Conceptos clave:**
- **Burp Suite:** la suite de herramientas estándar para probar seguridad web. La versión **Community** (gratis) es suficiente para toda esta ruta; la **Professional** solo hace falta para el examen BSCP más adelante.
- **Proxy:** el módulo de Burp que intercepta el tráfico. Configuras tu navegador para que pase por Burp y así puedes ver/editar cada petición.
- **Intercept:** el botón que pausa una petición para que la modifiques antes de que siga al servidor.
- **Repeater:** la herramienta para reenviar una misma petición muchas veces, cambiándola cada vez. **Es la que más vas a usar.** Pruebas un payload, ves la respuesta, ajustas, repites.
- **Intruder:** automatiza el envío de muchas peticiones con valores distintos (útil para fuerza bruta o fuzzing). En la versión Community va limitada en velocidad.
- **Decoder / Inspector:** para codificar y descodificar datos (Base64, URL-encoding, etc.).

**Recursos:**
- 📄 Lectura: PortSwigger tiene tutoriales oficiales de Burp → https://portswigger.net/web-security/getting-started (sección "Find your way around Burp Suite").
- 🎥 Video: los tutoriales oficiales en video de PortSwigger sobre interceptar peticiones y usar Repeater están enlazados en esa misma página.
- 🧪 Práctica: instala Burp Suite Community (https://portswigger.net/burp/communitydownload), configura el navegador y intercepta tu primera petición. Reenvíala con Repeater cambiando un valor.

**Mini-evaluación:**
- [ ] Tengo Burp Community instalado y mi navegador configurado para pasar por él.
- [ ] Puedo interceptar una petición, modificarla y dejarla pasar.
- [ ] Puedo mandar una petición a Repeater y reenviarla con cambios.
- [ ] Entiendo para qué sirve Intruder (aunque no lo domine aún).

---

## Módulo 0.3 — El mapa: OWASP Top 10 (2025)

**Objetivo:** tener una vista de pájaro de los grandes tipos de riesgo antes de meterte en cada uno. No memorices; solo familiarízate.

**Conceptos clave — las 10 categorías del OWASP Top 10:2025** (versión final, publicada en enero de 2026):

1. **A01 — Broken Access Control (control de acceso roto):** cuando un usuario puede acceder a datos o acciones que no le corresponden. La #1 por frecuencia.
2. **A02 — Security Misconfiguration (configuración de seguridad deficiente):** servidores, frameworks o permisos mal configurados.
3. **A03 — Software Supply Chain Failures (fallos en la cadena de suministro):** riesgos por librerías y dependencias de terceros. *(Categoría nueva/ampliada en 2025.)*
4. **A04 — Cryptographic Failures (fallos criptográficos):** mal uso o ausencia de cifrado, que expone datos sensibles.
5. **A05 — Injection (inyección):** meter código o comandos maliciosos donde la app espera datos (incluye SQL injection y XSS).
6. **A06 — Insecure Design (diseño inseguro):** fallos de raíz en cómo se diseñó la aplicación, no solo en el código.
7. **A07 — Authentication Failures (fallos de autenticación):** debilidades en el login, gestión de sesiones, contraseñas.
8. **A08 — Software or Data Integrity Failures (fallos de integridad):** confiar en código o datos sin verificar que no fueron manipulados.
9. **A09 — Security Logging and Alerting Failures (fallos de registro y alertas):** no detectar ni registrar ataques.
10. **A10 — Mishandling of Exceptional Conditions (manejo incorrecto de condiciones excepcionales):** errores al gestionar situaciones inesperadas. *(Categoría nueva en 2025.)*

**Recursos:**
- 📄 Lectura: el documento oficial → https://owasp.org/Top10/2025/ (hay versión en español en https://owasp.org/Top10/2025/es/, pero por tu preferencia léelo en inglés). Lee la intro y pasa por cada categoría sin agobiarte.

**Mini-evaluación:**
- [ ] Puedo nombrar al menos 6 de las 10 categorías sin mirar.
- [ ] Entiendo que "Injection" engloba tanto SQLi como XSS.
- [ ] Sé que el Top 10 es un mapa de referencia, no un curso paso a paso.

---

# 🔵 FASE 1 — Vulnerabilidades del lado del servidor (server-side)

> PortSwigger recomienda empezar por estas porque solo tienes que razonar qué pasa en el servidor. Son la base. Para cada módulo: **lee la teoría de PortSwigger → haz los labs de nivel Apprentice → si te atascas, ve el video de Rana Khalil de ese lab.**

## Módulo 1.1 — SQL Injection (SQLi)

**Objetivo:** el tema estrella para empezar. Aprender a manipular las consultas que la app hace a su base de datos.

**Conceptos clave:**
- **SQL (Structured Query Language):** el lenguaje con el que las aplicaciones consultan su base de datos.
- **SQL Injection:** cuando logras insertar tu propio SQL en un parámetro, la app lo ejecuta como si fuera suyo. Puedes leer datos ajenos, saltarte logins o incluso tomar control.
- **UNION-based injection:** técnica para añadir una consulta propia y recuperar datos de otras tablas.
- **Blind injection:** cuando no ves el resultado directamente, pero lo deduces por el comportamiento (respuestas distintas, tiempos de espera).

**Recursos:**
- 📄 Teoría + 🧪 labs: https://portswigger.net/web-security/sql-injection (18 labs; haz los Apprentice y Practitioner)
- 🎥 Video de apoyo: serie de Rana Khalil sobre SQLi (de las más elogiadas de su canal).

**Mini-evaluación:**
- [ ] Completé los labs Apprentice de SQLi.
- [ ] Entiendo cómo se hace un login bypass con SQLi.
- [ ] Hice al menos un lab de UNION-based y uno de blind.

---

## Módulo 1.2 — Authentication (autenticación)

**Objetivo:** romper logins mal implementados. Muy común en máquinas HTB.

**Conceptos clave:**
- **Autenticación:** el proceso de verificar que eres quien dices (login).
- **Fuerza bruta (brute force):** probar muchas contraseñas hasta dar con la correcta.
- **Enumeración de usuarios:** descubrir qué nombres de usuario existen por diferencias en las respuestas.
- **MFA (Multi-Factor Authentication):** segundo factor (código, app). Veremos cómo a veces se puede saltar.

**Recursos:**
- 📄 Teoría + 🧪 labs: https://portswigger.net/web-security/authentication (14 labs)
- 🎥 Rana Khalil tiene videos de esta sección.

**Mini-evaluación:**
- [ ] Completé los labs Apprentice de autenticación.
- [ ] Sé enumerar usuarios por diferencias en las respuestas.
- [ ] Entiendo cómo abusar de una recuperación de contraseña mal hecha.

---

## Módulo 1.3 — Path Traversal (recorrido de directorios)

**Objetivo:** leer archivos del servidor que no deberías poder ver.

**Conceptos clave:**
- **Path traversal (o directory traversal):** manipular una ruta de archivo (con secuencias como `../`) para salir de la carpeta permitida y leer archivos del sistema (como `/etc/passwd`).
- **Filtro / sanitización:** los intentos de la app por bloquear estos ataques, que a veces se pueden evadir.

**Recursos:**
- 📄 Teoría + 🧪 labs: https://portswigger.net/web-security/file-path-traversal (6 labs)

**Mini-evaluación:**
- [ ] Completé los labs de path traversal.
- [ ] Sé al menos dos formas de evadir filtros básicos.

---

## Módulo 1.4 — Command Injection (inyección de comandos)

**Objetivo:** ejecutar comandos del sistema operativo a través de la app. Lleva directo a tomar control del servidor.

**Conceptos clave:**
- **OS command injection:** cuando la app pasa tu input a un comando del sistema sin filtrarlo, y logras ejecutar tus propios comandos.
- **Blind command injection:** cuando no ves la salida del comando, pero confirmas que se ejecutó (por tiempos, por conexiones de red que provocas).

**Recursos:**
- 📄 Teoría + 🧪 labs: https://portswigger.net/web-security/os-command-injection (5 labs)

**Mini-evaluación:**
- [ ] Completé los labs de command injection.
- [ ] Sé qué caracteres sirven para encadenar comandos (`;`, `&&`, `|`).
- [ ] Entiendo la versión "blind" y cómo confirmarla.

---

## Módulo 1.5 — Access Control (control de acceso)

**Objetivo:** acceder a cosas que no te corresponden. Es la **A01 del OWASP**, la más frecuente del mundo real.

**Conceptos clave:**
- **Control de acceso:** las reglas que deciden quién puede hacer/ver qué.
- **IDOR (Insecure Direct Object Reference):** cuando cambias un identificador (`?user_id=123` → `124`) y accedes a datos de otro usuario.
- **Escalada de privilegios:** pasar de usuario normal a administrador.

**Recursos:**
- 📄 Teoría + 🧪 labs: https://portswigger.net/web-security/access-control (13 labs)

**Mini-evaluación:**
- [ ] Completé los labs Apprentice de access control.
- [ ] Sé qué es un IDOR y cómo detectarlo.
- [ ] Hice un lab de escalada de privilegios.

---

## Módulo 1.6 — Information Disclosure (divulgación de información)

**Objetivo:** encontrar datos que la app filtra sin querer (y que ayudan a otros ataques).

**Conceptos clave:**
- **Information disclosure:** cuando la app revela datos sensibles sin querer: mensajes de error con detalles internos, archivos de backup, comentarios en el código, rutas del sistema.
- **Reconocimiento (recon):** fase inicial de recolectar toda la información posible del objetivo.

**Recursos:**
- 📄 Teoría + 🧪 labs: https://portswigger.net/web-security/information-disclosure (5 labs)

**Mini-evaluación:**
- [ ] Completé los labs de information disclosure.
- [ ] Sé buscar información filtrada en errores, comentarios y archivos expuestos.

---

## Módulo 1.7 — Business Logic Vulnerabilities (fallos de lógica de negocio)

**Objetivo:** abusar de fallos en cómo está pensada la aplicación, no en el código. Requiere creatividad.

**Conceptos clave:**
- **Lógica de negocio:** las reglas propias de la aplicación (p. ej., "no puedes comprar más stock del que hay").
- **Fallo de lógica:** cuando esas reglas se pueden saltar con flujos inesperados (comprar con precio negativo, aplicar un descuento dos veces, etc.).

**Recursos:**
- 📄 Teoría + 🧪 labs: https://portswigger.net/web-security/logic-flaws (11 labs)

**Mini-evaluación:**
- [ ] Completé los labs de business logic.
- [ ] Puedo explicar un ejemplo de fallo de lógica con mis palabras.

---

## Módulo 1.8 — File Upload Vulnerabilities (subida de archivos)

**Objetivo:** subir archivos maliciosos para ejecutar código en el servidor.

**Conceptos clave:**
- **File upload vulnerability:** cuando puedes subir un archivo peligroso (como un script) y hacer que el servidor lo ejecute.
- **Web shell:** un script que subes y que te da una interfaz para ejecutar comandos en el servidor.
- **Evasión de filtros:** trucos para saltarte las comprobaciones de tipo de archivo.

**Recursos:**
- 📄 Teoría + 🧪 labs: https://portswigger.net/web-security/file-upload (7 labs)

**Mini-evaluación:**
- [ ] Completé los labs de file upload.
- [ ] Sé qué es una web shell y cómo se sube.
- [ ] Conozco un par de técnicas de evasión de filtros de extensión.

---

## Módulo 1.9 — SSRF (Server-Side Request Forgery)

**Objetivo:** hacer que el servidor haga peticiones por ti a sitios que tú no alcanzas. Es la puerta a redes internas.

**Conceptos clave:**
- **SSRF:** cuando logras que el servidor haga una petición a una URL que tú controlas, incluyendo recursos internos que desde fuera no verías (bases de datos internas, servicios en la nube).

**Recursos:**
- 📄 Teoría + 🧪 labs: https://portswigger.net/web-security/ssrf (7 labs)

**Mini-evaluación:**
- [ ] Completé los labs Apprentice de SSRF.
- [ ] Entiendo por qué SSRF es peligroso en entornos cloud.

---

## Módulo 1.10 — XXE Injection (XML External Entity)

**Objetivo:** abusar de cómo la app procesa XML para leer archivos o provocar SSRF.

**Conceptos clave:**
- **XML:** un formato de datos que algunas apps usan para intercambiar información.
- **XXE:** cuando el procesador de XML permite definir "entidades externas" y las abusas para leer archivos del servidor o lanzar peticiones internas.

**Recursos:**
- 📄 Teoría + 🧪 labs: https://portswigger.net/web-security/xxe (9 labs)

**Mini-evaluación:**
- [ ] Completé los labs de XXE.
- [ ] Sé leer un archivo del servidor vía XXE.

---

# 🟣 FASE 2 — Vulnerabilidades del lado del cliente (client-side)

> Estas añaden la capa del navegador: el ataque afecta a otros usuarios, no solo al servidor. Un poco más abstractas, pero con la base de la Fase 1 las entenderás bien.

## Módulo 2.1 — Cross-Site Scripting (XSS)

**Objetivo:** el tema cliente más importante. Inyectar JavaScript que se ejecuta en el navegador de otras víctimas.

**Conceptos clave:**
- **XSS:** inyectar código JavaScript en una página para que se ejecute en el navegador de quien la visita. Permite robar sesiones, suplantar al usuario, etc.
- **Reflected XSS:** el payload viaja en la petición y se "refleja" de vuelta en la respuesta (se dispara al abrir un enlace manipulado).
- **Stored XSS:** el payload queda guardado en el servidor (p. ej., un comentario) y afecta a todo el que vea esa página.
- **DOM-based XSS:** el fallo está en el JavaScript del propio navegador, no en el servidor.

**Recursos:**
- 📄 Teoría + 🧪 labs: https://portswigger.net/web-security/cross-site-scripting (30 labs; es grande, haz Apprentice y Practitioner)
- 🎥 Rana Khalil tiene una serie extensa de XSS.

**Mini-evaluación:**
- [ ] Completé los labs Apprentice de XSS.
- [ ] Distingo reflected, stored y DOM-based.
- [ ] Logré un payload que roba una cookie en un lab.

---

## Módulo 2.2 — CSRF (Cross-Site Request Forgery)

**Objetivo:** engañar al navegador de una víctima para que haga acciones sin querer.

**Conceptos clave:**
- **CSRF:** forzar al navegador de una víctima autenticada a enviar una petición que ella no quería (cambiar su email, hacer una transferencia), aprovechando que sus cookies viajan solas.
- **Token anti-CSRF:** la defensa habitual; un valor secreto que la app exige en cada acción sensible.

**Recursos:**
- 📄 Teoría + 🧪 labs: https://portswigger.net/web-security/csrf (12 labs)

**Mini-evaluación:**
- [ ] Completé los labs Apprentice de CSRF.
- [ ] Entiendo cómo un token anti-CSRF detiene el ataque y cómo a veces se evade.

---

## Módulo 2.3 — CORS (Cross-Origin Resource Sharing)

**Objetivo:** entender y abusar de configuraciones laxas de intercambio entre dominios.

**Conceptos clave:**
- **Origin (origen):** la combinación de protocolo + dominio + puerto. El navegador aísla orígenes distintos por seguridad (Same-Origin Policy).
- **CORS:** el mecanismo que relaja esa regla para permitir que un sitio consuma datos de otro. Mal configurado, deja que un atacante lea datos de la víctima.

**Recursos:**
- 📄 Teoría + 🧪 labs: https://portswigger.net/web-security/cors (3 labs)

**Mini-evaluación:**
- [ ] Completé los labs de CORS.
- [ ] Sé qué es una Same-Origin Policy.

---

## Módulo 2.4 — Clickjacking

**Objetivo:** entender ataques que engañan al usuario a hacer clic donde no cree.

**Conceptos clave:**
- **Clickjacking:** superponer de forma invisible una página sobre otra para que la víctima haga clic en algo distinto de lo que ve.

**Recursos:**
- 📄 Teoría + 🧪 labs: https://portswigger.net/web-security/clickjacking (5 labs)

**Mini-evaluación:**
- [ ] Completé los labs de clickjacking.

---

# 🔴 FASE 3 — Temas avanzados y metodología

> A partir de aquí ya tienes base. Estos temas requieren más contexto y se encadenan unos con otros. No es necesario hacerlos todos de golpe: elige según lo que te vayas encontrando en las máquinas de HTB.

## Módulo 3.1 — Metodología de pentesting web

**Objetivo:** dejar de atacar "a lo loco" y seguir un proceso. Esto es lo que diferencia a un pentester de alguien que solo resuelve labs.

**Conceptos clave:**
- **Metodología:** un orden de trabajo repetible: reconocimiento → mapeo de la aplicación → identificación de vulnerabilidades → explotación → reporte.
- **Enumeración:** listar todo lo que ofrece el objetivo (rutas, parámetros, tecnologías, subdominios).
- **Fuzzing:** enviar muchas entradas automáticas para descubrir rutas o parámetros ocultos.

**Recursos:**
- 📄 Lectura de referencia: HackTricks — *Pentesting Web Methodology* → https://book.hacktricks.wiki (es una wiki enorme; úsala como enciclopedia de consulta, no para leer de corrido).
- 📄 Si tienes HTB Academy: el módulo **Information Gathering - Web Edition** y **Web Fuzzing** cubren esto muy bien.

**Mini-evaluación:**
- [ ] Puedo describir las fases de un pentest web en orden.
- [ ] Sé usar una herramienta de fuzzing (ffuf o gobuster) para descubrir rutas.

---

## Módulo 3.2 — Temas avanzados de PortSwigger (a demanda)

**Objetivo:** cubrir vulnerabilidades más especializadas conforme te las encuentres.

Estos los haces "a demanda" (cuando aparezcan en una máquina o cuando quieras profundizar). Todos tienen teoría + labs en PortSwigger:

- **Insecure Deserialization** → https://portswigger.net/web-security/deserialization
- **Server-Side Template Injection (SSTI)** → https://portswigger.net/web-security/server-side-template-injection
- **JWT Attacks** (ataques a tokens de sesión) → https://portswigger.net/web-security/jwt
- **OAuth Authentication** → https://portswigger.net/web-security/oauth
- **GraphQL API Vulnerabilities** → https://portswigger.net/web-security/graphql
- **API Testing** → https://portswigger.net/web-security/api-testing
- **HTTP Host Header Attacks** → https://portswigger.net/web-security/host-header
- **Web Cache Poisoning** → https://portswigger.net/web-security/web-cache-poisoning
- **HTTP Request Smuggling** (avanzado de verdad) → https://portswigger.net/web-security/request-smuggling

**Mini-evaluación:**
- [ ] Hice al menos uno de estos temas por curiosidad propia.

---

## Módulo 3.3 — Aplicar todo en HTB (cerrar el círculo)

**Objetivo:** volver a las máquinas de HTB (tu motivación inicial) con las herramientas para no atascarte.

**Cómo hacerlo:**
- Retoma las máquinas **fáciles** con foco web y aplica la metodología del Módulo 3.1.
- Cuando te atasques en una máquina ya resuelta por la comunidad, usa **IppSec**: su buscador https://ippsec.rocks indexa cientos de horas de video por técnica. Buscas la máquina o la técnica y te lleva al minuto exacto.
- Si te decides por la ruta de HTB Academy, el **Web Penetration Tester path** (cert **CWES**) es la continuación natural: te da metodología formal y objetivos sin pistas. Con correo académico el plan Student cuesta ~$10/mes.

**Mini-evaluación:**
- [ ] Resolví una máquina HTB con foco web aplicando metodología, sin ver el writeup completo.
- [ ] Sé usar ippsec.rocks para desatascarme en un punto concreto.

---

# 🎯 Certificaciones (metas a futuro)

No son obligatorias, pero dan un objetivo concreto y validan tu aprendizaje de cara a un empleo:

- **BSCP — Burp Suite Certified Practitioner** (PortSwigger): el cierre natural de esta ruta. Examen práctico de 4 horas. Cuesta ~$99, pero necesitas Burp Suite Professional activo durante el examen. Prerrequisito realista: completar todos los labs Apprentice y Practitioner de PortSwigger. Es de las certs con mejor relación calidad-precio.
- **CWES — HTB Certified Web Exploitation Specialist** (Hack The Box): antes se llamaba CBBH. Requiere completar el 100% del *Web Penetration Tester path*. Examen de 7 días con reporte profesional. Examen ~$210.

---

# 📌 Resumen de la ruta (checklist maestro)

- [ ] **Fase 0** — Fundamentos (HTTP, Burp, OWASP Top 10)
- [ ] **Fase 1** — Server-side (SQLi, Auth, Path Traversal, Command Injection, Access Control, Info Disclosure, Business Logic, File Upload, SSRF, XXE)
- [ ] **Fase 2** — Client-side (XSS, CSRF, CORS, Clickjacking)
- [ ] **Fase 3** — Metodología, temas avanzados y vuelta a HTB
- [ ] **Meta** — BSCP y/o CWES

---

## 🔗 Enlaces maestros (para tener a mano)

- PortSwigger — todos los temas: https://portswigger.net/web-security/all-topics
- PortSwigger — learning paths guiados: https://portswigger.net/web-security/learning-paths
- OWASP Top 10 (2025): https://owasp.org/Top10/2025/
- Rana Khalil (YouTube): https://www.youtube.com/@RanaKhalil101
- IppSec (buscador por técnica): https://ippsec.rocks
- HackTricks (wiki de consulta): https://book.hacktricks.wiki
- HTB Academy: https://academy.hackthebox.com
