---
title: Lab 3 - Branch Policy Package
linkTitle: Lab 3 - Branch PP
weight: 20
---

## Modifying Branch Policy Package

In this section we are going to modify the 'Branch' Policy Package that will be used on the Branch Fortigates. 

Back in Step 4 of our SD-WAN Overlay template creation, we asked SOT to 'Add Health Check Firewall Policy to Branch Policy Package' and we created the "Branch" policy package for SOT to use.

![Hub Policy SOT](sotstep4branchfwpp.png?width=750px)

We will now add additional policies to the Branch policy for DIA_OUT, and SDWAN_IN / SDWAN_OUT

**Navigate to:**

***FMG>Policy & Objects>Policy Packages***

- Select Branch > Firewall Policy


![Branch Firewall Policy Empty](branchfwpolicyempty.png?width=950px)


In the SD-WAN Provisioning Template we created a DIA (Direct Internet Access) SD-WAN Service Rule for our Branches_DIA Device Group.  It will need a corresponding firewall policy rule.  Let's create that.

Before we create the policy, let's go assign our Device Groups as Installation Targets.

- Select the BRANCH Policy Package
- Select Installation Targets 
- Click Edit 
- Select Device Group - Branches_DIA 
- Click the Right Arrow to move them to the Selected Entries section
- Click OK

![Branch Policy Install Targets](branchinstalltargets.png?width=950px) 

Result:

![Branch Policy Install Targets](branchppinstalltargetsfinal.png?width=750px) 

Within the newly created BRANCH Policy Package, select Create New.

![Branch DIA Policy Create](branchppnewpolicy.png?width=950px)  

- Name – ` DIA_OUT `
- Schedule - always
- Action – Accept
- Incoming Interface – TRUSTED_ZONE *(Normalized Interface we created for the zone created by the Branch System Template)*
- Outgoing Interface – WAN1 and WAN2 *(sdwan zones created by SOT)*
- Source – BRANCH_LANS
- Destination – RFC1918-GRP
- **Negate Destination – Toggle On**
- Service – ALL
- **NAT - Toggle On**
Scroll to Security Profiles
- **Security Profile Status – Toggle On**
- Profile Type - Use Standard Security Profiles
- Application Control - default
- SSL/SSH Inspection - certificate-inspection
- Click OK

![Branch DIA Policy Create](brdiaoutpolicy.png?width=750px)

![Branch DIA Policy Result](brdiaoutpolicyresult.png?width=950px)

{{% notice note %}}
The Application Control Profile is required for application based SD-WAN Steering.

{{% /notice %}}

We have created the DIA_OUT policy, now we need to assign it ONLY to the Branches_DIA Device Group.  We do this by changing the Install On setting within the policy.  

- Right Click in the Install On column
- Select Edit Install On

![DIA Install Targets 1](editinstallondia.png?width=950px)

- Move the Branches_DIA device group by selecting and clicking the right arrow to move it to the Selected Entries section.
- Click OK

![Branch DIA Install Target 1](installedonbranchesdia.png?width=750px)

Result:

![Branch DIA Install Target Result](diaoutfinal.png?width=950px)

&nbsp; 


Create 2 more policies as shown below to allow traffic to and from the overlays. ` SDWAN_OUT ` `SDWAN_IN`.  No installation targets will need to be specified as they will apply to all Installation Targets (both Branch device groups).

**Don’t forget about the ‘Clone Reverse’ feature as it can be used to save time.**

Result:

![Pic43](branchpolfinal.png?width=950px)



&nbsp;
&nbsp;

# END OF LAB 3