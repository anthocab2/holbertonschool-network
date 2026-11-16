# Holberton School — Network

Holberton School networking fundamentals coursework, including conceptual answers and Bash scripts for practicing Linux network diagnostics.

## Modules

| Directory | Contents | Documentation |
| --- | --- | --- |
| [basics_0](./basics_0/) | OSI model, network types, MAC and IP addresses, TCP, UDP, sockets, and connectivity | [Module README](./basics_0/README.md) |
| [basics_1](./basics_1/) | Local hostname resolution, active IPv4 addresses, and a localhost TCP listener | [Module README](./basics_1/README.md) |
| [What happens when...](./what_happens_when_your_type_google_com_in_your_browser_and_press_enter/) | English article about DNS, TCP/IP, security, and servers when visiting Google | [Module README](./what_happens_when_your_type_google_com_in_your_browser_and_press_enter/README.md) |

## Repository structure

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
├── basics_1/
│   ├── README.md
│   ├── 0-change_your_home_IP
│   ├── 1-show_attached_IPs
│   └── 2-port_listening_on_localhost
└── what_happens_when_your_type_google_com_in_your_browser_and_press_enter/
    ├── README.md
    ├── 0-blog_post
    └── blog_post.md
```

In `basics_0`, files 0–3 contain numerical answers to coursework questions; files 4–5 are executable extensionless Bash scripts. In `basics_1`, three scripts configure local name resolution, list active IPv4 addresses, and receive text on localhost TCP port 98.

## Learning objectives

- Identify the OSI layers and distinguish LAN, WAN, and the Internet.
- Explain MAC and IP addressing, TCP, UDP, and network sockets.
- Inspect listening sockets and test ICMP responses with Linux tools.
- Configure hostname-to-IP mappings in an isolated practice environment.
- List active IPv4 addresses and receive data through netcat.

## Getting started

Target environment: **Ubuntu 22.04** with Bash and Git. Install `net-tools` for `netstat` and `ifconfig`, `iputils-ping` for `ping`, `netcat-openbsd` for TCP listening, and `shellcheck` for static analysis:

```bash
sudo apt-get update
sudo apt-get install net-tools iputils-ping netcat-openbsd shellcheck
git clone https://github.com/anthocab2/holbertonschool-network.git
cd holbertonschool-network/basics_0
```

## Usage

For **Change your home IP**, **Show attached IPs**, and **Port listening on localhost**, see [basics_1](./basics_1/README.md). Task 0 edits `/etc/hosts` and should run only in an isolated practice environment.

From `basics_0/`, inspect listening sockets and related processes:

```bash
./4-TCP_and_UDP_ports
```

Send five pings to the loopback address:

```bash
./5-is_the_host_on_the_network 127.0.0.1
```

Without an argument, the second script prints a usage message without making a network request:

```text
Usage: 5-is_the_host_on_the_network {IP_ADDRESS}
```

Reported ports, processes, and latency depend on the system. Some process details may require additional permissions.

## Recorded validation

From `basics_0/`, check syntax and run static analysis:

```bash
bash -n 4-TCP_and_UDP_ports
bash -n 5-is_the_host_on_the_network
shellcheck 4-TCP_and_UDP_ports 5-is_the_host_on_the_network
```

Scripts are tracked as executable (`100755`); answers and README files use `100644`. Text files use LF line endings and end in a newline.

The recorded local validation included Bash syntax, ShellCheck, actual `netstat` execution, the missing-argument case, and five successful loopback ping packets, on **Ubuntu 24.04.4 under WSL2**. Ubuntu 22.04 is the target environment; Holberton's private checker was not run.

In `basics_1`, tests also checked hostname resolution, active IPv4 output, and exact TCP receipt on port 98 with localhost resolving to `127.0.0.1` and `127.0.0.2`. Those checks ran in isolated environments without modifying the host machine's active name-resolution files or interfaces.

See the [basics_0](./basics_0/README.md) and [basics_1](./basics_1/README.md) READMEs for exercises and validation details.

## What happens when you visit Google?

The [English article](./what_happens_when_your_type_google_com_in_your_browser_and_press_enter/blog_post.md) covers the eight required topics and is [published on Medium](https://medium.com/@anthonycaban1010/what-happens-when-you-type-https-www-google-com-in-your-browser-and-press-enter-080383ce98f8). The `0-blog_post` file contains its public URL; see the [module README](./what_happens_when_your_type_google_com_in_your_browser_and_press_enter/README.md) for submission instructions.

Maintained by [anthocab2](https://github.com/anthocab2).
