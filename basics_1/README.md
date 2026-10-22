# Networking Basics 1

Ejercicios para configurar la resolución local de nombres, mostrar las
direcciones IPv4 activas y recibir texto por una conexión TCP en localhost.

## Archivos

| Archivo | Función |
| --- | --- |
| [0-change_your_home_IP](./0-change_your_home_IP) | Asigna `localhost` a `127.0.0.2` y `facebook.com` a `8.8.8.8`. |
| [1-show_attached_IPs](./1-show_attached_IPs) | Muestra una IPv4 por línea para las interfaces activas, incluida loopback. |
| [2-port_listening_on_localhost](./2-port_listening_on_localhost) | Escucha en el puerto TCP 98 de localhost y muestra el texto recibido. |

Los scripts comienzan con `#!/usr/bin/env bash`, incluyen un comentario
explicativo en la segunda línea y se registran en Git como ejecutables
(`100755`). Usan finales de línea LF y terminan con un salto de línea.

## Requisitos

- Ubuntu con Bash, awk y las herramientas habituales de coreutils.
- `iproute2` para el comando `ip` y `netcat-openbsd` para `nc`.
- Permiso de escritura sobre `/etc/hosts` para la tarea 0 y permiso para
  escuchar en el puerto 98 para la tarea 2.
- ShellCheck para el análisis estático y `getent` para comprobar la resolución.

```bash
sudo apt-get update
sudo apt-get install iproute2 netcat-openbsd shellcheck
```

Ejecuta los ejemplos siguientes desde `basics_1/`.

## 0. Change your home IP

Este ejercicio cambia la resolución de `localhost` y de `facebook.com`.
Ejecútalo en un contenedor o una máquina de prácticas desechable, porque esos
cambios pueden afectar a las aplicaciones que utilizan dichos nombres.

```bash
sudo ./0-change_your_home_IP
getent hosts localhost
getent hosts facebook.com
```

Las direcciones esperadas son `127.0.0.2` y `8.8.8.8`, respectivamente. Si el
contenedor ya ejecuta los comandos como root, no necesitas anteponer `sudo`.

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

## 1. Show attached IPs

```bash
./1-show_attached_IPs | cat -e
```

Ejemplo de salida:

```text
127.0.0.1$
10.0.2.15$
```

Las direcciones y su orden dependen del equipo. `ip -o -4 addr show up`
selecciona las direcciones IPv4 de interfaces habilitadas; awk elimina el
prefijo de red y muestra una dirección por línea. Incluye loopback y las
direcciones secundarias. No muestra IPv6 ni direcciones de interfaces
deshabilitadas. Los signos `$` del ejemplo los añade `cat -e` para señalar
el final de cada línea.

Cambiar la resolución del nombre `localhost` en la tarea 0 no cambia las
direcciones asignadas a la interfaz loopback.

## 2. Port listening on localhost

En la primera terminal:

```bash
sudo ./2-port_listening_on_localhost
```

En otra terminal del mismo entorno, conecta y escribe unas líneas:

```bash
nc localhost 98
```

```text
Hello world
test
```

Si tienes instalado telnet, también puedes usar `telnet localhost 98`, como
en el enunciado. El texto enviado aparece en la primera terminal.

El script usa `nc -4 -l localhost 98`: escucha TCP sobre la dirección IPv4
que resuelve `localhost`. Funciona con `127.0.0.1` y con `127.0.0.2` después
de la tarea 0. No muestra mensajes adicionales ni escucha en las interfaces
externas. Atiende una conexión y termina cuando el cliente la cierra;
`Ctrl+C` permite detenerlo desde la terminal.

## Verificación

```bash
bash -n 0-change_your_home_IP
bash -n 1-show_attached_IPs
bash -n 2-port_listening_on_localhost
shellcheck 0-change_your_home_IP 1-show_attached_IPs 2-port_listening_on_localhost
```

Los tres scripts pasaron las comprobaciones de sintaxis y ShellCheck 0.9.0
en Ubuntu 24.04.4 bajo WSL2. Las pruebas funcionales se ejecutaron en espacios
de montajes o red aislados:

- Tarea 0: `getent` devolvió las dos direcciones exigidas; se conservaron los
  demás alias y comentarios, y la ejecución repetida no añadió duplicados.
- Tarea 1: mostró exactamente las IPv4 de loopback y de una interfaz activa
  con dos direcciones. Excluyó IPv6 y una interfaz deshabilitada.
- Tarea 2: recibió exactamente `Hello world` y `test`, en líneas separadas,
  por conexiones TCP reales al puerto 98. Se comprobó que escuchaba solo
  en la dirección de localhost, tanto con `127.0.0.1` como con `127.0.0.2`.

Los archivos originales `/etc/hosts` y `/etc/nsswitch.conf` conservaron sus
hashes, y las direcciones de las interfaces del equipo no cambiaron.

Estas verificaciones locales no son una ejecución del checker privado de
Holberton. Los ejercicios de Networking Basics 0 están en
[basics_0](../basics_0/README.md).
