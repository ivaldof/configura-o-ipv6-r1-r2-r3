# Documentação - Topologia IPv6 com Três Roteadores (R1, R2, R3)

## 📅 Cenário

Simulação de uma rede IPv6 em ambiente Cisco Packet Tracer (PAC-13), com três roteadores modelo 2911 (R1, R2 e R3), cada um com um switch 2960 e dois PCs.

## ⚙️ Configuração dos Roteadores

### R1

```bash
enable
conf t

interface g0/0
 description LAN R2
 ipv6 address 2001:db8:acad:5::/127
 no shutdown

interface g0/1
 description Link para R3
 ipv6 address 2001:db8:acad:5::4/127
 no shutdown

interface g0/2
description LAN Rede
 ipv6 address 2001:db8:acad:1::/64
 no shutdown

ipv6 route 2001:db8:acad:2::/64 2001:db8:acad:5::1
ipv6 route 2001:db8:acad:3::/64 2001:db8:acad:5::3

ipv6 unicast-routing
```

### R2

```bash
enable
conf t

interface g0/0
description LAN R1
 ipv6 address 2001:db8:acad:5::1/127
 no shutdown

interface g0/1
 description Link para R3
 ipv6 address 2001:db8:acad:5::2/127
 no shutdown

interface g0/2
 description Link para Rede
 ipv6 address 2001:db8:acad:2::/64
 no shutdown

ipv6 route 2001:db8:acad:1::/64 2001:db8:acad:5::
ipv6 route 2001:db8:acad:3::/64 2001:db8:acad:5::5

ipv6 unicast-routing
```

### R3

```bash
enable
conf t

interface g0/0
 description LAN R2
 ipv6 address 2001:db8:acad:5::5/127
 no shutdown

interface g0/1
 description Link para R1
 ipv6 address 2001:db8:acad:5::3/127
 no shutdown

interface g0/2
 description Link para Rede
 ipv6 address 2001:db8:acad:3::/64
 no shutdown

ipv6 route 2001:db8:acad:1::/64 2001:db8:acad:5::4
ipv6 route 2001:db8:acad:2::/64 2001:db8:acad:5::2

ipv6 unicast-routing
```
Criado por \ivaldo - \11-06-2025
