
---

# 📘 Documentação - Topologia IPv6 com Três Roteadores (R1, R2, R3)

## 📅 Cenário

Simulação de uma rede IPv6 no **Cisco Packet Tracer (PAC-13)**, utilizando:

* **3 Roteadores Cisco 2911**: R1, R2 e R3
* **3 Switches Cisco 2960** (um por roteador)
* **6 PCs** (dois por roteador, conectados à respectiva LAN)

Cada roteador está interligado via IPv6 em sub-redes ponto-a-ponto e tem sua própria LAN em /64.

---

## 🖥️ Topologia

```
[PCs] -- SW -- R1 -- R2 -- R3 -- SW -- [PCs]
                 \     /
                  \   /
                 [Rede IPv6]
```

---

## ⚙️ Configuração dos Roteadores

### 🔧 R1

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
ipv6 route 2001:db8:acad:3::/64 2001:db8:acad:5::5

ipv6 unicast-routing
```

---

### 🔧 R2

```bash
enable
conf t

interface g0/0
 description Link para R1
 ipv6 address 2001:db8:acad:5::1/127
 no shutdown

interface g0/1
 description Link para R3
 ipv6 address 2001:db8:acad:5::2/127
 no shutdown

interface g0/2
 description LAN Rede
 ipv6 address 2001:db8:acad:2::/64
 no shutdown

ipv6 route 2001:db8:acad:1::/64 2001:db8:acad:5::0
ipv6 route 2001:db8:acad:3::/64 2001:db8:acad:5::3

ipv6 unicast-routing
```

---

### 🔧 R3

```bash
enable
conf t

interface g0/0
 description Link para R2
 ipv6 address 2001:db8:acad:5::3/127
 no shutdown

interface g0/1
 description Link para R1
 ipv6 address 2001:db8:acad:5::5/127
 no shutdown

interface g0/2
 description LAN Rede
 ipv6 address 2001:db8:acad:3::/64
 no shutdown

ipv6 route 2001:db8:acad:1::/64 2001:db8:acad:5::4
ipv6 route 2001:db8:acad:2::/64 2001:db8:acad:5::2

ipv6 unicast-routing
```

---

## ✅ Testes Recomendados

* `ping` entre PCs de diferentes LANs via IPv6.
* Verificação das rotas com `show ipv6 route` em cada roteador.
* Simular falhas nos links ponto-a-ponto para validar o roteamento manual.

---

