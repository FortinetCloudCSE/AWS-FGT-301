---
title: Lab 1 - Add FAZ to FMG
linkTitle: Lab 1 - Add FAZ to FMG
weight: 10
---



{{% notice info %}} The primary objective of this lab is to explore how to create a single pane of glass with FMG and FAZ.{{% /notice %}}


&nbsp;  

## Adding FAZ to your FMG:

FortiAnalyzer will be connected to FortiManager in this section.

**Navigate to:**

***FMG>Device Manager>Device & Groups***

 - Click the down arrow next to the 'Add Device' button.
 - Select 'Add FortiAnalyzer'

![Add FAZ](addfaz1.png?width=850px)



&nbsp; 

- Enter the FAZ lab management IP - ` 192.168.0.5 `
- Toggle 'Use legacy device login' to on
- Enter username ` admin `
- Enter password ` Fortinet1234! `
- Click Next

![Discover FAZ](addfaz13.png?width=950px)


A ***"Probe failed: network"*** should pop up.
![FAZ Fail](fazprobefail.png?width=950px)


&nbsp; 

**The discovery probe failed because FAZ does not allow FMG to access it by default.  This can be resolved by logging into FAZ and enabling FortiManager access on the interface the discovery is pointed to.**

&nbsp;  
&nbsp; 
&nbsp;  
&nbsp; 

<!-- MSG-COMMENT - NEED TO UPDATED WITH FNDN PASSWORD UPDATE -->

## Connect to FAZ via HTTPS
![FAZ Link](fazconnect.png?width=550px)

&nbsp; 

- Enter 'admin' as username
- Enter ` Fortinet1234! ` as password
![FAZ Login](fazloginprompt.png?width=650px)
&nbsp; 
&nbsp; 

<!-- MSG-COMMENT - No longer comes up will re-verify

### The next step has been completed for you.  It is only shown below for reference. Click Begin to continue.
![FAZ System Settings](fazsetup1.png?width=650px)
&nbsp; 
&nbsp; 

### No need to setup a FAZ backup for this lab.  Turn the toggle off for "Automatic System Backup".  Click Next to continue.
![FMGSetup](fazsetup2.png?width=650px)

&nbsp; 
&nbsp; 

### Click Finish to continue.
![FMGSetup](fazsetupfinish.png?width=650px)
-->
**Navigate to:**

***FAZ>System Settings>Network***
&nbsp; 
&nbsp; 

- Within Network
- Select 'port1'
- Click Edit button
![FAZ allow FMG](fazintedit.png?width=950px)

&nbsp; 
&nbsp; 

- Select FortiManager in the Administrative Access section
- Click OK
![FAZ allow FMG2](fazfmgtoggle.png?width=950px)

&nbsp; 
&nbsp; 

Select OK on the Warning: Updating Interface Configuration below:
![fazcli](fazinterfacewarn.png?width=950px)

&nbsp; 
&nbsp; 
<!-- MSG-Comment - No longer needed
Let's try this again, but via the CLI.  The FAZ CLI can be accessed by clicking the icon shown below:
![fazcli](fazcli.png?width=950px)

&nbsp;  

Enter the following commands into the FAZ CLI
```
config system interface
  edit port1
    set allowaccess http https ping ssh fgfm
  next
end
```
Let's verify the access on Port1 now via CLI
```
show system interface port1
```
You can now close your CLI window.  Be aware, the FGFM/FortiManager access on Port1 will still not show up in the FAZ GUI.
&nbsp; 
&nbsp; 
-->

## Return to your FMG and add FAZ again
**Navigate to:**

***FMG>Device Manager>Device & Groups***

{{% notice note %}} Due to the port forwards in FNDN, you will be logged out of the FMG once logged into the FAZ. This is expected, you must log back into FMG to proceed. {{% /notice %}}

 - Click the down arrow next to the 'Add Device' button.
 - Select 'Add FortiAnalyzer'

![Add FAZ](addfaz1.png?width=950px)



&nbsp; 

- Enter the FAZ lab management IP - ` 192.168.0.5 `
- Toggle 'Use legacy device login' to on
- Enter username ` admin `
- Enter password ` Fortinet1234! `
- Click Next

![Discover FAZ](addfaz13.png?width=950px)

&nbsp; 

 - Verify the information in step 2/3
 - Click Next
![Add FAZ 2 of 3](addfazstep2.png?width=950px)

&nbsp; 

 - Click 'Synchronize ADOM and Devices' button on step 3/3
![Add FAZ 3 of 3](fazsync.png?width=950px)

&nbsp; 

 - Click 'Finish' to complete the adding of FAZ to FMG.
![Add FAZ 4 of 3](addfazfinish.png?width=950px)

&nbsp; 

{{% notice tip %}}
**You might need to refresh your browser for these new FAZ panes to show up.**
{{% /notice %}}

Your FMG Device Manager should now look like this:
![FAZ Tiles](fmgwithfaz.png?width=750px)

You should now see that additional FAZ sections in FMG:
![FAZ Tiles](fazaddsections.png?width=250px)

