---
title: Lab 2 - BGP Templates Review and Edits
linkTitle: Lab 2 - BGP Templates
weight: 20
---

## HUB BGP Templates Review and Edits

&nbsp;

The SOT created 3 BGP Templates. Lets start by reviewing the HUB Specific templates '2H_AP_BGPOL_HUB1_BGP' and '2H_AP_BGPOL_HUB2_BGP'.
![SOT BGP Templates](bgp.png?width=950px)

&nbsp;

The HUB BGP Templates have a dynamic Neighbor Group configured with an associated Neighbor Range. You will also notice the Neighbor for 172.33.1.1 that we configured earlier in the SOT. We will edit that later in the section. 

![HUB BGP Template 1](bgpneighgr.png?width=950px)

<!-- MSG COMMENT - No longer needed 
&nbsp;

Each Neighbor Group has the IPv4 Route Map In applied to it as we specified during the SOT creation.  The HUBs are also configured to be BGP Route Reflector Clients.
![HUB BGP Template 2](bgpneighgrdetails1.png?width=950px)

&nbsp;

Each HUB orginates routes for its IPSec tunnnel subnets via a BGP network statement. These are the same IP ranges that are used to dynamically hand out tunnels IPs to the branches via mode-cfg.
![HUB1 BGP network statements](bgpneighnets.png?width=950px)
-->

&nbsp;
&nbsp;

### Configuring an eBGP peer to DC Routers

For each Hub, we are going to establish an eBGP peer to a DC Router. In this configuration, we will utilize the 'attribute-unchanged med'. 

BGP MED is a BGP attribute that is not transitive meaning it is typically removed when it crosses an AS Boundry unless this "attribute-unchanged" feature is applied to pass it along. This is how we will signal the branch status to the DC Core. We will use this feature in favor of a route-map to send metrics to the DC Core.

### Edit the Hub1 BGP Template

**Navigate to:**

 ***Device Manager>Provisioning Templates>BGP***
  - Select 2H_AP_BGPOL_HUB1_BGP
  - Click the Edit button

![Branch BGP](hub1bgpedit.png?width=950px)

&nbsp;

Edit Neighbor '172.33.1.1' in the Neighbors Section
![Hub1 BGP Neighbor](hub1neighedit.png?width=950px)

&nbsp;

Fill in the following:
 - Toggle 'Interface' to on and fill in `port2`
 - Toggle 'Update Source' to on and fill in `port2`
 - Change Advertisement Interval to '1'
 - Change Connect Timer to 10
 - Under IPv4 Filtering
   - Toggle on 'Attribute Unchanged' and set to Med
	 - Soft Reconfiguration
	 - Capability: Graceful Restart
	 - Capability: Route Refresh
 - Click OK

![BGP Neighbor](hub1neighedit1.png?width=650px)

Click OK again to complete the BGP Template edit.

&nbsp;
&nbsp;
&nbsp;

### Edit the Hub2 BGP Template

We need to make the same edits to the Hub2 DC Core Neighbor

**Navigate to:**

 ***Device Manager>Provisioning Templates>BGP***
  - Select 2H_AP_BGPOL_HUB2_BGP
  - Click the Edit button

Edit Neighbor '172.33.2.1' in the Neighbors Section

Fill in the following:
 - Toggle 'Interface' to on and fill in `port2`
 - Toggle 'Update Source' to on and fill in `port2`
 - Change Advertisement Interval to '1'
 - Change Connect Timer to 10
 - Under IPv4 Filtering
   - Toggle on 'Attribute Unchanged' and set to Med
	 - Soft Reconfiguration
	 - Capability: Graceful Restart
	 - Capability: Route Refresh
 - Click OK
 
![Hub2 BGP Neighbor](hub2neighedit1.png?width=650px)

Click OK again to complete the BGP Template edit.

&nbsp;

### Review Branch BGP Template

Now let's look at the Branch BGP Template.  The Branch BGP Template has static neighbors configured.  Also notice that the SOT generated metadata variable ‘branch_id’ is used for the Router ID IP address configuration.  This is the same IP configured on the loopback created by SOT.
![Branch BGP neighbors](bgpbranch1.png?width=950px)

&nbsp;

There are two Branch neighbors configured, one for each Hub's BGP-Lo interface.

![Branch BGP neighbors 2](bgpbranch2.png?width=950px)

### Configure Branch LAN Network Statement
<!-- MSG-COMMENT - How do we want to talk about this bug? -->
On the Branch we will add a Network Statement for the Branch LAN network. 

 - Navigate to '2H_AP_BGPOL_BRANCH_BGP' BGP Template and scroll to the Networks
   - Create New Network

![Branch BGP Network](branchbgpnetworkedit.png?width=950px)

 - Enter a '$' into the Prefix Section
 - Click the '+' to add a new Meta Variable

![Branch BGP Network](branchbgpnetworklan1net.png?width=650px)

 - Fill in the Name `lan1_net`
 - Fill in the Default Value - `1.1.1.0`
 - Hit OK

![Branch BGP Network](branchbgpnetworklan1net2.png?width=650px)

 - On the previous screen, click the '+' to add another new Meta Variable

![Branch BGP Network](branchbgpnetworklan1mask1.png?width=650px)

 - Fill in the Name `lan1_mask`
 - Fill in the Default Value - `255.255.255.255`
 - Hit OK

![Branch BGP Network](branchbgpnetworklan1mask2.png?width=650px)

 - Enter the newly created Meta Variables in as the prefix `$(lan1_net)/$(lan1_mask)`

![Branch BGP Network](branchbgpnetworkcreate.png?width=650px)

 - Save the '2H_AP_BGPOL_BRANCH_BGP' Branch BGP Template

![Branch BGP Network](branchbgpnetworksave.png?width=650px)

&nbsp;
&nbsp;

### Now that we have completed the GUI BGP Templates, take a minute to view the CLI Configuration Preview for all 3 BGP Templates.

&nbsp;

This completes the BGP Template Review and Edits section