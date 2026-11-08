# Lesson 1: First Lab Output

Output captured from the `first-lab` topology (two Nokia SR Linux nodes connected
back to back on `ethernet-1/1`), defined in `topology/lab.clab.yml`.

## 1. `containerlab inspect`

| Name                | Kind | Image                          | State   | IPv4        | IPv6              |
|---------------------|------|--------------------------------|---------|-------------|-------------------|
| clab-first-lab-srl1 | srl  | ghcr.io/nokia/srlinux:24.10.1  | running | 172.20.20.2 | 3fff:172:20:20::2 |
| clab-first-lab-srl2 | srl  | ghcr.io/nokia/srlinux:24.10.1  | running | 172.20.20.3 | 3fff:172:20:20::3 |

- Container names follow the pattern `clab-<lab-name>-<node-name>`.
- Both nodes are running and have management addresses on the default
  containerlab management network (`172.20.20.0/24` and `3fff:172:20:20::/64`).

## 2. System version (`show version` on srl1)

| Field                 | Value                    |
|-----------------------|--------------------------|
| Hostname              | srl1                     |
| Chassis Type          | 7220 IXR-D2L             |
| Part Number           | Sim Part No.             |
| Serial Number         | Sim Serial No.           |
| System HW MAC Address | 1A:99:00:FF:00:00        |
| OS                    | SR Linux                 |
| Software Version      | v24.10.1                 |
| Build Number          | 492-gf8858c5836          |
| Architecture          | x86_64                   |
| Last Booted           | 2026-11-08T17:03:37.420Z |
| Total Memory          | 12,898,410 kB (~12.3 GiB) |
| Free Memory           | 8,337,281 kB (~7.95 GiB) |

- The node is emulating a **7220 IXR-D2L** switch. The "Sim" part and serial
  numbers show it is a containerised simulation, not real hardware.
- The software version (v24.10.1) matches the image tag in the topology file.
  The `sversion` file holds just this line (`Software Version : v24.10.1`).

## 3. Interfaces (`show interface brief` on srl1)

| Port                           | Admin State | Oper State | Speed |
|--------------------------------|-------------|------------|-------|
| ethernet-1/1                   | enable      | **up**     | 25G   |
| ethernet-1/2 – ethernet-1/48   | disable     | down       | 25G   |
| ethernet-1/49 – ethernet-1/56  | disable     | down       | 100G  |
| ethernet-1/57 – ethernet-1/58  | disable     | down       | 10G   |
| mgmt0                          | enable      | **up**     | 1G    |

The Type and Description columns were empty for every port.

- The IXR-D2L has 58 front-panel ports: 48 × 25G, 8 × 100G and 2 × 10G.
- Only two interfaces are up:
  - **ethernet-1/1**: the data-plane link to `srl2:e1-1` defined in the topology
    `links` section. Containerlab enables it automatically.
  - **mgmt0**: the management interface that carries the `172.20.20.2` address
    shown by `containerlab inspect`.
- Every other port is admin-disabled because nothing is wired to it.
