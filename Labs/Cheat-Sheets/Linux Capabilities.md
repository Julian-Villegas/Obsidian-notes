```bash
getcap -r / 2>/dev/null
```

### Desglose parte por parte

- **`getcap`**: Es la herramienta nativa de Linux (_Get Capabilities_) que sirve para listar qué capacidades especiales tienen asignadas los archivos ejecutables.
    
- **`-r`**: Significa _recursive_ (recursivo). Le dice al comando que busque dentro de la carpeta actual y en todas las subcarpetas que existan de ahí hacia abajo.
    
- **`/`**: Es el directorio raíz de Linux. Al ponerlo junto al `-r`, le estás diciendo: _"busca en absolutamente todo el sistema de archivos"_.
    
- **`2>/dev/null`**: Esta es una redirección de errores clásica en Linux.
    
    - El número `2` representa el canal de "Errores Estándar" (_stderr_).
        
    - El símbolo `>` redirige ese canal.
        
    - `/dev/null` es el "hoyo negro" de Linux (un dispositivo virtual que borra todo lo que recibe).
        
    - **En resumen:** Como eres un usuario común (`nathan`), no tienes permiso para leer muchas carpetas del sistema. Si no pones esto, tu pantalla se llenará de miles de líneas diciendo _"Permiso denegado"_. Esto las oculta y solo te muestra los resultados exitosos.


## El comando para listar procesos activos (`ps`)

Si quieres auditar qué programas o scripts se están ejecutando en tiempo real en la máquina (por si hay alguna tarea programada de root mal configurada), usas:

```bash
ps aux
```
### Desglose parte por parte:

- **`ps`**: Viene de _Process Status_ (Estado de los Procesos). Muestra una instantánea de los procesos que se están ejecutando en el sistema.
    
- **`a`**: Le dice a `ps` que muestre los procesos de **todos los usuarios**, no solo los del usuario `nathan` con el que iniciaste sesión.
    
- **`u`**: Modifica el formato de salida para mostrarlo en "formato de usuario", lo que incluye detalles muy útiles como el nombre del usuario que corre el proceso, el uso de CPU y el uso de memoria.
    
- **`x`**: Incluye en la lista los procesos que no están asociados a una terminal de control (como los demonios del sistema, servicios en segundo plano o tareas automáticas).