---
title: "How to Customise Your Demo in Fabric Studio"
linkTitle: "Module 12: How to Customise Your Demo in Fabric Studio"
chapter: false
weight: 12
---

## What is Fabric Studio?

Fabric Studio is an **internal** Fortinet tool to create simple or complex network topologies using different **Fortinet products**.

Key facts:

- It is a network emulation platform like GNS3 or EVE-NG.
- It supports 3rd party devices.
- All FNDN demos and labs are created in Fabric Studio or will be migrated to Fabric Studio.
- The 7.6 US-CSE SD-WAN demo is built in Fabric Studio as well.

**Resources:**

- Fabric Studio SharePoint Page: [Link](https://fortinet.sharepoint.com/sites/FabricStudio/SitePages/Home.aspx)
- Fabric Studio Starting Guide: [Link](https://tec.myfortinet.com/fndn/fabric-studio/)

---

## How to Login to Fabric Studio

1. Open a new HTTPS session to one of the demo FMG/FAZ/FGTs/etc.

   ![HTTPS](printscreen-12-1.png)

2. Remove everything after the base URL, which should present you with a login screen of Fabric Studio.

   ![LOGIN](printscreen-12-2.png)

3. Login with `admin` / `fortinet`

   ![CREDENTIALS](printscreen-12-3.png)

---

## How to Edit the 7.6 SD-WAN Template

Open 7.6 SD-WAN template called **"FortiManager POC Prep 7.6"**.

![FORTIMANAGER POC PREP](printscreen-12-4.png)

---

## Fabric Studio Functions

### Reboot INET-RTR

We have seen the INET-RTR VM lock up in some cases. This will cause "internet" connectivity to be down, which obviously causes problems in an SD-WAN demo.

![INET-RTR REBOOT](printscreen-12-5.png)

**Resolution:** Powering the INET-RTR off then back on has resolved this issue in every case.

### Break/Repair or Add/Delete Connections

- You can **edit the connections** by right-clicking the cables. This is useful when testing connections, traffic flows, or high-availability (HA) scenarios.

  ![EDIT CONNECTIONS](printscreen-12-6.png)

- You can **add or remove ports** by editing the device's settings. This is helpful for expanding your lab and incorporating additional devices or scenarios into your demo.

  ![ADD/REMOVE PORTS](printscreen-12-7.png)

### Add or Remove Devices

- You can add additional devices and cables to your demo and customise the environment based on your customer's use cases.

  ![ADD DEVICES AND CABLES](printscreen-12-8.png)

- You can view the list of supported Fortinet and third-party devices in Fabric Studio (see documentation).

  ![VIEW SUPPORTED 3RD PARTY DEVICES](printscreen-12-9.png)
