# Lazy Zabbix

This repository offers a ready-to-use Docker Compose setup for Zabbix: Server, Proxy, Web Frontend, and Agent, backed by MariaDB.

## Table of Contents

- [Features](#features)
- [Prerequisites](#prerequisites)
- [Setup](#setup)
- [Usage](#usage)
- [TLS / PSK Configuration](#tls--psk-configuration-optional)
- [Ports](#ports)
- [License](#license)
- [Contribution](#contribution)

## Features

- MariaDB configured with UTF-8 (utf8mb4) for Zabbix
- Zabbix Server, Web, Agent, Proxy, SNMP Traps ready
- Volumes for persistent data
- Optional TLS/PSK for Zabbix Proxy

## Prerequisites

- Docker >= 24.0
- Docker Compose >= 2.0
- `.env` file containing:

  ```text
  MYSQL_PASSWORD=YourStrongPassword
  PHP_TZ=Your/Timezone

## Setup

1. Clone the repository:

```bash
git clone https://github.com/netns/zbx-docker.git
cd zbx-docker
```

1. Create a .env file if not already present:

```py
MYSQL_PASSWORD=supersecretpassword
PHP_TZ=America/New_York
```

1. Optional: prepare TLS/PSK files for Proxy (see )

| Variable         | Purpose                               |
| ---------------- | ------------------------------------- |
| `MYSQL_PASSWORD` | Password for Zabbix database user     |
| `PHP_TZ`         | Timezone for PHP in the Web container |

## Usage

Start the server or proxy using Docker/Podman:

```bash
docker compose -f [SERVICE] up -d
```

Example:

```bash
docker compose -f zbx_proxy_mysql.yml up -d
```

## TLS / PSK Configuration (Optional)

For PSK-based TLS:

- Mount your key file at `./proxy-tls/proxy_psk.key`

- Ensure `ZBX_TLSCONNECT=psk` and `ZBX_TLSPSKIDENTITY` match the Server configuration

For Certificate-based TLS:

- Mount `ca.pem`, `proxy.crt`, `proxy.key` in `./proxy-tls`

- Use `ZBX_TLSCONNECT=cert` and set `ZBX_TLSCAFILE`, `ZBX_TLSCERTFILE`, `ZBX_TLSKEYFILE` accordingly

## Ports

| Service          | Container Port | Host Port |
| ---------------- | -------------- | --------- |
| Zabbix Server    | 10051          | 10051     |
| Zabbix Web       | 8080           | 8080      |
| Zabbix Web HTTPS | 8443           | 8443      |
| SNMP Traps       | 1162/udp       | 1162/udp  |

> [!WARNING]
> Do not bind 162/udp directly; non-root alternative port 1162 is used.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Contribution

Contributions are welcome! Feel free to open issues or submit pull requests to improve font selection, installation scripts, or documentation.
