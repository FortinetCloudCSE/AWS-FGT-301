---
title: Lab 3 - Create Address Objects and Address Groups
linkTitle: Lab 3 - Create Address Objects
weight: 10
---

## Create Address Objects and Address Groups

**Create Firewall Address Object**

Next, we are going to create an address object ‘LAN1’ and the object definition is going to be defined using the metadata variables.  The benefit here is that when you populate the per-device mapping for the metadata variable, the firewall address object will also be populated.  One less click when setting up another device.

**Navigate to:**

***FMG>Policy & Objects>Firewall Objects>Addresses***

 - Click "Create New"

 - Click "Address"

![Pic9](newfwaddress.png?width=750px)

 - Populate to match the below image and click "OK".
 - Name - LAN1
 - IP/Netmask - `$(lan1_net) $(lan1_mask)`

![Pic10](newfwaddresslan1.png?width=750px)



Create an Address Object Group and put our newly created LAN1 address object as a member.

![Pic12](createfwaddrgrp.png?width=750px)

Populate to match the below image and click "OK". ` LAN_Nets `

![Pic13](createfwaddrgrplannets.png?width=750px)

{{% notice note %}}
The LAN_Nets Address Object Group will be used in the Policy package even though LAN1 is the only member.  This way, if additional networks are added, all we would need to do is add the additional address to the address group.
{{% /notice %}}


Let's continue and make a few more addresses we will use when creating our Policy Packages.

 - Click ***Create New>Address*** from the current Addresses menu. ` BRANCH_LANS ` `10.1.0.0/16`
 - Popluate with the values shown below
 - Click OK
![Pic14](fwaddrbranchlans.png?width=750px)



Create an address object for ` DC_NET `.  This is called out separate from the previous steps as we will be using per device mappings.

Once named, expand the Per-Device Mapping section and click "Create New".

![Pic15](dcnetcreatemapping.png?width=750px)

Create 2 per device mappings (one for each HUB):

- Mapped Device – HUB1
- IP/Netmask – ` 172.33.1.0/24 `
- Click "OK"

![Pic16](dcnethub1map.png?width=750px)

- Mapped Device – HUB2
- IP/Netmask – ` 172.33.2.0/24 `
- Click "OK"

![Pic17](dcnethub2map.png?width=750px)

The end result should look like the below image.  Click "OK".

![Pic18](dcnetcomplete.png?width=750px)

{{% notice note %}}
You will recieve a prompts like the below stating conflict with [FABRIC_DEVICE] or IP/Subnet is set to 0.0.0.0/0. This is expected since you are configuring a dynamic object with 0.0.0.0/0 as the default IP. 

In a production environment you may want to use a default value in the event you are adding an additional hub later and that hub's value would default to 0.0.0.0 if not specificed as a dynamic mapping. ![Address Accept](anyaddressadd.png?width=650px)
{{% /notice %}}

**We will use two other fw address objects in our Policy Packages that SOT created for us:**

In Step 1 of our SOT creation, we specified an IP address range for our Loopback interface IPs.  

![Pic18](sotstep1addresses.png?width=750px)

SOT, being so intuitive, figured you would need to use those in a Policy Package, so it so kindly created these firewall address objects for you.

![Pic18](sotcreatedfwobjects.png?width=950px)



**Now that we have all our firewall objects ready, let's go create some Policies!**
