---
title: "Auto-Discovery VPN (ADVPN)"
linkTitle: "Section 6: Auto-Discovery VPN (ADVPN)"
chapter: false
weight: 6
---

## Overview

Auto-Discovery VPN (ADVPN) allows the Hub to dynamically inform a Branch about a better path for traffic between two Branches.

### 7.4 ADVPN Routing Enhancements

- The Hubs no longer need to be BGP route-reflectors.
  - Default routes or RFC 1918 routes can be used on the Branches to get traffic back to the hub.
- BGP can now be dynamically established between branches over ADVPN.
- Next-hop tunnel interface IP no longer needs to be preserved.
- Branches/Spokes will exchange routes and next hops directly between each other over ADVPN tunnels.

### 7.4 ADVPN 2.0

- Initial tunnel is formed by passing traffic through the hub.
- Branches exchange full underlay information.
- Branches exchange tunnel preference information.
- Branches exchange health check information every 5 seconds.

---

## VPN Monitoring (Before ADVPN)

***Navigation: Device Manager → Device & Groups → Branch1 → Dashboard → Network Monitors → IPsec VPN (Enlarge)***

Branch1 has 2 IPsec VPN tunnels to Hub1 and Hub2.

![IPSEC VPN](printscreen-07-1.png)

> [!NOTE]
> We will refer to this FMG Dashboard after generating traffic from Branch1 to Branch2 to view the ADVPN shortcut tunnel that was created.

---

## Route Monitoring (Before ADVPN)

***Navigation: Device Manager → Device & Groups → Branch1 → Dashboard → Network Monitors → Routing – Static & Dynamic (Enlarge)***

On the same Dashboard – Network Monitors, if you scroll down, you will see the Routing – Static & Dynamic section.

![ROUTING STATIS & DYNAMIC](printscreen-07-2.png)

Notice that Branch1 currently does **not** have a route to the Branch2 network **10.48.0.32/28**.

> After generating traffic from Branch1 to Branch2, we will refer to this FMG Dashboard to view the route learned via BGP for the  10.48.0.32/28 Branch2 network.

---

## Generate Traffic from Branch1 to Branch2

1. Navigate to the **EC2 Console**, confirm you are in the **us-west-1 (N. California) region** in the upper right hand corner, and connect to **scw-region1-branch1-linux-instance** using the **[Serial Console directions](../2_cloudwan)** to get the unique username and console access.
    - Username: instance-id (serial console directions link above shows how to get this for your environment)
    - Password: `FORTInet123!`
2. Start a ping and leave it running to **scw-region2-branch-2-linux-instance** which is in **us-west-2 (Oregon) region**. Don not stop the ping as we will need this traffic for this section and the next section of the workshop.
    - `ping 10.48.0.46`

    This will generate ping traffic from a device behind Branch1 (**10.32.0.46**) toward a device behind Branch2 (**10.48.0.46**).

---

## ADVPN Monitoring

### IPsec VPN Dashboard

***Navigation: Device Manager → Device & Groups → Branch1 → Dashboard → Network Monitors → IPsec VPN (Enlarge)***

Now that traffic is flowing between Branch1 and Branch2, you should see a new ADVPN shortcut IPsec Tunnel named **HUB1-VPN1_0**.

![HUB1-VPN1](printscreen-07-4.png)

1. ADVPN shortcut IPsec tunnels are named using the **parent tunnel name followed by an underscore**.
2. The Peer ID is **Branch2-HUB1-VPN1**, denoting the far end hostname and far end tunnel name.

### ADVPN Monitoring

When the ADVPN shortcut IPsec tunnel HUB1-VPN1_0 is created, it can be viewed in the SD-WAN Dashboard under the **SD-WAN Interfaces** section below its parent tunnel.

![SD-WAN DASHBOARD](printscreen-07-5.png)

### Health Check

When the ADVPN shortcut IPsec tunnel HUB1-VPN1_0 is created, a health check is also created. It can be viewed in the SD-WAN Dashboard under the Health Check assigned to its parent tunnel. (Remember to scroll down to see all of the SD-WAN widget)

![HEALTH CHECK](printscreen-07-6.png)

> [!NOTE]
> SDWAN CSEs, can more info be specified here, what IPs are used for the shortcut health checks? The ADVPN Health Check is **not** using the HUB IP of 169.254.253.253 as its target.

---

## Route Monitoring (After ADVPN)

***Navigation: Device Manager → Device & Groups → Branch1 → Dashboard → Network Monitors → Routing – Static & Dynamic (Enlarge)***

![ROUTE MONITORING](printscreen-07-7.png)

Branch1 now has a dynamic **BGP route** to the Branch2 network **10.48.0.32/28** and a **static route** to the Branch2 loopback IP pointing to the shortcut tunnel **HUB1-VPN1_0**

---

## ADVPN Monitoring – FortiView VPN

***Navigation: FMG → FortiView → SD-WAN → Secure SD-WAN Monitor → SD-WAN Interfaces (scroll down to widget)***

FortiAnalyzer provides the FMG with a Secure SD-WAN Monitor. Here you can see the ADVPN shortcut tunnel **HUB1-VPN1_0** displays under its parent IPsec tunnel **HUB1-VPN1**.

![ADVPN MONITORING](printscreen-07-8.png)

> ADVPN shortcut IPsec tunnels are depicted with a special symbol next to them on this monitor.

---

## ADVPN Logging

***Navigation: Device Manager → Log View → Custom Views → ADVPN Events***

![alt text](printscreen-07-9.png)

---

## VPN Monitor – ADVPN Links

***Navigation: FMG → Device Manager → Monitors → SD-WAN Monitor → Map View***

Hovering over an individual link shows the ADVPN tunnel name from each Branch endpoint's perspective.

![ADVPN LINKS](printscreen-07-10.png)

### This concludes this section