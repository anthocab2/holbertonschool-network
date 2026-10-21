# Networking Basics 0

Introducción a los conceptos de redes y a dos herramientas de diagnóstico en
Linux. El módulo cubre el modelo OSI, las redes LAN y WAN, Internet, las
direcciones MAC e IP y los protocolos de transporte TCP y UDP.

TCP proporciona un flujo de bytes fiable y ordenado. UDP no garantiza por sí
solo la entrega ni el orden de los datagramas; la comparación de velocidad
entre ambos depende de la aplicación y de las condiciones de la red.

## Archivos

| Archivo | Propósito |
| --- | --- |
| [0-OSI_model](./0-OSI_model) | Respuestas sobre el modelo OSI y el orden de sus capas. |
| [1-types_of_network](./1-types_of_network) | Respuestas sobre redes locales, redes de área amplia e Internet. |
| [2-MAC_and_IP_address](./2-MAC_and_IP_address) | Respuestas sobre direcciones MAC e IP. |
| [3-UDP_and_TCP](./3-UDP_and_TCP) | Respuestas sobre las características de TCP y UDP. |
| [4-TCP_and_UDP_ports](./4-TCP_and_UDP_ports) | Consulta sockets en escucha y muestra información de procesos. |
| [5-is_the_host_on_the_network](./5-is_the_host_on_the_network) | Solicita cinco envíos de ping al destino indicado. |

Los archivos 0–3 contienen únicamente respuestas numéricas, una por línea,
según el orden de las opciones del cuestionario. Los archivos 4 y 5 son
scripts Bash ejecutables sin extensión. Todos utilizan finales de línea LF
y terminan con un salto de línea.

## Requisitos

- Ubuntu 22.04 como entorno objetivo.
- Bash y `/usr/bin/env`.
- `net-tools`, que proporciona `netstat`.
- `iputils-ping`, que proporciona `ping`.
- `shellcheck` para el análisis estático de los scripts.

Instalación de las herramientas en Ubuntu:

```bash
sudo apt-get update
sudo apt-get install net-tools iputils-ping shellcheck
```

## Uso

Ejecuta los siguientes comandos desde `basics_0/`.

### Consultar sockets

```bash
./4-TCP_and_UDP_ports
```

El script ejecuta `netstat -lpn`: `-l` selecciona sockets en escucha, `-p`
solicita el PID y el nombre del programa y `-n` evita resolver nombres para
las direcciones y los puertos. También pueden aparecer sockets de dominio
Unix. Los puertos y procesos mostrados dependen del equipo; algunos datos de
procesos pueden no estar disponibles con los permisos del usuario actual.
El script conserva la salida y los diagnósticos de `netstat`.

### Comprobar un destino

```bash
./5-is_the_host_on_the_network 127.0.0.1
```

El primer argumento se pasa a `ping -c 5`. Se conserva la salida de ping y su
código de salida. El comando solicita cinco envíos; los errores de destino,
permisos o red pueden impedir que se completen o que lleguen respuestas.
La ausencia de respuesta no demuestra por sí sola que un equipo esté apagado:
puede existir un filtro de ICMP.

Sin argumentos:

```bash
./5-is_the_host_on_the_network
```

El script muestra exactamente esta línea y no ejecuta `ping`:

```text
Usage: 5-is_the_host_on_the_network {IP_ADDRESS}
```

## Verificación

Comprueba la sintaxis y ejecuta ShellCheck:

```bash
bash -n 4-TCP_and_UDP_ports
bash -n 5-is_the_host_on_the_network
shellcheck 4-TCP_and_UDP_ports 5-is_the_host_on_the_network
```

Para comprobar el comportamiento en el sistema:

```bash
./4-TCP_and_UDP_ports
./5-is_the_host_on_the_network
./5-is_the_host_on_the_network 127.0.0.1
```

Los dos scripts se registran en Git con modo `100755`; las respuestas y los
README usan `100644`. Puedes comprobarlo con `git ls-files --stage`.
Estas comprobaciones locales no equivalen al checker privado de Holberton.

### Resultados locales

Se verificaron los contenidos exactos de las respuestas 0–3, los nombres de
archivo, los finales LF, los shebangs y los comentarios de los scripts.
Ambas comprobaciones con `bash -n` y ShellCheck finalizaron sin errores.
`netstat` se ejecutó correctamente; el caso sin argumentos mostró el mensaje
esperado sin invocar ping, y la prueba real de loopback transmitió y recibió
cinco paquetes. Los comandos terminaron con código de salida 0.

Estas ejecuciones se realizaron en **Ubuntu 24.04.4 bajo WSL2**, con Bash
5.2.21, ShellCheck 0.9.0 y ping de iputils 20240117. El entorno objetivo del
encargo sigue siendo Ubuntu 22.04; no se ejecutaron pruebas en esa versión
ni se accedió al checker privado.
