---
title: Lab 2 - SOT Created Templates
linkTitle: Lab 2 - SOT Created Templates
weight: 5
---

&nbsp;  
&nbsp; 

---

## What Did the SOT Create?

Let's review which Templates were generated now that the SD-WAN Overlay Template (SOT) creation is complete.
- IPsec Tunnel Templates – HUB1, HUB2, and BRANCH
- SD-WAN Template – BRANCH, and HUB
- BGP Templates - HUB1, HUB2, and BRANCH
- CLI Templates - HUB1, HUB2, and BRANCH (Jinja) 
- CLI Template Groups - HUB1, HUB2, and BRANCH
- Static Route Templates - BRANCH, and HUB
- Template Groups - HUB1, HUB2, and BRANCH

&nbsp;  
&nbsp; 

### **IPsec Tunnel Templates** - HUB1, HUB2, and BRANCH

![SOT IPsec Templates](ipsectemplates.png?width=950px)

{{% notice note %}} Don't be alarmed that none of these provisioning templates are assigned to a Device or Device Group.  This is taken care of by the Template Groups created by SOT.  The **Device Groups** are assigned to the **Template Groups**. {{% /notice %}}

&nbsp;  
&nbsp; 

### **SD-WAN Template** - Branch, and HUB
<!-- MSG-COMMENT - The new screenshot has the HUB SDWAN - Do we want to talk about the addition of HUB SDWAN on 7.6 vs 7.4? -->
The SD-WAN Template named SDWAN_Branch_2UL was created by SOT.  Notice the Branch underlay interfaces you created, ul1 and ul2, along with the VPN Overlay interfaces created in the IPSec Template, have been added as SD-WAN Members.  We will discuss this more in an upcoming section where we will complete this configuration.
![SOT SDWAN Template](sdwantemplate.png?width=950px)

&nbsp;  
&nbsp; 

### **BGP Templates** - HUB1, HUB2, and BRANCH
![SOT BGP Template](bgptemplates.png?width=950px)


&nbsp; 

### **CLI Templates** - HUB1, HUB2, and BRANCH(Jinja)

SOT has created two CLI templates for HUB1, HUB2, and BRANCH.  The Branch CLI Template is Jinja that uses a SOT generated metadata variable "branch_id".
![SOT CLI Template](clitemplates2.png?width=950px)

### **CLI Templates Groups** - HUB1, HUB2, and BRANCH

The screen capture below shows the 3 CLI Template Groups and the CLI Templates that are members of those groups. 
![SOT CLI Template](cligroups.png?width=950px)

{{% notice note %}} The CLI Template Groups are what is added to the Template Groups that devices are assigned to.{{% /notice %}}




&nbsp;  
&nbsp; 

### **Template Groups** - HUB1, HUB2, and BRANCH
SOT created Template Groups are where the Device or Device Group assingments are made.
![SOT Template Groups](tempgroups.png?width=950px)



