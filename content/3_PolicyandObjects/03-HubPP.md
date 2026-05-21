---
title: Lab 3 - Hub Policy Package
linkTitle: Lab 3 - Hub PP
weight: 15
---

## Hub Policy Package
<!-- MSG-COMMENTS - We created this during the SOT -->
This lab will walk you through editing the HUB Policy Package that SOT created for you.  This Policy Package will be used for both our HUB1 and HUB2 Fortigates.  We will use the installation target feature to apply specific rules to one or the other while sharing the remaining.

Back in Step 4 of our SD-WAN Overlay template creation, we asked SOT to 'Add Health Check Firewall Policy to Hub Policy Package' and we created the "Hub" policy package for SOT to use.

![Pic19](sotstep4hubfwpp.png?width=750px)

<!-- MSG-COMMENT - NEED to add make notes about the ADVPN Policy being disabled and the need to enable that rule -->
**Let's go take a look at what SOT created for us:**

The SD-WAN Overlay Template, based on your selections in Step 4, creates a Policy Block to allow SLA health checks from all the Branch Fortigates traversing the Overlay Tunnels to the Hub Loopback interfaces, it also allows BGP to establish to the BGP-Lo interface.  This policy block is then applied to the top of the HUB Policy Package you specify.

To see the created Policy Block, we must first turn visibility on, as it is not shown by default.

**Navigate to:**

***FMG>Policy & Objects>Policy Packages***

- Click Tools
- Select Feature Visibility

![Pic19](pofeaturevis1.png?width=750px)

- Select Policy Block
- Click OK

![Pic19](pofeaturevis2.png?width=750px)


You should now see the Policy Block section.  Expand the Policy Block section and the '2H_AP_BGPOL_HBLK'.  Select Firewall Policy.  Here we see the "Health Check - Peering" line of policy that is using the OVERLAYS, HUB-Lo and BGP-Lo Normalized Interfaces along with the Loopback and Overlay Network firewall address objects SOT auto-created. You will also notice a disabled policy for ADVPN that was created by the SOT.

![Pic19](policyblockhc.png?width=950px)

{{% notice tip %}}
The SOT creates an ADVPN policy and sets it to disabled. It is recommended this policy be defined more specifically to the customer's environment. We will modify this policy in a later step to include the Branch_LANS.
{{% /notice %}}

Now let's check out the Hub policy package that SOT has applied this Policy Block to.

- Select Firewall Policy under our Hub Policy Package.

![Pic19](hubpolicy1.png?width=950px)

Along with creating the HUB Policy Package and applying the Health Check Access Policy Block to it, SOT has also assigned both HUB1 and HUB2 as Installation Targets.

![Pic20](hubpolicyinstalltargets.png?width=950px)

**Modify Hub ADVPN Policy Block Policy**

We will now edit and enable the ADVPN Policy created with your Hub Policy Block '2H_AP_BGPOL_HBLK'.

![Hub Policy Block ADVPN 1](hubadvpnpolicyedit.png?width=950px)

Make the following modifications to the policy:
- Source – BRANCH_LANS
- Destination – BRANCH_LANS
- 'Enable This Policy' - Enabled

![Hub Policy Block ADVPN 1](hubadvpnpolicycomplete1.png?width=750px)

![Hub Policy Block ADVPN 1](hubadvpnpolicycomplete2.png?width=750px)

{{% notice note %}}
You will recieve a notice stating changes made to policy block will be reflected in all policy packages which are using this policy block. Click OK since this is expected behavior.

![Hub Policy Block ADVPN](hubadvpnpolicyconfirm.png?width=750px)

{{% /notice %}}
**Create Additional Hub Policies**


Navigate back to the *Hub Policy > Firewall Policy* 

We will now edit the Hub Policy package to add additional policies for SDWAN_IN, SDWAN_OUT to the DC Core.

Starting with ` SDWAN_IN `, let’s allow traffic from the Branches to the HUB datacenter networks defined in the previous section.

- Within the HUB Firewall Policy, click Create New. 

![Pic31](hubpolicycreatenew.png?width=950px)

Fill in the policy with the following information:

- Name – ` SDWAN_IN `
- Schedule - always
- Action – Accept
- Incoming Interface – OVERLAYS
- Outgoing Interface – port2
- Source – BRANCH_LANS
- Destination – DC_NET
- Service – ALL
- Click OK

![Pic31](hubsdwanin.png?width=750px)

The SDWAN_IN rule covers both HUBs as the DC_NET address object has mappings for both HUB1 and HUB2.  The "Install On" setting does not need to be specified as this rule will be used by both Installation Targets.

![Pic32](sdwaninresult.png?width=950px)

{{% notice note %}}
If a specific device is NOT specified in the "Install On" column, and displays "Installation Targets", that line of policy will be applied to ALL installation targets defined.
{{% /notice %}}

&nbsp; 


Most customers will need to allow traffic from their DCs to the Branches, which will be our next rule.

- Select rule #3 ‘SDWAN_IN’
- Right Click
- Select ‘Clone Reverse’

![Pic33](sdwaninclonerev.png?width=950px)

Notice the From and To and the Source and Destination has been swapped.

![Pic34](sdwaninclonerev2.png?width=950px)

**Edit the newly cloned rule and name it ` SDWAN_OUT `.**

The HUB firewall policy should now look like the image below.

![Pic35](hubpolicywithsdwanout.png?width=950px)

<!-- MSG-COMMENT - Done with policy block

The next rule we need to create is to allow Branch to Branch traffic on the HUB policy.  Although we are enabling ADVPN in our SOT generated configurations, the first couple packets must traverse through the HUB so it can then orchestrate the shortcut tunnel establishment.

- Name – ` BRANCH_TO_BRANCH `
- Incoming Interface – VPN1 and VPN1-2
- Outgoing Interface – VPN1 and VPN1-2
- Source – BRANCH_LANS
- Destination – BRANCH_LANS
- Service – ALL
- Schedule - always
- Action – Accept
- Click OK

![Pic36](branch2branch.png?width=750px)

-->

The final rule will be for the Brances Device Group, that doesn't allow for Direct Internet Access (DIA), to allow Branch to Internet traffic thru our HUBs.

- Name – ` BRANCH_TO_INTERNET `
- Schedule - always
- Action – Accept
- Incoming Interface – OVERLAYS
- Outgoing Interface – port3
- Source – BRANCH_LANS
- Destination – all
- Service – ALL
- NAT - Toggle Enable
- IP Pool Configuration - Use Outgoing Interface Address
- Click OK

![Pic37](hubpolbranch2inet.png?width=750px)


The Hub policy creation is now complete.  Please ensure your policy looks like the screen capture below.

![Pic37](hubppend.png?width=950px)


### Now that we have completed the HUB Policy Package, let's move to the next section and modify the policy package for the Branches.