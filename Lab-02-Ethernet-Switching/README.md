# Lab 02 - Ethernet Switching & MAC Address Learning

## Objective

Practice Ethernet switching, ARP, MAC address learning, and MAC address
table verification using Cisco Packet Tracer.

## Lab Environment

- Cisco Packet Tracer
- 2 Cisco switches
- 4 PCs
- 192.168.1.0/24
- Switch MAC address tables initially empty
- PC ARP tables initially empty

---

## 1. Analyze PC1 to PC3 Communication

Before sending traffic, both switches had empty MAC address tables, and
the PCs had empty ARP tables.

I analyzed what should happen when PC1 sends a ping to PC3 before
running the simulation.

### Expected Traffic

Because PC1 does not initially know PC3's MAC address, it must first
use ARP to discover it.

The communication process includes:

1. PC1 sends an ARP Request.
2. The ARP Request is a broadcast.
3. The switches learn MAC addresses from the source MAC addresses of
   frames they receive.
4. The switches flood the broadcast out the appropriate ports except
   the port where it was received.
5. PC3 receives the ARP Request and sends an ARP Reply.
6. PC1 learns PC3's MAC address.
7. PC1 can then send the ICMP Echo Request.
8. PC3 responds with an ICMP Echo Reply.

---

## 2. Verify Using Simulation Mode

I used Packet Tracer Simulation Mode to observe the traffic generated
when PC1 pinged PC3.

I observed the ARP process before the ICMP ping communication.

### Protocols Observed

- ARP
- ICMP
- Ethernet

### What I Learned

A device needs to know the destination MAC address before it can
encapsulate an IPv4 packet into an Ethernet frame for a destination
on the local network.

---

## 3. Generate Traffic for MAC Address Learning

I sent pings between the PCs to generate network traffic.

As frames passed through the switches, the switches dynamically
learned the source MAC addresses and associated them with switch
interfaces.

---

## 4. Verify the MAC Address Tables

I used the following command on the switches:

```text
show mac address-table
| Device | MAC Address | Switch    | Interface |
| ------ | ----------- | --------- | --------- |
| PC1    | 0001.647b.3119   | SW2 | Fa0/2 |
| PC2    | 0004.9a6e.d870   | SW2 | Fa0/1 |
| PC3    | 0060.5c56.14d3   | SW1 | Fa0/2 |
| PC4    | 00d0.d3ad.9cab   | SW1 | Fa0/1 |

