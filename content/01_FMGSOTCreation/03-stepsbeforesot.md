---
title: Lab 1 - Steps before creating SD-WAN Overlay Template (SOT)
linkTitle: Lab 1 - Steps Before SOT
weight: 15
---

&nbsp;  
&nbsp; 
&nbsp;  
&nbsp; 

## Things we need to do before we create our SD-WAN Overlay Template:

- Create Device Groups - Hubs, Branches, and Branches_DIA
- Add Branches_DIA device group to the Branches device group.
- Join Hub FortiGates to FMG and put them in the Hubs device group.

&nbsp; 

## Create Device Groups in Device Manager:

There are several steps you must do in FortiManager before creating a SDWAN Overlay Template.  Let's get started by creating some Device Groups in Device Manager. 

&nbsp;
**Navigate to:**

***Device Manager>Device & Groups***


 Click the Device Group drop down and select "Create New Group"

![Create New Group1](createdevicegrp.png?width=750px)

 - Enter the group name - ` Hubs `
   - Click the OK button.

![Create New Group1](hubsgroup.png?width=750px)
&nbsp;

 - Repeat the steps and create a ` Branches ` device group.

Result:
![Create New Group1](devicegroups.png?width=750px)

In our lab we will have two variations of Branch configurations.  One set of Branches will have Direct to Internet (DIA) access, and the other will not.  So let's create an additional Device Group called ` Branches_DIA ` and make it a member of our Branches Device Group.

- Repeat the steps and create a ` Branches_DIA ` device group.
- Right Click on our "Branches" device group and select Edit Group.
- Click Add Members
![Edit Branch Group](editbranchgroup.png?width=950px)


- Select the "Branches_DIA" device group.
- Click "Add" 
- Click "OK" on the next page to add the Branches_DIA group as a nested group within the Branches Device Group.

![Edit Branch Group](diaadd.png?width=950px)

- Your Devices and Groups should now look like this:

![Branch Groups](branchesgroups.png?width=650px)

&nbsp;  
&nbsp; 
 ## Add Hub1 and HUB2 to FMG Device Manager
 As mentioned in the prior section, both Hubs 1 and 2, have their interface IPs configured and a static default route pointing to their Underlay default gateway. Both Hubs can communicate with FMG via their MGMT IP addresses.
<!-- MSG-COMMENT - NEED TO UPDATE FNDN PASSWORD SCREENSHOT -->

 Log into HUB1 and HUB2 via the HTTPS or SSH links provided in the FortiDemo Console.  Find the IP address of port1(mgmt) for both Hubs and save for the next step.
 ![FortiDemo Console](fndnhubs.png?width=450px)

 HUB1 and HUB2 can now be added to FMG using their port1 IP addresses.  There are multiple ways to do this, but we are going to discover them from FMG for this lab.

 **Navigate to:** 

 ***FMG>Device Manager>Devices & Groups>Add Device***

Click Add Device then select Discover Device
 ![FortiManager Discover Device](discoverdevice.png?width=950px)


&nbsp; 

- Enter Hub1’s port1 ip address = `192.168.0.3`
- Toggle the button next to the ‘Use Legacy Device Login’ option
- Enter User Name/Password admin/` fortinet `
- Click Next

![Discover HUB1](hub1add.png?width=950px)


&nbsp; 

 - Toggle the button next to the ‘Add to Device Group’ option
 - Select Hubs
 - Click OK to add.
 - Click Next

![Discover HUB1 2 of 3](hub1discover.png?width=950px)


&nbsp;  
&nbsp; 

FMG will then discover HUB1 FGT.  
 - Click Import Later (there are currently no policy or objects configured so we will skip this step) 

![Discover HUB1 2 of 3](hub1importlater.png?width=950px)


 
&nbsp; 

HUB1 should now show as a Managed FortiGate and be a member of the ‘Hubs’ Device Group
![Hub1 Added](hub1added1.png?width=950px)
&nbsp; 
## REPEAT THE ABOVE STEPS TO ADD HUB2 TO FMG

FMG should look like this after adding HUB2 - IP Address `192.168.0.13`

![Hub2 Added](bothhubsadded.png?width=950px)

&nbsp;  
&nbsp; 

**The HUB configurations can be reviewed by clicking on the individual device names in the left hand column of Device Manager.**



 **Navigate to:** 

 ***FMG>Device Manager>Devices & Groups>***

 - Click and select HUB1
 - Click Network
 - Select Interfaces

![Hub1 Interfaces](hub1ints.png?width=950px)

&nbsp;  
&nbsp; 

{{% notice note %}} To create an SD-WAN Overlay Template you must have a Hub model device(s) or “real” Hub FGT(s) joined to FMG with configured underlay interface(s) for VPN termination.  The IP addresses of these interfaces are needed and used to create the Branch IPSec Template.  A device group is all that is needed for the Branch requirement in the SOT.  This is why we created the ‘Branches’ device group in an earlier step. {{% /notice %}}

<!-- MSG-COMMENT - No longer needed on 7.6 BGP on LB
&nbsp;  
&nbsp; 

The following router community-lists and route-maps now exist by default in FMG for use in our SOT generated Templates.  These objects are viewable in the FMG via Policy & Objects.

**Navigate to:**

***FMG>Policy & Objects >Advanced>CLI Configurations>router*** 

![communtiy list](commlist.png?width=650px)
![route map](routemap.png?width=650px)

-->