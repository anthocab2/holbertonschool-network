# Holberton School — Network

Ejercicios de fundamentos de redes de Holberton School. El repositorio reúne
respuestas conceptuales y scripts Bash para practicar las herramientas de
diagnóstico de Linux.

## Módulos

| Directorio | Contenido | Documentación |
| --- | --- | --- |
| [basics_0](./basics_0/) | Modelo OSI, tipos de red, direcciones MAC e IP, TCP, UDP, sockets y conectividad. | [README del módulo](./basics_0/README.md) |
| [basics_1](./basics_1/) | Resolución local de nombres, direcciones IPv4 activas y escucha TCP en localhost. | [README del módulo](./basics_1/README.md) |

## Estructura

```text
holbertonschool-network/
├── README.md
├── basics_0/
│   ├── README.md
│   ├── 0-OSI_model
│   ├── 1-types_of_network
│   ├── 2-MAC_and_IP_address
│   ├── 3-UDP_and_TCP
│   ├── 4-TCP_and_UDP_ports
│   └── 5-is_the_host_on_the_network
└── basics_1/
    ├── README.md
    ├── 0-change_your_home_IP
    ├── 1-show_attached_IPs
    └── 2-port_listening_on_localhost
```

En `basics_0`, los archivos 0–3 contienen las respuestas numéricas del
cuestionario y los archivos 4 y 5 son scripts Bash ejecutables, sin extensión.
En `basics_1`, los tres scripts configuran la resolución local de nombres,
muestran las IPv4 activas y reciben texto por TCP en el puerto 98 de localhost.

## Objetivos de aprendizaje

- Identificar las capas del modelo OSI y distinguir LAN, WAN e Internet.
- Comprender la función de las direcciones MAC e IP.
- Reconocer las características de TCP y UDP.
- Consultar sockets en escucha y comprobar respuestas ICMP con herramientas
  de Linux.
- Configurar asociaciones entre nombres e IP en un entorno de prácticas.
- Enumerar las direcciones IPv4 activas y recibir texto mediante netcat.

## Primeros pasos

El entorno objetivo es **Ubuntu 22.04** con Bash y Git. Instala `net-tools`
para disponer de `netstat` e `ifconfig`, `iputils-ping` para utilizar `ping`,
`netcat-openbsd` para escuchar conexiones TCP y
`shellcheck` para analizar los scripts:

```bash
sudo apt-get update
sudo apt-get install net-tools iputils-ping netcat-openbsd shellcheck
```

Clona el repositorio y entra en el módulo:

```bash
git clone https://github.com/anthocab2/holbertonschool-network.git
cd holbertonschool-network/basics_0
```

## Uso

Para las tareas **Change your home IP**, **Show attached IPs** y **Port
listening on localhost**, consulta las instrucciones de
[basics_1](./basics_1/README.md). La tarea 0 modifica `/etc/hosts` y se ejecuta
en un entorno de prácticas aislado.

Desde `basics_0/`, consulta los sockets en escucha y su información de proceso:

```bash
./4-TCP_and_UDP_ports
```

Solicita cinco envíos de ping a la interfaz de loopback:

```bash
./5-is_the_host_on_the_network 127.0.0.1
```

Si ejecutas el segundo script sin argumentos, muestra el siguiente mensaje
y no envía solicitudes:

```text
Usage: 5-is_the_host_on_the_network {IP_ADDRESS}
```

Los puertos, procesos y tiempos de respuesta dependen del sistema. Algunos
datos de procesos pueden requerir permisos adicionales para ser visibles.

## Verificación

Desde `basics_0/`, comprueba la sintaxis y ejecuta el análisis estático:

```bash
bash -n 4-TCP_and_UDP_ports
bash -n 5-is_the_host_on_the_network
shellcheck 4-TCP_and_UDP_ports 5-is_the_host_on_the_network
```

Los scripts se guardan en Git con permiso ejecutable (`100755`); las
respuestas y los README usan `100644`. Todos los archivos de texto utilizan
finales de línea LF y terminan con un salto de línea.

La validación local incluyó sintaxis Bash, ShellCheck, ejecución real de
`netstat`, el caso sin argumentos y cinco paquetes de ping enviados y
recibidos en loopback. Se realizó en **Ubuntu 24.04.4 bajo WSL2**; Ubuntu 22.04
es el entorno objetivo y no se ejecutó el checker privado de Holberton.

En `basics_1` también se comprobaron la resolución de nombres, la salida de
IPv4 activas y la recepción exacta de texto en el puerto 98, con localhost
resuelto a `127.0.0.1` y a `127.0.0.2`. Estas pruebas se realizaron en entornos
aislados, sin cambiar los archivos de resolución ni las interfaces del equipo.

Consulta los README de [basics_0](./basics_0/README.md) y
[basics_1](./basics_1/README.md) para ver los ejercicios y sus comprobaciones.

## Mantenimiento

Repositorio de [anthocab2](https://github.com/anthocab2).
