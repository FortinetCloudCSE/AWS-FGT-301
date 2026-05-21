---
title: Lab 1 - FMG Setup
linkTitle: Lab 1 - FMG Setup
weight: 5
---
<!-- MSG-COMMENT - NEED TO UPDATE THE SCREENSHOT FOR THE FNDN WHEN PASSWORDS ARE UPDATED -->

## Login to FMG via HTTPS:

![FMGLink](fndnfmg.png?width=250px)
&nbsp;  

{{% notice info %}} The password has already been entered and the device name has been changed to FMG for you.{{% /notice %}}

&nbsp;  
&nbsp;  

### Login to your FortiManager

* **Username**: admin
* **Password**: `Fortinet1234!`

![FMGLoginScreen](fmg-login.png?width=450px)  

<!-- MSG-COMMENT - No longer comes up
&nbsp;  
&nbsp;  

### The next step has been completed for you.  It is only shown below for reference. Click Begin to continue.
![FMGSetup](fmg-setup.png?width=550px)

&nbsp;  
&nbsp;  


### No need to setup a FMG backup for this lab.  Turn the toggle off for both "Automatic System Backup" and "ADOM Revision".  Click Next to continue.
![FMGSetup](disablefmgbackup.png?width=650px)

&nbsp;  
&nbsp;  

### Click Finish to continue.
![FMGSetup](fmg-setup-finish.png?width=650px)

&nbsp;  
&nbsp;  
 -->


### Some of the Admin Settings will need to be changed

**Navigate to:**

***System Settings>Settings***

- Change the Idle Timeout and Idle Timeout (GUI) to 1800 seconds or 30 minutes.
- Change the Theme to something you like.
- Click Apply.
![FMGAdminSettings](adminsettings.png?width=750px)

<!-- MSG-COMMENT - Reword about the remote gui on 7.4 -->

&nbsp;
{{% notice note %}}Some settings like "Idle Timeout" will not be applied to your current session.  You will have to log out and back in for these settings to take effect.  The "Idle Timeout" setting impacts the CLI and the "Idle Timeout (GUI)" setting impacts the web browser GUI.

"Access Remote GUI via Port" highlighted in blue in the above screen shot is a new 7.4 FortiManager feature that allows FortiManager to proxy a connection to a managed FOS devices' GUI.  The default port is 8082, you can change it here.  Keep in mind you can only connect to one FortiGate GUI at one time.
{{% /notice %}}

&nbsp;  
&nbsp;  


Please disable the change note requirement via CLI.  This is done to save time in the lab environment, but is not recommended in production.  We also want to ensure that the FGFM allow VM is enabled for later in the lab when importing FortiGate VMs.  The FMG CLI can be accessed by clicking the icon in the upper right shown below:

![FMGCLI](fmgcli1.png?width=950px)


&nbsp;  

Enter the following commands into the FMG CLI
```
 config system global 
    set object-revision-mandatory-note disable
    set fgfm-allow-vm enable
 end
```


Enable SD-WAN Monitor History while in the FMG CLI.  This will allow the FMG to save historical information about SDWAN devices.  Also, enable Traffic Shaping Monitor History while in the FMG CLI.  This will allow the FMG to save historical information about Traffic Shaping Statistics. SD-WAN Manager is also disabled since templates will be handled in the Provisoning Templates.

```
config system admin setting
    set sdwan-monitor-history enable 
    set traffic-shaping-history enable
    set show-sdwan-manager disable
end
```


![FMGCLI](fmgcli2.png?width=750px)




Setup FortiManager Management IP Address. The mgmt-addr setting in FortiManager (FMG) defines the IP address or FQDN (using mgmt-fqdn) that managed FortiGates use to connect.  

```
config system admin setting
 set mgmt-addr 198.18.250.1
end
```
![FMG MGMT Address](fmgcli2-mgmt-addr.png?width=750px)

{{% notice tip %}}
**This allows for automatic propigation during FMG IP Address or FQDN changes.** 

Multiple addresses (mgmt-addr) or multiple FQDNs (mgmt-fqdn) may also be used.
{{% /notice %}}


You may close the FMG CLI window.
Once again, you must log out and back into FMG for these settings to take effect.  Please do that now.

