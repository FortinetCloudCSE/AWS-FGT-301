---
title: Lab 4 - Install Configs on Hub FGTs
linkTitle: Lab 4 - Install Configs on Hubs
weight: 5
---

## Install Configs on Hub FGTs

HUB1 and HUB2 were joined to the FMG in Lab 1.  Each Hub has a Provisioning Template Group and the HUB policy package applied to them. The configurations can now be pushed to the Hub FGTs.
![Config Deployment 1](dmwithhubs2.png?width=950px)

&nbsp;  
&nbsp; 

Let’s start by pushing the configurations to HUB1.  First, we need to do an install of the Device Settings.  This will create the VPN interfaces and loopback interfaces used in the Policy Package.

- Select HUB1
- Click Install
- Click Install Wizard
![Config Deployment 2](hub1installwiz1.png?width=950px)

&nbsp;

 - Select – Install Device Settings (only)
 - Click ‘Next’
 ![Config Deployment 2](hub1installwiz2.png?width=750px)

&nbsp;

 - Select HUB1
 - Click ‘Next’
![Config Deployment 3](hub1installwiz3.png?width=750px)

&nbsp;


{{% notice warning %}} **Did your Installation Fail?  Let’s review the log and find out why….** {{% /notice %}}
![Config Deployment 4](hub1installwiz4.png?width=750px)

![Config Deployment 5](failedinstallpreview.png?width=950px)

You used the metadata variable ‘ul1_gateway_IP’ in the HUB1_STATIC_ROUTES provisioning template, but forgot to define those values for HUB1 and HUB2.  Let's go do that now.

There are multiple ways to define the per-device mappings for metadata variables.  You can go to ***Policy & Objects>Advanced>Metadata Variables*** then edit the variable and add the Per-Device Mappings there.  For this lab, you are going to define variable values within each managed device.

 - Close out Error Message by clicking ‘Cancel’ button
 - Click ‘Cancel’ to cancel the Install process.
 - Go back to ***Device Manager>Managed FortiGate***
 - Right Click on HUB1
 - Select Edit Variable Mapping
 ![Config Deployment 6](hub1metaedit.png?width=650px)

&nbsp;

 - Find the $(ul1_gateway_IP) Variable Name
 - Enter – ` 198.18.101.100 `
 - Click the checkmark **(IMPORTANT)**
 - Click ‘OK’
 ![Config Deployment 7](hub1metadedit2.png?width=650px)

&nbsp;

While we are here let’s define the variable mapping for HUB2 as well.

 - Right Click on HUB2
 - Select Edit Variable Mapping
 - Find the $(ul1_gateway_IP) Variable Name
 - Enter – ` 198.18.102.100 `
 - Click the checkmark **(IMPORTANT)**
 - Click ‘OK’
 ![Config Deployment 8](hub2metadedit2.png?width=650px)
&nbsp;

{{% notice tip %}}
If you do not click the check mark next to the variable mapping the value is **NOT** saved.  This is easily missed so ensure you do this.
{{% /notice %}}

Now let’s try that Install Wizard and see what the Preview shows this time!

Scroll through the output and review all the config that will to be installed from all the Templates applied in our Template Group.  
Then click Close.
![Config Deployment 8](hub1installprev.png?width=750px)

 - Click Install
 ![Config Deployment 8](clickinstall.png?width=750px)
 - Click Finish
![Config Deployment 9](clickfinish.png?width=750px)

Result:
![Config Deployment 10](hub1installresult.png?width=950px)

&nbsp;

Perform the exact same steps used to install HUB1 to install the config on HUB2.  

Your Device Manager should now look like this with green check marks next to the assigned Provisioning Templates.
![Config Deployment 11](hubsinstalled.png?width=950px)

&nbsp;

The Hub policy package can be installed now that the HUBs have the SOT generated configuration. 
 - Click Install
 - Select ‘Install Wizard’
 ![Config Deployment 12](installwizard.png?width=950px)


&nbsp;

 - Check the radio button next to ‘Install Policy Package & Device Settings’
 - Select the ‘Hub’ Policy Package from the drop down list
 - Leave ADOM Revison selected
 - Click Next
![Config Deployment 13](hubspolinstall.png?width=750px)

&nbsp;

 - Ensure both HUB1 and HUB2 are selected.
 - Click Next
 ![Config Deployment 14](hubspolinstall2.png?width=650px)
 - Click ‘Install Preview’ button
 ![Config Deployment 14](installwizard2.png?width=650px)

&nbsp;

Here you can review the configurations that will be pushed to both HUB1 and HUB2. Notice that the ‘DC_NET’ subnet definition is different based on the per-device mappings we entered.  
![Config Deployment 15](hub1ppinstallprev1.png?width=750px)
 - Finish reviewing the Install Preview
 - Click Close on the Install Preview window
 - Click Install
 - Click Finish

Result:
![Config Deployment 16](hubsppinstallfinal.png?width=750px)

&nbsp;

The HUBs are now synchronized and configured with both the Policy Package and the Provisioning Templates in their Template Group.  The green check mark next to both indicates they have both been installed.
![Config Deployment 17](hubssync.png?width=950px)

{{% notice note %}} 
You will often need to manually refresh your browser for the green check marks to appear next to the assigned Provisioning Templates.
{{% /notice %}}
