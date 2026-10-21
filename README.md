# Holberton School — Network

Ejercicios de fundamentos de redes de Holberton School. El repositorio reúne
respuestas conceptuales y scripts Bash para practicar las herramientas de
diagnóstico de Linux.

## Módulos

| Directorio | Contenido | Documentación |
| --- | --- | --- |
| [basics_0](./basics_0/) | Modelo OSI, tipos de red, direcciones MAC e IP, TCP, UDP, sockets y conectividad. | [README del módulo](./basics_0/README.md) |

## Primeros pasos

El entorno objetivo es **Ubuntu 22.04** con Bash. Instala las herramientas
del sistema y clona el repositorio:

```bash
sudo apt-get update
sudo apt-get install net-tools iputils-ping shellcheck
git clone https://github.com/anthocab2/holbertonschool-network.git
cd holbertonschool-network/basics_0
./4-TCP_and_UDP_ports
./5-is_the_host_on_the_network 127.0.0.1
```

Los requisitos, los comandos de verificación y el comportamiento de cada
archivo están descritos en el [README de basics_0](./basics_0/README.md).

## Mantenimiento

Repositorio de [anthocab2](https://github.com/anthocab2).
