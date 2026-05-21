---
title: Lab 1 - Create SD-WAN Overlay Template (SOT)
linkTitle: Lab 1 - Create SOT
weight: 20
---


&nbsp; 
&nbsp;  
&nbsp; 

## Create New SD-WAN Overlay Template (SOT)

Before we can get started, in FMG version 7.6.4, the SD-WAN Overlay and BGP Provisioning Templates are not visible by default.  So let's go change that. 

**Navigate to:**

***FMG>Device Manager>Provisioning Templates>Feature Visibility***

 - Check the box next to the 'BGP' and 'SD-WAN Overlay' Templates
 - Click the OK button.

![Enable SOT](enablesot.png?width=950px)

You should now see both listed now in Provisioning Templates

![Creatte New SOT](bgpandsdwano.png?width=950px)

Now that we can see the SD-WAN Overlay Template, let's get started creating one of our own.

**Navigate to:**

***FMG>Device Manager>Provisioning Templates>SD-WAN Overlay***

![Creatte New SOT](createnewsot1.png?width=950px)

&nbsp;  
&nbsp; 

### Complete Step 1 of 5 - Region Settings:


Enter Name - ` 2H_AP_BGPOL ` - This name was chosen because there will be 2 Hubs, the Branches will have 2 underlays, and ADVPN will be enabled. 

Select new Topology - Dual Hub (Primary & Secondary)

Expand Advanced
 - Loopback IP Address - ` 169.254.252.0/255.255.254.0 `
 - BGP-AS Number - ` 65000 `
 - BGP on Loopback - Toggle on
 - Dynamic BGP - Toggle On
 - Auto-Discovery VPN - ADVPN 2.0

Click Next to complete step 1
![SOT 1 of 5](sotstep1.png?width=950px)


&nbsp; 
&nbsp;  
&nbsp;

### Complete Step 2 of 5 - Role Assignment:
In the HUB section
 - Primary HUB - Click to select HUB1 from the drop-down list  
    - Add Cost of 10
 - Secondary HUB - Click to select HUB2 from the drop-down list  
    - Add Cost of 20

In the Branch section
 - Device Group Assignment - Click to select Branches from the drop-down list
 - Toggle Automatic Branch ID Assignment

Click Next of complete step 2
![SOT 2 of 5](sotstep2.png?width=950px)


&nbsp;
&nbsp;  
&nbsp;

### Complete Step 3 of 5 - Network Configuration:
In the HUB section
 - **Primary Hub**
   - WAN Underlay 1
     - Enter 'port3'
   - WAN Underlay 2
     - Click the 'x' in the action section as our Hubs only have 1 underlay.
   - Network Advertisement - Leave Connnected selected
    - Click + under Interface and add port2
![sotprihubnetwork](sotprihubnetwork.png?width=950px)
 - Expand Advanced
  - Select Create New under Neighbors
  
![sotprihub](sotprihub.png?width=950px)

&nbsp;
 - Fill in Neighbor IP ` 172.33.1.1 `
 - Fill in Remote AS ` 65001 `
Click OK to add Primary HUB Neighbor
  ![sotprihubbgpneighbor](sotprihubbgpneighbor.png?width=950px)

Ensure your Primary HUB matches with the following settings
  ![sotprihubwithneighbor](sotprihubwithneighbor.png?width=950px)


 - **Secondary Hub**
   - WAN Underlay 1
     - Enter 'port3'
   - WAN Underlay 2
     - Click the 'x' in the action section as our Hubs only have 1 underlay.
   - Network Advertisement - Leave Connnected selected
    - Click + under Interface and add port2
    ![sotprihubnetwork](sotsechubnetwork.png?width=950px)
 - Expand Advanced
  - Select Create New under Neighbors
  
  ![sotsechub](sotsechub.png?width=950px)

&nbsp;
 - Fill in Neighbor IP ` 172.33.2.1 `
 - Fill in Remote AS ` 65001 `
Click OK to add Secondary HUB Neighbor
  ![sotsechubbgpneighbor](sotsechubbgpneighbor.png?width=950px)

Ensure your Secondary HUB matches with the following settings
  ![sotsechubwithneighbor](sotsechubwithneighbor.png?width=950px)

 - **Branch Route Maps** 
   - In the 7.6 BGP on Loopback Branch Route Maps are no longer needed, signaling will be setup later on in the guide
![sotrm](sotrm1.png?width=950px)

In the Branch Section:
 - **WAN Underlay 1**
   - Type $
   - Then click the '+' to create a new metadata variable

![SOT Branch ul1](branchul1.png?width=950px)

   - Enter **ul1** in the Name field
   - Click 'OK"

![SOT Branch ul1-2](ul1.png?width=950px)

   - Select the newly created (ul1) metadata variable from the drop-down list for WAN Underlay 1.
![SOT Branch ul1-2](ul1select.png?width=950px)

 - **WAN Underlay 2**
   - Follow the above steps and create a new metadata variable name 'ul2' and select it for your WAN Underlay 2.
![SOT Branch ul1-ul2 Select](ul1ul2select.png?width=950px)

 - **Branch WAN Underlay Cost and Transport Group**
   - Fill in Cost and Transport Groups
    - WAN Underlay 1
      - Cost '0'
      - Transport Group '1'
    - WAN Underlay 2
      - Cost '1'
      - Transport Group '1'

![SOT Branch Underlay Cost and Transport](branchulcosttransport.png?width=950px)

 - **Network Advertisement**
    - Select Connected
      - We will add a network statement later in the BGP Templates
    - Click Next to complete step 3

![SOT Branch Network](branchnetworkadv.png?width=950px)
<!-- MSG-COMMENT - No longer needed in 7.6.x
- **Advanced Branch Section**
  - Click the '>' next to Advanced to expand the section
  - Click the radio button next to 'Route map out' for each overlay to enable the feature
    - Select 'Priority_9999' from the drop down
  - Click the radio button next to 'Route map out preferable' for each overlay to enable the feature
    - Select 'Priority_1' from the drop down
        - Set the "route map out preferable' priorities for each overlay as shown in the screenshot.
  - Click Next to complete step 3
![SOT Branch Route-maps](rmopref.png?width=950px)
-->


&nbsp;  
&nbsp; 
&nbsp;  
&nbsp;

### Complete Step 4 of 5 - SD-WAN Template Options:
- Click the radio button next to 'Add Overlay Objects to SD-WAN Template'
  - Click on '+' to create an SD-WAN Template
![SOT SD-WAN Radio](step4sdwan.png?width=950px)
- Name your SD-WAN Template - ` SDWAN_Branch_2UL `
- Toggle the 'SD-WAN Status' radio button to 'on'
- Clicl OK at the bottom of the screen
![SOT Name SD-WAN](sdwanname.png?width=950px)
- Select the newly created SD-WAN Template from the drop-down list
![SOT Select SD-WAN](nameselect.png?width=950px)
&nbsp;
- Click the radio button next to 'Add Overlay Interfaces and Zones'
- Click the radio button next to 'Add Health Check Servers for Each HUB as Performance SLA'
- Click the radio button next to 'Add Overlay Objects to Hub SD-WAN Template'
![SOT Create Hub SD-WAN](sothubsdwancreate.png?width=950px)

- Name your SD-WAN Template - ` SDWAN_Hub `
- Toggle the 'SD-WAN Status' radio button to 'on'
- Clicl OK at the bottom of the screen
![SOT Create Hub Template SD-WAN](sothubsdwantemplatecreate.png?width=950px)

- Select the newly created Hub SD-WAN Template from the drop-down list
- Click the radio button next to 'Normalize Interfaces'
- Click the radio button next to 'Add Health Check Firewall Policy to Hub Policy Package'
  - Click the '+' to create a new Firewall Policy
  ![SOT Name SD-WAN Hub](hubfwpolicy.png?width=950px)
    - Enter 'Hub' for the Name
    - Click OK
    ![SOT Name SD-WAN Hub](namehubfwpol.png?width=950px)
      - Select Hub
      ![SOT Name SD-WAN](selecthub.png?width=950px)

- Click the radio button next to 'Add Health Check Firewall Policy to Branch Policy Package'
  - Click the '+' to create a new Firewall Policy
  ![SOT Name SD-WAN Branch](branchfwpolicy.png?width=950px)
    - Enter 'Branch' for the Name
    - Click OK
    ![SOT Name SD-WAN Branch](namebranchfwpol.png?width=950px)
      - Select Branch
      ![SOT Name SD-WAN Branch](selectbranch.png?width=950px)

- Click the radio button next to 'Add Default Route to SD-WAN Zones in Static Route Template'
  - Click the '+' to create a new Static Route Template
  ![SOT Name SD-WAN Branch Route Template](branchroutetemplate.png?width=950px)
    - Enter ` BRANCH_STATIC_ROUTES ` for the Name
    - Click OK
    ![SOT Name SD-WAN Branch Route Template](namebranchroutetemplate.png?width=950px)
      - Select BRANCH_STATIC_ROUTES
      ![SOT Name SD-WAN Branch Route Template](selectbranchroutetemplate.png?width=950px)

Click Next to complete step 4
![SOT 4 of 5](next1.png?width=950px)


&nbsp;  
&nbsp; 
&nbsp;  
&nbsp;

### Complete Step 5 of 5 - Summary:
Review Configuration Settings
![SOT 5 of 5](summary1.png?width=950px)
![SOT 5 of 5.1](summaryfinish.png?width=950px)
Click Finish

**The '2H_AP_BGPOL'  SD-WAN Overlay Template and all accompanying templates have now been created!**
![SOT End](sotfinal.png?width=950px)

&nbsp;  
&nbsp;

# END OF LAB 1
