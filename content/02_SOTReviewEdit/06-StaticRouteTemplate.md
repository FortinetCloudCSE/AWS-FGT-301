---
title: Lab 2 - Static Route Template Review and Edits
linkTitle: Lab 2 - Static Route Template
weight: 30
---

&nbsp;

## Static Route Template Creation

Static routes are needed for both HUBs and Branches.  The HUB Template is already configured with a blackhole static route for the loopback subnet but we will need to add a static default route on the underlay for the tunnels to come up. The Branch template has a static default route to SD-WAN for all of the underlays and overlays.

&nbsp;

<!-- MSG - COMMENT - Ensure we are adding a default to port3 -->

### HUB Static Route Template

**Navigate to:**

***Device Manager>Provisioning Templates>Static Route***
 - Edit the Hub Static Route Template '2H_AP_BGPOL_HUB_Router'
![HUB Static Template](hubstatic1.png?width=950px)
 - Click the Create New 
 - Select IPv4 Static Route
 ![HUB1 Static Template2](createnewroute.png?width=650px)

&nbsp;

Leave the Destination as 0.0.0.0/0.0.0.0 as this is a default route.  The route will be pointed to our Underlay 1 interface or port3.  
A metadata variable will be used for the Gateway Address.  We can call the same one created while working in the SD-WAN template.
 - Type $ in the Gateway Address section and select the (ul1_gateway_IP) variable from the drop down list.
 - Interface - port3
 - Expand the Advanced section
 - Seq-num – change to 100
 - Click 'OK'
![HUB1 Static Template3](hubstaticdefault.png?width=750px)
Result:
![HUB1 Static Template4](hubstaticdefaultok.png?width=750px)
The HUB Static Route Template is now complete.
 - Click ‘OK’

&nbsp;
&nbsp;

### Branch Static Route Template

The Branch Template was preconfigured during the SOT, we will open it to review the configured static route to all SD-WAN Members.

&nbsp;

**Navigate to:**

 ***Device Manager>Provisioning Templates>Static Route***
 - Edit the Branch Static Route Template 'BRANCH_STATIC_ROUTES'

![Branch Static Templates](branchstatictemplate.png?width=950px)


### Now that we have completed the GUI Static Route Templates, take a minute to view the CLI Configuration Preview.