---
title: Lab 2 - System Templates Creation
linkTitle: Lab 2 - System Templates
weight: 35
---

## System Template Creation

This section will describe how to create System Templates for the HUBs and Branches. We will create HUB System Templates to configure FortiAnalyzer logging and GUI Theme.  The Branch System Template will set up FAZ logging, admin timeout, hostname, GUI theme, a DHCP Server, and an interface zone used later in our Policy Package.

&nbsp;
&nbsp;

### Hub System Template

**Navigate to:**

***Device Manager>Provisioning Templates>System Templates***

- Click ‘Create New’ and ‘Blank Template’
![New HubSystem Template](systemblank.png?width=950px)

Fill in the following fields:
 - Name – ` HUB_SYSTEM `
 - Toggle Admin Settings on
  - Change Idle Timeout to 60
  - In View Settings
    - Change Theme to Graphite
    - Set Timezone to the timezone you are in
 - Toggle Log Settings to on
 - Toggle Send Logs to FortiAnalzer/Fortimanager
 - Send To – Specify IP Address
 - Enter FAZ port1 IP address – ` 192.168.0.5 `
 - Upload Option – Real-time
 - Encrypt Log Transmission – High
 - Toggle Reliable Logging to FortiAnalyzer to on
 - Click OK
![Name HubSystem Template](hubsystem1.png?width=750px)
![FAZ HubSystem Template](hubsystem2.png?width=750px)
Result:
![HubSystem Template](hubsystem3.png?width=950px)

The HUB System Template is now setup for FAZ logging and our other settings.

&nbsp;
&nbsp;

### Branch System Template

The Branch System Template will be configured for FAZ logging, a DHCP Server, and an interface zone used later in the Branch Policy Package.

**Navigate to:**

***Device Manager>Provisioning Templates>System Templates***

- Click ‘Create New’ and ‘Blank Template’
![New System Template](systemblank.png?width=950px)

&nbsp;

#### Branch Admin and Log Settings Section
Fill in the following fields:
 - Name – ` BRANCH_SYSTEM `
 - Toggle Admin Settings on
  - Change Idle Timeout to 60
  - In View Settings
    - Change Theme to Graphite
    - Hostname - ` {{DVMDB.name}} `
    - Set Timezone to the timezone you are in
 - Toggle Log Settings to on
  - Toggle Send Logs to FortiAnalzer/Fortimanager
    - Sent To – Specify IP Address
    - Enter FAZ port1 IP address – ` 192.168.0.5 `
    - Upload Option – Real-time
    - Encrypt Log Transmission – High
    - Toggle Reliable Logging to FortiAnalyzer to on
![Name Branch System Template](brsystem1.png?width=750px)
![Name Branch System Template](btsystem2.png?width=750px)

&nbsp;

#### Branch Interface Section
This portion of the System Template will use metadata variables and IPv4 address variable manipulation to create a DHCP server for the LAN interfaces.
- Toggle the Interface radio button to on
- Click 'Create New'
![DHCP Branch System Template](brsystemint1.png?width=750px)
Fill in the following fields:
 - Action – Select ‘DHCP Server’
 - Interface Name – port2
 - ID – ‘1000’
 - Default Gateway – ` $(lan1_net:4,1) `
 - Netmask – `$(lan1_mask)`
 - IP Range
   - Start IP - ` $(lan1_net:4,20) `
   - End IP - ` $(lan1_net:4,126) `
 - Click ‘OK’
![DHCP Branch System Template8](brsystemintdhcp.png?width=750px)
Result:
![DHCP Branch System Template9](brsystemintdhcp2.png?width=750px)

&nbsp;

Add the Interface Zone to be used later in our Branch Policy Package.

Still in the Interface section where we just created our DHCP Server, let’s hit the ‘Create New’ button again.
![Zone Branch System Template10](sysintzonecreate.png?width=750px)

Fill in the following fields:
 - Action – Select ‘Add Zone’
 - Zone Name – Type ` TRUSTED_ZONE `
 - Interface Members – Type ‘port2’
 - Click ‘OK’
![Zone Branch System Template11](brsysintzone2.png?width=750px)

Result:
![Zone Branch System Template12](brsysintcomplete.png?width=750px)
All 3 sections in the BRANCH_SYSTEM template are now complete.
 - Click the ‘OK” button.

&nbsp;
&nbsp;

The creation of our Hub and Branch System Templates are complete.
![Zone Branch System Template13](systemdone.png?width=950px)


### Now that we have completed the GUI System Templates, take a minute to view the CLI Configuration Preview both System Templates.