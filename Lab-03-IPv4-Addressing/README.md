# Lab 03 - IPv4 Addressing & Interface Configuration

## Objective

Configure IPv4 addressing on a Cisco router and end devices, enable router interfaces, verify the configuration, and test network connectivity using ping.

## Lab Environment

- Cisco Packet Tracer
- 1 Cisco Router (R1)
- PC1
- PC2
- PC3

---

## 1. Configure R1 Hostname

I changed the router hostname to R1.

### Commands

```text
enable
configure terminal
hostname R1
```

After changing the hostname, the CLI prompt changed to:

```text
R1(config)#
```

### What I Learned

The `hostname` command changes the device name and helps identify the device when working in the Cisco IOS CLI.

---

## 2. Check Router Interfaces

Before configuring the router interfaces, I checked their current IP addresses and status.

### Command

```text
show ip interface brief
```

Short form:

```text
sh ip int br
```

### What I Learned

`show ip interface brief` provides a quick summary of the router's interfaces, including:

- Interface name
- IP address
- Interface status
- Line protocol status

This is useful for quickly checking whether interfaces have IP addresses and whether they are operational.

---

## 3. Configure R1 Interfaces

I configured the required interfaces on R1 with IPv4 addresses, subnet masks, descriptions, and enabled the interfaces.

### Interface 1

```text
interface [FILL IN: interface name]
description [FILL IN: description]
ip address [FILL IN: IP address] [FILL IN: subnet mask]
no shutdown
```

### Interface 2

```text
interface [FILL IN: interface name]
description [FILL IN: description]
ip address [FILL IN: IP address] [FILL IN: subnet mask]
no shutdown
```

### Interface 3

```text
interface [FILL IN: interface name]
description [FILL IN: description]
ip address [FILL IN: IP address] [FILL IN: subnet mask]
no shutdown
```

> Delete Interface 3 if the lab only uses two router interfaces.

### What I Learned

- `interface` selects the interface I want to configure.
- `description` documents what the interface connects to.
- `ip address` assigns an IPv4 address and subnet mask to the interface.
- `no shutdown` administratively enables the interface.

---

## 4. Verify R1 Interfaces

After configuring and enabling the interfaces, I checked their status again.

### Command

```text
show ip interface brief
```

### Results

| Interface | IP Address | Status | Protocol |
|---|---|---|---|
| [FILL IN] | [FILL IN] | [FILL IN] | [FILL IN] |
| [FILL IN] | [FILL IN] | [FILL IN] | [FILL IN] |
| [FILL IN] | [FILL IN] | [FILL IN] | [FILL IN] |

### What I Observed

**[FILL IN: Write what you observed. For example: The configured interfaces displayed the correct IP addresses and showed an up/up status.]**

---

## 5. Verify and Save the Configuration

I viewed the running configuration to confirm my changes.

### View Running Configuration

```text
show running-config
```

Short form:

```text
sh run
```

After confirming the configuration, I saved the running configuration to the startup configuration.

```text
copy running-config startup-config
```

Short form:

```text
copy run start
```

### What I Learned

The `running-config` contains the router's current active configuration.

The `startup-config` contains the saved configuration that will be loaded when the device starts.

Saving the running configuration prevents my changes from being lost after the router is restarted.

---

## 6. Configure PC IPv4 Addresses

I configured the IPv4 settings for PC1, PC2, and PC3 in Packet Tracer.

### Packet Tracer Path

```text
PC → Desktop → IP Configuration
```

### Addressing Table

| Device | IP Address | Subnet Mask | Default Gateway |
|---|---|---|---|
| PC1 | [FILL IN] | [FILL IN] | [FILL IN] |
| PC2 | [FILL IN] | [FILL IN] | [FILL IN] |
| PC3 | [FILL IN] | [FILL IN] | [FILL IN] |

### What I Learned

Each PC needs an appropriate IPv4 address and subnet mask.

The default gateway is the IP address of the router interface connected to the PC's local network.

When a PC needs to communicate with a device on another network, it sends the traffic to its default gateway.

---

## 7. Test Network Connectivity

After configuring R1 and the PCs, I tested network connectivity from PC1.

### PC1 → PC2

```text
ping [FILL IN: PC2 IP address]
```

**Result:** [FILL IN: Successful / Failed]

### PC1 → PC3

```text
ping [FILL IN: PC3 IP address]
```

**Result:** [FILL IN: Successful / Failed]

### What I Observed

**[FILL IN: Describe your ping results.]**

### What I Learned

The `ping` command uses ICMP to test IP connectivity between network devices.

A successful ping confirms that the devices can communicate at the network layer.

If the PCs are on different networks, successful communication also confirms that R1 is correctly forwarding traffic between its directly connected networks.

---

## Key Takeaways

In this lab, I practiced:

- Navigating the Cisco IOS CLI
- Configuring a router hostname
- Using `show ip interface brief`
- Configuring IPv4 addresses on router interfaces
- Configuring subnet masks
- Adding interface descriptions
- Enabling interfaces with `no shutdown`
- Verifying interface status
- Using `show running-config`
- Saving the running configuration
- Configuring IPv4 addresses on PCs
- Configuring default gateways
- Testing network connectivity with `ping`
- Understanding how a router connects different IP networks

---

## Commands Practiced

```text
enable
configure terminal
hostname R1
show ip interface brief
interface <interface>
description <description>
ip address <IP-address> <subnet-mask>
no shutdown
show running-config
copy running-config startup-config
ping <destination-IP>
```
