# Documentação - Topologia IPv6 com Três Roteadores (R1, R2, R3)

## 📅 Cenário

Simulação de uma rede IPv6 em ambiente Cisco Packet Tracer (PAC-13), com três roteadores modelo 2911 (R1, R2 e R3), cada um com um switch 2960 e dois PCs.

## 🔢 Topologia

```
 [PC1_R1]      [PC2_R1]     [PC1_R2]      [PC2_R2]     [PC1_R3]      [PC2_R3]
     |             |            |             |            |             |
   [SW1]        [SW2]        [SW3]       [SW4]        [SW5]         [SW6]
     |             |            |             |            |             |
    R1-----------R2-----------R3-----------R1
```

## 🌐 Endereçamento IPv6

### Redes LAN:

| Roteador | Faixa IPv6 LAN           | Prefixo | Hosts Necessários |
| -------- | ------------------------ | ------- | ----------------- |
| R1       | 2001\:db8\:acad:10::/120 | /120    | 30                |
| R2       | 2001\:db8\:acad:20::/120 | /120    | 100               |
| R3       | 2001\:db8\:acad:30::/124 | /124    | 10                |

### Links entre Roteadores:

| Link    | Faixa IPv6             | Prefixo |
| ------- | ---------------------- | ------- |
| R1 ↔ R2 | 2001\:db8\:acad:1::/64 | /64     |
| R2 ↔ R3 | 2001\:db8\:acad:2::/64 | /64     |
| R3 ↔ R1 | 2001\:db8\:acad:3::/64 | /64     |

## 💻 IPv6 dos PCs

### LAN R1:

| Dispositivo | Endereço IPv6              | Gateway               |
| ----------- | -------------------------- | --------------------- |
| PC1\_R1     | 2001\:db8\:acad:10::10/120 | 2001\:db8\:acad:10::1 |
| PC2\_R1     | 2001\:db8\:acad:10::11/120 | 2001\:db8\:acad:10::1 |

### LAN R2:

| Dispositivo | Endereço IPv6              | Gateway               |
| ----------- | -------------------------- | --------------------- |
| PC1\_R2     | 2001\:db8\:acad:20::10/120 | 2001\:db8\:acad:20::1 |
| PC2\_R2     | 2001\:db8\:acad:20::11/120 | 2001\:db8\:acad:20::1 |

### LAN R3:

| Dispositivo | Endereço IPv6              | Gateway               |
| ----------- | -------------------------- | --------------------- |
| PC1\_R3     | 2001\:db8\:acad:30::10/124 | 2001\:db8\:acad:30::1 |
| PC2\_R3     | 2001\:db8\:acad:30::11/124 | 2001\:db8\:acad:30::1 |

## ⚙️ Configuração dos Roteadores

### R1

```bash
enable
conf t
ipv6 unicast-routing

interface g0/0
 description LAN R1
 ipv6 address 2001:db8:acad:10::1/120
 no shutdown

interface g0/1
 description Link para R2
 ipv6 address 2001:db8:acad:1::1/64
 no shutdown

interface g0/2
 description Link para R3
 ipv6 address 2001:db8:acad:3::2/64
 no shutdown

ipv6 route 2001:db8:acad:20::/120 2001:db8:acad:1::2
ipv6 route 2001:db8:acad:30::/124 2001:db8:acad:3::1
```

### R2

```bash
enable
conf t
ipv6 unicast-routing

interface g0/0
 description LAN R2
 ipv6 address 2001:db8:acad:20::1/120
 no shutdown

interface g0/1
 description Link para R1
 ipv6 address 2001:db8:acad:1::2/64
 no shutdown

interface g0/2
 description Link para R3
 ipv6 address 2001:db8:acad:2::1/64
 no shutdown

ipv6 route 2001:db8:acad:10::/120 2001:db8:acad:1::1
ipv6 route 2001:db8:acad:30::/124 2001:db8:acad:2::2
```

### R3

```bash
enable
conf t
ipv6 unicast-routing

interface g0/0
 description LAN R3
 ipv6 address 2001:db8:acad:30::1/124
 no shutdown

interface g0/1
 description Link para R2
 ipv6 address 2001:db8:acad:2::2/64
 no shutdown

interface g0/2
 description Link para R1
 ipv6 address 2001:db8:acad:3::1/64
 no shutdown

ipv6 route 2001:db8:acad:10::/120 2001:db8:acad:3::2
ipv6 route 2001:db8:acad:20::/120 2001:db8:acad:2::1
```

## ✅ Testes de Conectividade

Em cada PC, usar o comando:

```bash
ping 2001:db8:acad:XX::YY
```

Exemplos:

* PC1\_R1 → ping PC1\_R2: `ping 2001:db8:acad:20::10`
* PC2\_R2 → ping PC2\_R3: `ping 2001:db8:acad:30::11`
* PC1\_R3 → ping PC1\_R1: `ping 2001:db8:acad:10::10`

## 📄 Observações

* Todos os PCs devem ter gateway corretamente configurado.
* Certifique-se de ativar as interfaces com `no shutdown`.
* O roteamento estático é suficiente para esse cenário.

---

Criado por \ivaldo - \11-06-2025
