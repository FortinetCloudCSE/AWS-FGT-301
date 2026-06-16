---
title: "SD-WAN Demo Preparation Steps"
linkTitle: "Section 2: SD-WAN Demo Preparation Steps"
chapter: false
weight: 2
---

&nbsp;
&nbsp; 
&nbsp;
&nbsp;

## Review and Edit Existing SD-WAN Overlay Template (SOT)

  {{% notice info %}}
This environment already is preconfigured with an SD-WAN Overlay Template (ie SOT), Policy Packages, SDWAN Rules, etc. In this section we are going to review, edit, and deploy the existing SOT to the Hub and Branch FGTs. Normally if you are starting from scratch, these are prerequisites below that would need to complete before creating the SOT.

- Create device groups for Hub and Branch FGTs
- Configure Hub FGTs with interface IPs and routing needed for reaching the underlay default gateway
- Add all Hub FGTs to FortiManager and place them in the hub device group
- Enable SOT and BGP provisioning template visibility in the FMG under FMG>Device Manager>Provisioning Templates>Feature Visibility
  {{% /notice %}}



**Login** to your FMG using `scw-region1-fmg-login-url` in your [**environment outputs**](../0_labprep/02_logistics)

***Navigate: FMG → Device Manager → Provisioning Templates → SD-WAN Overlay***
 - Select and Edit the existing SOT

![image](sot.png?width=950px)

### Review Step 1 of 5 - Region Settings:

The name 2H_AP_BGPOL was chosen because there will be 2 Hubs, the Branches will have 2 underlays, and ADVPN will be enabled. 

For Select New Topology we have selected - Dual Hub (Primary & Secondary)

Expand Advanced
 - Loopback IP Address - 169.254.252.0/255.255.254.0
 - BGP-AS Number - 65000
 - BGP on Loopback - Toggled on
 - Dynamic BGP - Toggled On
 - Auto-Discovery VPN - ADVPN 2.0

Click Next to complete step 1
![image](sotstep1.png?width=950px)

&nbsp;  
&nbsp; 
&nbsp;  
&nbsp;

### Review Step 2 of 5 - Role Assignment:
In the HUB section
 - Primary HUB - HUB1 is selected
    - The Cost is 10
 - Secondary HUB - HUB1 is selected
    - The Cost is 10

{{% notice info %}}
The link cost can be explicitly configured in a more granular way for in and out of SLA scenarios in the SD-WAN provisioning template. We will review that in the SD-WAN Configuration Overview section. This will allow each branch to reach out to each region's Hub FGT deployment directly to avoid cross regional data charges in a normal state. IE branch traffic for region1 destinations goes to region1-hub1, and region2 destinations goes to region2-hub2.
{{% /notice %}}


In the Branch section
 - Device Group Assignment - Branches is selected
 - Automatic Branch ID Assignment is enabled

{{% notice info %}}
Automatic Branch ID Assignment uses a metadata variable **branch_id** for configuring unique values in the provisioning templates (BGP router id, loopback IP, etc) for each branch location. Each Branch FGT will have a unique value for this metadata variable. We will get into metadata variables in more detail later in this section.
{{% /notice %}}

Click Next of complete step 2
![image](sotstep2.png?width=950px)

&nbsp;  
&nbsp; 
&nbsp;  
&nbsp;

### Review and Edit Step 3 of 5 - Network Configuration:
In the HUB section
 - **Primary Hub**
   - WAN Underlay 1
     - 'port1' is selected
	 - **Update** the **Override IP** to the public IP listed in `scw-region1-hub1-login-url` in your [**environment outputs**](../0_labprep/02_logistics)
   - Network Advertisement - Static is selected and below it is a CIDR the Primary Hub is in
     - No interface is selected as the Hub FGTs will advertise BGP routes learned from Cloud WAN to the Branches
![image](sotprihubnetwork.png?width=950px)

 - **Secondary Hub**
   - WAN Underlay 1
     - 'port1' is selected
	 - **Update** the **Override IP** to the public IP listed in `scw-region2-hub2-login-url` in your [**environment outputs**](../0_labprep/02_logistics)
   - Network Advertisement - Static is selected and below it is a CIDR the Secondary Hub is in
![image](sotsechubnetwork.png?width=950px)

{{% notice info %}}
We need to select **Override IP** as the Hub FGTs are running in AWS and are using [**Elastic IPs**](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-eips.html) (EIP) for a public static IP address for internet communications. These EIPs behave like an upstream NAT rule so the FGTs and FMG will not see this public IP on the FGT's interface configuration. Thus we are overriding the IP value so the Branch FGTs point to the correct public IP to reach the Hubs.

You can technically configure multiple network interfaces or ENIs (Elastic Network Interface) on an EC2 instance in AWS. However, there is a limit of how many interfaces you can attach based on the instance type and size. Generally, if you want more than 4x interfaces in AWS, you will need to use a *.4xlarge or bigger instance size. Reference [**AWS Documentation**](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AvailableIpPerENI.html) for more information.

It is generally recommended to have a public and private data plane interface and even dedicated management interface if desired. However, there is typically no need for multiple public data plane interfaces as AWS provides resiliency and automatic recovery for any hardware or software failures. Also, a design using multiple availability zones provides resiliency and automatic recovery for availability zone failures.

In this demo the Hub FGTs are actually unicast FGCP Active-Passive clusters deployed across 2x availability zones to provide a more fault tolerant design and automatically failover data plane routes and EIPs between the FGTs.
{{% /notice %}} 

&nbsp;

 - **Branch Route Maps** 
   - In the 7.6 BGP on Loopback Branch Route Maps are no longer needed, signaling will be setup later on in the guide
![image](sotrm1.png?width=950px)

{{% notice info %}}
SD-WAN In and Out of SLA Priorities are configured under each SD-WAN Member and translated to BGP MED on the HUB. We explicitly set these values later in the SD-WAN Configuration Overview section. Reference [**Fortinet Documentation**](https://docs.fortinet.com/document/fortigate/7.6.0/sd-wan-new-features/460015/map-sd-wan-member-priorities-to-bgp-med-attribute-when-spoke-advertises-routes-using-ibgp-to-hub-7-6-1) for more information.
{{% /notice %}}

In the Branch Section we have the following selected:
 - **WAN Underlay 1**
   - '$(ul1)' is selected
   - Cost is 0
   - Transport Group is 1
 - **WAN Underlay 2**
   - '$(ul2)' is selected
   - Cost is 1
   - Transport Group is 1
 - Network Advertisement - Connected selected
   - 'port3' is selected as this is our LAN interface

![image](branchul1.png?width=950px)

{{% notice info %}}
The Branch underlays are specified as [**metadata variables**](https://docs.fortinet.com/document/fortimanager/7.6.6/administration-guide/478643/adom-level-metadata-variables). This allows us to define different interfaces for various Branch FGTs deployed in an environment. These variables can actually be used in scripts, templates, firewall address objects, IP pools, VIPs, and more.
{{% /notice %}}

{{% notice info %}}
Transport groups are used to define what overlay interfaces branches can use to build ADVPN shortcut tunnels on.  Only overlays with matching transport groups can build dynamic tunnels with each other, ie overlay shortcuts.  For example UL1 could be general broadband in transport group 1 and UL2 could be a private path like MPLS or Direct Connect in transport group 2.
{{% /notice %}}

&nbsp;  
&nbsp; 
&nbsp;
&nbsp;

### Complete Step 4 of 5 - SD-WAN Template Options:
In the next section, we have already selected existing templates (for SD-WAN and Static Routes) and specified policy packages (for Hub and Branch FGTs). If you were running through this for the first time, you can create new templates and policy packages right here in the SOT wizard.

- For example, you could click a drop down list and click on '+' to create an SD-WAN Template
![image](step4sdwan.png?width=950px)

- However these are already selected for you and should look like this. We will review what all is created by the SOT in the next secion, SD-WAN Configuration. Click Next to complete step 4.
![image](step4bsdwan.png?width=950px)

&nbsp;
&nbsp; 
&nbsp;
&nbsp;

### Complete Step 5 of 5 - Summary:
Review the configuration settings and click finish to save the changes made (ie Override IP values).
![image](summaryfinish.png?width=950px)

&nbsp;
&nbsp;
&nbsp;
&nbsp;

### Review and Edit Metadata Variables
Before deploying the provisioning templates and policy packages to the Hub and Branch FGTs, we need to review and update a few metadata variables we are using.

**Navigate to:**

***FMG → Device Manager → Device & Groups → Managed FortiGate***
  - Right click either Branch1 or Branch2 FGT and select Edit Variable Mapping
  - Notice the variables **$(ul1)** and **$(ul2)** are defined as 'port1' and 'port2'
  - Click OK to close the window
![image](branchmv1.png?width=950px)
![image](branchmv2.png?width=950px)

  - For the Hub FGTs we are using the variables below in a CLI script to configure the BGP neighbors to peer with Cloud WAN CNEs
  - **Edit each Hub FGT and update the variable mapping values using the values in your** [**environment outputs**](../0_labprep/02_logistics)
  - **Click OK to save the changes** and close the window

{{% notice tip %}}
If you do not click the check mark next to the variable mapping the value is **NOT** saved.  This is easily missed so ensure you do this.
{{% /notice %}}
  
  FGT | Variable Name | Mapping Value | Environment Output
  ---|---|---|---
  Hub1 | cwan_peer1_ip1 | 100.64.1.a | hub1_fgt_cwan_connect_peer1_address1
  Hub1 | cwan_peer1_ip2 | 100.64.1.b | hub1_fgt_cwan_connect_peer1_address2
  Hub1 | cwan_peer2_ip1 | 100.64.1.c | hub1_fgt_cwan_connect_peer2_address1
  Hub1 | cwan_peer2_ip2 | 100.64.1.d | hub1_fgt_cwan_connect_peer2_address2
  Hub2 | cwan_peer1_ip1 | 100.64.2.a | hub2_fgt_cwan_connect_peer1_address1
  Hub2 | cwan_peer1_ip2 | 100.64.2.b | hub2_fgt_cwan_connect_peer1_address2
  Hub2 | cwan_peer2_ip1 | 100.64.2.c | hub2_fgt_cwan_connect_peer2_address1
  Hub2 | cwan_peer2_ip2 | 100.64.2.d | hub2_fgt_cwan_connect_peer2_address2
  
  - When finished, you should have a list of **unique IPs for each Hub FGT**. The last octet of the IPs will be different in your environment.
![image](hubmv1.png?width=950px)
&nbsp;
![image](hubmv2.png?width=950px)

**Navigate to:**

***FMG → Device Manager → Provisioning Templates → 'Click three dots next to static route to expand' → CLI***
  - In the CLI Templates list, find the template **Hub_Cloud_WAN_BGP** and notice that the cwan metadata variables are being referenced here
  - Select and edit the template to view how the variables are being referenced in a CLI script
  - Both Hubs are using the same CLI script but have different values being used for each Hub FGT
  - Click OK to close the window
![image](cwanbgp1.png?width=950px)
&nbsp;
![image](cwanbgp3.png?width=950px)

&nbsp;
&nbsp;
&nbsp;
&nbsp;

### Install Configs on Hub & Branch FGTs

Each Hub and Branch has a Provisioning Template Group and the policy package applied to them. The configurations can now be pushed to the FGTs.
![Config Deployment 1](hub1installwiz1.png?width=950px)

&nbsp;
&nbsp; 

Let’s start by pushing the configurations to the Hubs. 

- Select HUB1
- Click Install
- Click Install Wizard
![Config Deployment 2](dmwithhubs2.png?width=950px)

&nbsp;

 - Install Wizard - Choose What to Install (1/4)
   - Select – Install Policy Package & Device Settings
   - For Policy Package select ***Hub***
   - Click ‘Next’
 ![Config Deployment 2](hub1installwiz2.png?width=750px)

&nbsp;

 - Install Wizard - Select Devices to Install (Hub) (2/4)
   - Hub1 and Hub2 should be selected
   - Click ‘Next’
![Config Deployment 3](hub1installwiz3.png?width=750px)

&nbsp;

 - Install Wizard - Validate Devices (Hub) (3/4)
   - Hub1 and Hub2 should be selected
   - Click ‘Install’
![Config Deployment 3](hub1installwiz3a.png?width=750px)

&nbsp;

 - Install Wizard - Installation Progress (Hub) (4/4)
   - Click ‘Install’
   - Once completed, you should see 'install and save finished status=OK'
   - Click 'Finish'
![Config Deployment 3](hub1installwiz4.png?width=750px)
![Config Deployment 3](hub1installwiz5.png?width=750px)

&nbsp;

 - **Repeat the same process to deploy the Branch FGTs except use the Branch Policy Package**:
	- Select – Install Policy Package & Device Settings
	- For Policy Package select ***Branch***
![Config Deployment 3](hub1installwiz6.png?width=750px)

The HUB and Branch FGTs are now synchronized and configured with both the Policy Package and the Provisioning Templates in their Template Group.  The green check mark next to them indicates they have both been installed.
![Config Deployment 17](hubssync.png?width=950px)

{{% notice note %}} 
You will often need to manually refresh your browser for the green check marks to appear next to the assigned Provisioning Templates.
{{% /notice %}}

&nbsp;
&nbsp;
&nbsp;
&nbsp;

### This concludes this section