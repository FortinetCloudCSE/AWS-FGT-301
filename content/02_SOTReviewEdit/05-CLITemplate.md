---
title: Lab 2 - CLI Templates Review and Edits
linkTitle: Lab 2 - CLI Templates
weight: 25
---

## CLI Templates and CLI Template Group Review and Edits

The SOT creates a CLI Templates for HUB1, HUB2, and BRANCH.  It then creates a CLI Template Group for each and puts the CLI Template into it.  The CLI Template Group is what gets assigned to the Template Group.  

No edits are needed to the SOT generated CLI Templates for our lab but we will be importing an additional Branch_Supplement template.
![CLI Template Groups](cligroups.png?width=950px)


**HUB1 CLI Templates**

The HUB CLI Templates configure Loopback interfaces that are dynamically assigned to each Hub.  The HUB1-Lo is the interface used for health-checks and is the same on both hubs, the BGP-Lo is unique to each hub and is used for Branch to Hub BGP peering, and the BGP Network ID on the hubs. 
![HUB1 CLI Template](hub1cli1.png?width=950px)

A second HUB CLI Template is created that configures the location-id.
![HUB1 CLI Template](hub1cli2.png?width=950px)

**HUB2 CLI Template**

There are similar CLI templates for HUB2.  Please open and see their contents. Note that the HUB2-Lo will be the same IP address as HUB1-Lo for health-checks. 



**Branch CLI Template**

There are two Branch CLI Templates created by the SOT.  The first uses Jinja and the ‘branch_id’ metadata variable to calculate the Branch Loopback IP address and location ID.  
![Branch SOT generated CLI Template](branchcli1-2.png?width=950px)

A second Branch CLI Template is created and configures the router ID, sets the IPSec Phase1 exchange IPs, and the source address used on the SD-WAN members.
![Branch SOT generated CLI Template](branchcli2.png?width=950px)

&nbsp;
&nbsp;

### Create Branch Supplement CLI Template
The SOT creates a majority of the necessary configuration for a working SD-WAN setup, but additional configuration is needed.  The extra configuration will be added to this lab with a new CLI Template.  A ‘Branch_Supplement_CLI’ CLI Template has been created for you and can be found in the HELPFUL RESOURCES section in the left hand pane.  

Download the ‘Branch_Supplement.txt’ from the HELPFUL RESOURCES section of this page. Open it and review.  Make note of the additional metadata variables that are used here.  
You will now import the downloaded file to FMG.

**Navitgate to:**

***Device Manager>Provisioning Templates>CLI***
- Click ‘+ Create New’ button
- Click ‘CLI Template’
![Create New CLI Template](createcli.png?width=950px)

&nbsp;

 - Enter Template Name – ` Branch_Supplement_CLI `
   - Select Type 'CLI'
   - Select Position 'Post-VDOM Copy'
 - Copy the contents of the Branch_Supplement.txt (Link in the **HELPFUL RESOURCES** in lower left hand pane) file and paste into the Script Details section:
 ![New CLI Template Paste](reatenewclipaste2.png?width=950px)

<!-- MSG-COMMENT - IMPORTANT - Fix screenshots on next run because they didn't include the LAN METAs -->
While reviewing the CLI Template you noticed the use of metadata variables we currently do not have configured in FMG.  
Click ‘OK’ and see what happens.
![Missing MVs](newmetasok.png?width=650px)

FMG notices that the variables referenced in the template do not exist in the FMG database.  

Click ‘OK’.

FMG is now going to create the metadata variables for you.  Click the ‘Create’ button at the bottom.
You should get a green banner across the top stating the new variables have been created.  
![New MVs](createmissingmetas.png?width=750px)

&nbsp;

Try to save the new CLI template now that the variables are created. 

Click ‘OK’

Result:
![Create New CLI Template2](newcliadded.png?width=950px)

The additional CLI template required for the Branches has now been created.  The template still needs to be applied though.

This new CLI template will not be applied directly to the Branch FGTs nor to the Branch device group.

The screen shot below shows the **Template Groups**.  Each Template Group has individual Templates assigned to it.  You can see that Template Group ‘2H_AP_BGPOL_BRANCH’ has the CLI Template Group ‘2H_AP_BGPOL_BRANCH_CLIGRP’ assigned to it.  The **Template Group** is assigned to the **Branches Device Group**.
The new Branch Supplement template needs to be assigned to the Branch CLI Template Group.
![Template Groups](brtemplategroup.png?width=950px)

&nbsp;

- Navigate back to the CLI Templates section
  - Edit the ‘2H_AP_BGPOL_BRANCH_CLIGRP’ by selecting it and clicking the ‘Edit’ button.
![Branch CLI Group Edit](branchcligroupedit.png?width=950px)
  - Click the ‘+’ next to Members.
  - Add the ‘Branch_Supplement_CLI’ CLI template as a member.
  - Click OK.
  - Click OK to update the CLI Group

![Add Supplemment](updatebrcligrp.png?width=950px)

### You will not be able to preview this template since we have not added any branches yet. You have now completed the CLI Template Review and Edits.  Let's go configure some Static Routes now!!!!
