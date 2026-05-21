---
title: Lab 3 - Review and Create Normalized Interfaces
linkTitle: Lab 3 - Review and Create Normalized Interfaces
weight: 5
---

## Review and Create Normalized Interfaces

Lab1 had you create your SD-WAN Overlay Template (SOT).  Normalized Ineterfaces were auto-created by SOT when it made the SD-WAN Template.  Let’s review those interfaces.

**Navigate to:**

***FMG>Policy & Objects>Normalized Interface***

Click the Normalized Interface section and type "SDWAN" in the search bar to show the Normalized Interfaces the SDWAN Overlay Template created.

Here we can see the Normalized Interfaces created for you by SOT in Step 4 because we toggled on the button next to "Normalized Interfaces".  
![Pic1](yestonormalints.png?width=750px)

The SD-WAN Zones created by the SDWAN Template.  The Loopback interfaces created in the HUB1, HUB2, and Branch CLI Templates.  Also, the HUB Overlay VPN interaces created in the HUB IPSec Templates.  These objects can now be referenced when creating the firewall Policy Packages.

![Pic1](sotnormalints.png?width=950px)

For our Branch Policy Package, we are going to create a TRUSTED_ZONE Normalized Interface.  If you remember, we are creating the TRUSTED_ZONE zone in the System Template.

From the current Normalized Interface screen:
 - Click Create New
 - Name - ` TRUSTED_ZONE `
 - Click Create New under Per-Platform Mapping

 ![Pic2](createnormalint.png?width=750px)

Input:
 - Matched Platform – Fortigate-VM64-KVM
 - Mapped Interface Name – ` TRUSTED_ZONE `
 
 Click OK

 ![Pic3](createnormalintmap.png?width=750px)

Confirm the screen matches the image below.  
Click OK

 ![Pic4](createnormalintdone.png?width=750px)

 Result:
![Pic3](normalintdone.png?width=850px)

### This is all we need to do for our Normalized Interfaces.  Let's proceed to the next section and create some address objects.