# Lab 06 - VLSM & Static Routing

## Objective

Subnet the `192.168.5.0/24` network using Variable Length Subnet Masking (VLSM), assign appropriate IPv4 addresses to the network devices, configure static routing between R1 and R2, and verify end-to-end connectivity.

## Lab Environment

- Cisco Packet Tracer
- 2 Routers (R1 and R2)
- Multiple LANs
- Point-to-point connection between R1 and R2
- PCs connected to each LAN

---

## 1. VLSM Address Planning

The original network was:

```text
192.168.5.0/24
```

I used VLSM to divide the `/24` network into smaller subnets based on the number of hosts required by each LAN.

I assigned address space to the **largest LAN first**, followed by progressively smaller networks.

### Subnetting Table

|Network| Network Address | Prefix|    Subnet Mask    |       Usable Host Range       | Broadcast |
|-------|-----------------|-------|-------------------|---------------------------------|-----------|
| LAN 2 | `192.168.5.0`   | `/25` | `255.255.255.128` | `192.168.5.1 - 192.168.5.126`   | `192.168.5.127` |
| LAN 1 | `192.168.5.128` | `/26` | `255.255.255.192` | `192.168.5.129 - 192.168.5.190` | `192.168.5.191` |
| LAN 3 | `192.168.5.192` | `/28` | `255.255.255.240` | `192.168.5.193 - 192.168.5.206` | `192.168.5.207` |
| LAN 3 | `192.168.5.208` | `/28` | `255.255.255.240` | `192.168.5.209 - 192.168.5.222` | `192.168.5.223` |
| R1-R2 | `192.168.5.224` | `/30` | `255.255.255.252` | `192.168.5.225 - 192.168.5.226` | `192.168.5.227` |

---

## 2. Understanding the /25 Subnet

LAN 2 uses:

```text
192.168.5.0/25
```

A `/25` provides 128 total addresses.

```text
Network:       192.168.5.0
First usable:  192.168.5.1
Last usable:   192.168.5.126
Broadcast:     192.168.5.127
Next subnet:   192.168.5.128
```

For this lab, the PC receives the **first usable address** and the router interface receives the **last usable address**.

Example:

```text
PC:  192.168.5.1
R1:  192.168.5.126
```

`192.168.5.127` cannot be assigned to R1 because it is the broadcast address.

---

## 3. Understanding the /28 Subnet

LAN 3 uses:

```text
192.168.5.192/28
```

The `/28` subnet mask is:

```text
255.255.255.240
```

To calculate the block size:

```text
256 - 240 = 16
```

Therefore, `/28` subnet boundaries increase by 16:

```text
.192
.208
.224
.240
```

LAN 3 starts at `.192`, and the next subnet starts at `.208`.

Therefore:

```text
Network:       192.168.5.192
First usable:  192.168.5.193
Last usable:   192.168.5.206
Broadcast:     192.168.5.207
```

### What I Learned

The `.240` in `255.255.255.240` is part of the **subnet mask**. It does not mean that `.240` is the broadcast address.

The broadcast address is one address before the beginning of the next subnet.

---

## 4. Configure the R1-R2 Point-to-Point Link

The connection between R1 and R2 uses:

```text
192.168.5.224/30
```

A `/30` subnet provides four addresses:

```text
Network:       192.168.5.224
R1/R2 usable:  192.168.5.225
R1/R2 usable:  192.168.5.226
Broadcast:     192.168.5.227
```

I assigned:

```text
R1: 192.168.5.225
R2: 192.168.5.226
```

Either usable address can be assigned to either router as long as the configuration is consistent.

### What I Learned

The specific router does not have to use `.225` or `.226`.

For example, both of these are valid:

```text
R1 = .225
R2 = .226
```

or:

```text
R1 = .226
R2 = .225
```

The static route must use the **neighboring router's actual IP address** as the next hop.

---

## 5. Configure Router Interfaces

I configured the router interfaces using the addresses calculated with VLSM.

### R1

```text
interface g0/0
description ## to SW1 ##
ip address 192.168.5.126 255.255.255.128
no shutdown

interface g0/1
description ## to SW2 ##
ip address 192.168.5.190 255.255.255.192
no shutdown
```

### R2

```text
interface g0/0
description ## to SW3 ##
ip address 192.168.5.206 255.255.255.240
no shutdown

interface g0/1
description ## to SW4 ##
ip address 192.168.5.222 255.255.255.240
no shutdown
```

---

## 6. Verify Router Interfaces

I verified the interface configuration using:

```text
show ip interface brief
```

Short form:

```text
sh ip int br
```

### Results

| Router | Interface |   IP Address  | Status | Protocol |
|--------|-----------|---------------|--------|----------|
|   R1   |    g0/0   | 192.168.5.126 |   up   |    up    |
|   R1   |    g0/1   | 192.168.5.190 |   up   |    up    |
|   R2   |    g0/0   | 192.168.5.206 |   up   |    up    |
|   R2   |    g0/1   | 192.168.5.222 |   up   |    up    |

---

## 7. Configure PC Addressing

For each LAN:

- The PC receives the **first usable host address**
- The router interface receives the **last usable host address**
- The router interface is used as the PC's **default gateway**

### PC Addressing

|      PC     |   IP Address    |    Subnet Mask    | Default Gateway |
|-------------|-----------------|-------------------|-----------------|
| PC on LAN 1 | `192.168.5.129` | `255.255.255.192` | `192.168.5.190 |
| PC on LAN 2 | `192.168.5.1`   | `255.255.255.128` | `192.168.5.126` |
| PC on LAN 3 | `192.168.5.193` | `255.255.255.240` | `192.168.5.206` |
| PC on LAN 4 | `192.168.5.109` | `255.255.255.240` | `192.168.5.222` |

---

## 8. Configure Static Routes

After configuring the interfaces, each router automatically knew about its directly connected networks.

Static routes were required to reach the remote LANs.

### Static Route Syntax

```text
ip route <destination-network> <subnet-mask> <next-hop-IP>
```

### R1

```text
192.168.5.192/28 via 192.168.5.226
192.168.5.208/28 via 192.168.5.226
```

### R2

```text
192.168.5.0/26 via 192.168.5.225
192.168.5.128/26 via 192.168.5.225
```

### What I Learned

The destination network is the **remote network I want to reach**.

The next-hop address is the IP address of the **neighboring router** that can forward traffic toward that network.

---

## 9. Verify the Routing Table

I verified the routes using:

```text
show ip route
```

Routes marked with:

```text
C
```

are directly connected networks.

Routes marked with:

```text
S
```

are static routes.


---

## 10. Test End-to-End Connectivity

After completing the VLSM addressing and static routing configuration, I tested connectivity between PCs on different LANs.

```text
ping <destination-IP>
```

### Results

|     Source    | Destination | Result |
|---------------|-------------|---|
| `192.168.5.1` | `192.168.5.129` | `Successful` |
| `192.168.5.1` | `192.168.5.193` | `Successful` |
| `192.168.5.1` | `192.168.5.109` | `Successful` |


---

## Key Takeaways

In this lab, I practiced:

- Variable Length Subnet Masking (VLSM)
- Determining subnet sizes based on host requirements
- Calculating network addresses
- Calculating usable host ranges
- Calculating broadcast addresses
- Understanding CIDR prefix lengths
- Calculating subnet block sizes
- Configuring router IPv4 addresses
- Configuring PC IPv4 addresses and default gateways
- Configuring `/30` point-to-point links
- Configuring static routes
- Understanding next-hop addresses
- Verifying routing tables
- Testing end-to-end connectivity

---

