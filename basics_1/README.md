# Networking Basics 1

Este directorio contiene el ejercicio **0. Change your home IP**. Su objetivo
es configurar la resolución local de dos nombres mediante `/etc/hosts`.

## Archivo

| Archivo | Función |
| --- | --- |
| [0-change_your_home_IP](./0-change_your_home_IP) | Asigna `localhost` a `127.0.0.2` y `facebook.com` a `8.8.8.8`. |

El script comienza con `#!/usr/bin/env bash`, incluye un comentario
explicativo en la segunda línea y se registra en Git como ejecutable
(`100755`).

## Requisitos

- Ubuntu con Bash, awk y las herramientas habituales de coreutils.
- Permiso de escritura sobre `/etc/hosts` al ejecutar el ejercicio.
- ShellCheck para el análisis estático y `getent` para comprobar la resolución.

## Uso en el entorno de prácticas

Este ejercicio cambia la resolución de `localhost` y de `facebook.com`.
Ejecútalo en un contenedor o una máquina de prácticas desechable, porque esos
cambios pueden afectar a las aplicaciones que utilizan dichos nombres.

Desde `basics_1/`, en ese entorno:

```bash
sudo ./0-change_your_home_IP
getent hosts localhost
getent hosts facebook.com
```

Las direcciones esperadas son `127.0.0.2` y `8.8.8.8`, respectivamente. Si el
contenedor ya ejecuta los comandos como root, no necesitas anteponer `sudo`.

## Comportamiento

El script retira las asociaciones anteriores de esos dos nombres, incluidas
las de IPv6, y añade una sola asociación para cada uno. Conserva los demás
nombres y los comentarios del archivo. Volver a ejecutarlo no duplica las
entradas.

Primero prepara el contenido en un archivo temporal y después lo escribe
sobre `/etc/hosts`. No intenta sustituir el archivo mediante un renombrado,
lo que permite utilizarlo cuando `/etc/hosts` es un archivo montado en un
contenedor. El temporal se elimina al terminar.

El formato de las entradas y los alias está descrito en el
[manual de hosts(5)](https://man7.org/linux/man-pages/man5/hosts.5.html).

## Verificación

```bash
bash -n 0-change_your_home_IP
shellcheck 0-change_your_home_IP
```

Se comprobó la sintaxis y ShellCheck 0.9.0. También se ejecutó el script en un
espacio de montajes aislado de Ubuntu 24.04.4 bajo WSL2, con archivos de
resolución de prueba. `getent` devolvió las dos direcciones exigidas; se
comprobó la conservación de otros alias y comentarios y la ejecución
repetida sin duplicados. Los archivos originales `/etc/hosts` y
`/etc/nsswitch.conf` del equipo conservaron sus hashes.

Estas verificaciones locales no son una ejecución del checker privado de
Holberton. Este directorio documenta la tarea 0; los ejercicios de
Networking Basics 0 están en [basics_0](../basics_0/README.md).
