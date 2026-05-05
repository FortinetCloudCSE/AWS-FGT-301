---
title: "Traffic Shaping"
linkTitle: "Module 10: Traffic Shaping"
chapter: false
weight: 10
---

> Additional Demo Section

---

## Traffic Shaping Test

SSH into **Ubuntu-Branch1** and **Ubuntu-DC** and issue the following commands:

![SSH](printscreen-10-1.png)

**Ubuntu-DC:**

```bash
iperf3 -s -p 6001 -i 1
```

**Ubuntu-Branch1:**

```bash
iperf3 -c 192.168.100.10 -p 6001 -u -i 1 -t 600 -b 25M -l 1360 -P 4 --bidir -S 0xb8
```

---

## Traffic Shaping Configuration

**Navigation:** FMG → Policy & Object → Policy Packages → Branch → Traffic Shaping

- Matching on **TOS values** with masks and specific ports and interfaces.
- Action assigns a **Traffic Shaping Class ID**.

![TRAFFIC SHAPING CONFIGURATION](printscreen-10-2.png)

---

## Shaping Profiles

**Navigation:** FMG → Policy & Object → Firewall Objects → Shaping Profile

Priority, Guaranteed, and Maximum Bandwidths are set within the shaping profiles.

![SHAPING PROFILES](printscreen-10-3.png)

---

## Branch Interface Bandwidth

**Navigation:** FMG → Device Manager → Device & Groups → Managed FortiGates → Branch* → Network: Interfaces

The Shaping Profile acts by limiting based on a **percentage of bandwidth**. The bandwidth is defined here along with applying the profile.

![INTERFACE BANDWIDTH](printscreen-10-4.png)

---

## Bandwidth Assignment to Dynamic Tunnels

### Branch Side

**Navigation:** FMG → Device Manager → Device & Groups → Managed FortiGates → Branch* → VPN: IPsec Phase 1

- The branches send the hubs BW information about their tunnels.
- The BW value set here should **match** the BW value of the interface.

![DYNAMIC TUNNELS](printscreen-10-5.png)

### Hub Side

**Navigation:** FMG → Device Manager → Device & Groups → Managed FortiGates → Hub* → VPN: IPsec Phase 1

- **Peer Egress Shaping** is enabled on the Hub Tunnels.
- A BW value should **NOT** be set.

![HUB SIDE](printscreen-10-6.png)

---

## Branch Traffic Shaping Monitoring

**Navigation:** FMG → Device Manager → Device & Groups → Managed FortiGates → Branch1 → Dashboard: Network Monitors

Select **HUB1-VPN1** in the Traffic Shaping (Interface-based) widget and expand.

![TRAFFIC SHAPING MONITORING - BRANCH](printscreen-10-7.png)

---

## Hub Traffic Shaping Monitoring

**Navigation:** FMG → Device Manager → Device & Groups → Managed FortiGates → Hub1 → Dashboard: Network Monitors

Select **VPN1_0** or **VPN1_1** in the Traffic Shaping (Interface-based) widget and expand.

![TRAFFIC SHAPING MONITORING - HUB](printscreen-10-8.png)
