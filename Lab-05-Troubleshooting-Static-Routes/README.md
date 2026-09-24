# Lab 05 - Network Troubleshooting

## Objective

Troubleshoot a network where PC1 and PC2 are unable to communicate.

The network contains multiple routers, and each router has one
misconfiguration. The goal is to identify and fix each problem until
PC1 and PC2 can successfully ping each other.

---

## 1. Identify the Connectivity Problem

I first tested connectivity from PC1 to PC2.

### Command

```text
ping 192.168.3.1
```

### Result

The ping was unsuccessful.

### Observation

PC1 could not communicate with PC2, so I began troubleshooting the
network to determine where the communication was failing.

---

## 2. Check PC1's IP Configuration

I checked PC1's network configuration.

### Command

```text
ipconfig
```

I verified:

- IP address
- Subnet mask
- Default gateway

### PC1 Configuration

|     Setting     |      Value    |
|-----------------|---------------|
|    IP Address   |  192.168.1.1  |
|   Subnet Mask   | 255.255.255.0 |
| Default Gateway | 192.168.1.254 |

---

## 3. Test PC1's Default Gateway

After checking PC1's configuration, I tested connectivity between PC1
and its default gateway.

### Command

```text
ping 192.168.1.254
```

### Result

**Successful**

### What This Tells Me

Because PC1 can successfully ping its default gateway, the connection
between PC1 and its local router is working.

This suggests that I should continue troubleshooting beyond PC1's
local network.

---

## 4. Troubleshoot Router 1

I began checking R1 for configuration problems.

### Commands Used

```text
show ip interface brief
show ip route
show running-config | include ip route
```

### Misconfiguration Found

R1 had an incorrect static route to the `192.168.3.0/24` network.

The routing table showed:

```text
S    192.168.3.0/24 [1/0] via 192.168.12.3
```

The next-hop IP address was incorrectly configured as `192.168.12.3`.

The correct next-hop IP address should be `192.168.12.2`.

### How I Fixed It

I first removed the incorrect static route using the `no` command:

```text
no ip route 192.168.3.0 255.255.255.0 192.168.12.3
```

Then I configured the correct static route:

```text
ip route 192.168.3.0 255.255.255.0 192.168.12.2
```
```

### Verification

I verified the corrected route using:

```text
show ip route
```

The routing table now showed the correct next-hop address:

```text
S    192.168.3.0/24 [1/0] via 192.168.12.2
```

---

## 5. Troubleshoot Router 2

I checked R2's interfaces, routing table, and configuration.

### Commands Used

```text
show ip interface brief
show ip route
show running-config
```

### Misconfiguration Found

The route to the `192.168.3.0/24` network was configured with the wrong exit interface.

The routing table showed:

```text
192.168.3.0/24 is directly connected, GigabitEthernet0/0
```

The route should use `GigabitEthernet0/1` instead of `GigabitEthernet0/0`.

### How I Fixed It

I first removed the incorrect route using the `no` command:

```text
no ip route 192.168.3.0 255.255.255.0 GigabitEthernet0/0
```

Then I configured the route again using the correct exit interface:

```text
ip route 192.168.3.0 255.255.255.0 GigabitEthernet0/1

### Verification

I verified the routing table again using:

```text
show ip route
```

The route now showed the correct exit interface:

```text
192.168.3.0/24 is directly connected, GigabitEthernet0/1

## 6. Troubleshoot Router 3

I checked R3 for configuration problems.

### Commands Used

```text
show ip interface brief
show ip route
show running-config
```

### Misconfiguration Found


The `G0/0` interface on R3 had an incorrect IP address.

The correct IP address for `G0/0` should be `192.168.13.3`.

### How I Fixed It

I entered the `G0/0` interface configuration mode and configured the correct IP address:

```text
configure terminal
interface g0/0
ip address 192.168.13.3 255.255.255.0
```

The new IP address replaced the incorrectly configured address.

### Verification

I verified the interface configuration using:

```text
show ip interface brief
```

I confirmed that `G0/0` now had the correct IP address:

```text
GigabitEthernet0/0    192.168.13.3
```

I also checked the routing table:

```text
show ip route
```

This confirmed that the connected network was correctly reflected in the routing table after fixing the interface IP address.

---

## 7. Final Connectivity Test

After correcting the router misconfigurations, I tested connectivity
from PC1 to PC2 again.

### Command

```text
ping 192.168.3.1
```

### Result

Successful


## Troubleshooting Process

I used a step-by-step troubleshooting approach:

1. Tested end-to-end connectivity from PC1 to PC2.
2. Confirmed that the ping failed.
3. Used `ipconfig` to verify PC1's network configuration.
4. Pinged PC1's default gateway.
5. Confirmed that PC1 could communicate with its local router.
6. Checked each router's interfaces and routing table.
7. Identified the misconfiguration on each router.
8. Corrected the configurations.
9. Tested connectivity again.


---

## Key Takeaways

In this lab, I practiced:

- Troubleshooting network connectivity
- Using `ping` to isolate network problems
- Using `ipconfig` to verify host configuration
- Testing default gateway connectivity
- Checking router interface status
- Checking routing tables
- Reviewing router configurations
- Identifying router misconfigurations
- Verifying fixes with end-to-end connectivity testing
