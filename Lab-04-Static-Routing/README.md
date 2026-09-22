# Lab 04 - Static Routing

## Objective

Configure IPv4 addressing on a network with three routers and two PCs, then configure static routes to allow communication between remote networks.

## Lab Environment

- Cisco Packet Tracer
- 3 Routers (R1, R2, R3)
- 2 Switches
- 2 PCs (PC1, PC2)

---

## 1. Network Topology

The network consists of three routers connecting two separate LANs.

```text
PC1 --- Switch --- R1 --- R2 --- R3 --- Switch --- PC2
```

The switches did not require configuration for this lab.

---

## 2. Configure Router Hostnames

I configured the hostname on each router.

### R1

```text
enable
configure terminal
hostname R1
```

### R2

```text
enable
configure terminal
hostname R2
```

### R3

```text
enable
configure terminal
hostname R3
```

---

## 3. Configure Router Interfaces

I configured the router interfaces according to the network diagram.

Each interface was configured with:

- IPv4 address
- Subnet mask
- Interface description
- `no shutdown`

### R1

```text
interface GigabitEthernet0/0
description Connection to PC1 LAN
ip address 192.168.12.1 255.255.255.0
no shutdown

interface GigabitEthernet0/1
description Connection to R2
ip address 192.168.1.1 255.255.255.0
no shutdown
```

### R2

```text
interface GigabitEthernet0/0
description Connection to R1
ip address 192.168.12.2 255.255.255.0
no shutdown

interface GigabitEthernet0/1
description Connection to R3
ip address 192.168.13.2 255.255.255.0
no shutdown
```

### R3

```text
interface GigabitEthernet0/0
description Connection to R2
ip address 192.168.13.3 255.255.255.0
no shutdown

interface GigabitEthernet0/1
description Connection to PC2 LAN
ip address 192.168.3.254 255.255.255.0
no shutdown
```

---

## 4. Verify Router Interfaces

I verified the IP addresses and interface status using:

```text
show ip interface brief
```

Short form:

```text
sh ip int br
```

### Results

| Router |      Interface     |  IP Address  | Status | Protocol |
|--------|--------------------|--------------|--------|----------|
|   R1   | GigabitEthernet0/0 | 192.168.12.1 |   up   |   down   |
|   R1   | GigabitEthernet0/1 | 192.168.1.1  |   up   |    up    |
|   R2   | GigabitEthernet0/0 | 192.168.12.2 |   up   |    up    |
|   R2   | GigabitEthernet0/1 | 192.168.13.2 |   up   |   down   |
|   R3   | GigabitEthernet0/0 | 192.168.13.3 |   up   |    up    |
|   R3   | GigabitEthernet0/1 |192.168.3.254 |   up   |    up    |


## 5. Configure PC IPv4 Addresses

I configured PC1 and PC2 according to the network diagram.

In Packet Tracer:

```text
PC → Desktop → IP Configuration
```

### Addressing Table

| Device | IP Address |  Subnet Mask  | Default Gateway |
|--------|------------|---------------|-----------------|
|   PC1  | 192.168.1.1| 255.255.255.0 |  192.168.1.254  |
|   PC2  | 192.168.3.1| 255.255.255.0 | 192.168.3.254   |

### Default Gateway

The default gateway of each PC is the IP address of the router interface connected to that PC's LAN.

PC1 uses R1's LAN interface as its default gateway.

PC2 uses R3's LAN interface as its default gateway.

---

## 6. Test Connectivity Before Static Routing

Before configuring static routes, I tested connectivity between PC1 and PC2.

From PC1:

```text
ping 192.168.3.1
```

### Result

**PC1 → PC2:** Failed

### What I Observed
4 packets sent, but received = 0


### Why?

At this point, each router only knows about its directly connected networks.

The routers do not automatically know how to reach all remote networks.

---

## 7. Check the Routing Tables

I checked the routing table on each router using:

```text
show ip route
```

Short form:

```text
sh ip route
```

### What I Learned

The routing table tells the router which networks it knows how to reach and which interface or next-hop router should be used.

Before configuring static routes, the routers primarily knew about their directly connected networks.

---

## 8. Configure Static Routes

I configured static routes so that the routers could reach networks that were not directly connected.

### Static Route Syntax

```text
ip route <destination-network> <subnet-mask> <next-hop-IP>
```

### R1

```text
ip route 192.168.3.0 255.255.255.0 192.168.12.2
```

### R2

```text
ip route 192.158.1.0 255.255.255.0 g0/0

ip route 192.168.3.0 255.255.255.0 192.168.13.3
```

### R3

```text
ip route 192.168.1.0 255.255.255.0 192.168.13.2
```
### What I Learned

A static route manually tells a router how to reach a remote network.

The destination network identifies the network I want to reach.

The subnet mask identifies the size of that destination network.

The next-hop IP address identifies the neighboring router that should receive the packet next.

---

## 9. Verify Static Routes

After configuring the static routes, I checked the routing tables again.

```text
show ip route
```

I looked for routes marked with:

```text
S
```

`S` identifies a static route in the Cisco routing table.

---

## 10. Test PC1 to PC2 Connectivity

After configuring the static routes, I tested connectivity again.

From PC1:

```text
ping 192.168.3.1
```

### Result

**PC1 → PC2:** Succesful

---

## 11. Save the Configurations

After verifying connectivity, I saved the configuration on each router.

```text
copy running-config startup-config
```

Short form:

```text
copy run start
```

---

## What I Learned

In this lab, I learned that configuring IP addresses alone is not enough for communication across multiple routers.

Routers automatically know their directly connected networks, but they need routes to reach remote networks.

I practiced configuring static routes and verified them using the routing table and end-to-end ping tests.

---

## Key Takeaways

In this lab, I practiced:

- Configuring Cisco router hostnames
- Configuring IPv4 addresses
- Configuring router interfaces
- Enabling interfaces with `no shutdown`
- Configuring PC IPv4 settings
- Configuring PC default gateways
- Using `show ip interface brief`
- Using `show ip route`
- Understanding directly connected networks
- Configuring static routes
- Understanding destination networks and next-hop addresses
- Testing end-to-end connectivity with `ping`
- Saving Cisco configurations

---

