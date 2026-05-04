---
title: Lab 4 - Create a CSV
linkTitle: Lab 4 - Create a CSV
weight: 15
---

## Create a CSV for Model Device Creation

We will make a CSV to be used to create the Branch Model Devices.  This file will provide the Branch FGT serial numbers, the device blueprint name, device name, number of VM interfaces, and define all Metadata Variables.  With this method multiple model devices can be created at one time.

This drastically reduces the number of clicks your customer must do to create model devices and defines the metadata variable values for each device.

Please download the CSV Import File in the left hand pane in the **HELPFUL RESOURCES** section:
Download and save the file locally.  We will edit it to match your demo environment.

Let’s get started. Open XpertsCSV.csv in Microsoft Excel as we need to make some edits.

![CSV1](csvimport.png?width=950px)

The information has been filled in for you.  Many fields are defined including each FGT’s metadata variable mappings.

{{% notice warning %}}
The FGT Serial Numbers listed in the CSV need to be replaced by the SN#s of the FGTS in your specific lab!
{{% /notice %}}



{{% notice note %}}
Any additional columns in your .CSV that do not match an existing Metadata Variable will be IGNORED.
{{% /notice %}}





 - Log into your ‘Branch1’ and ‘Branch2’ FGTs via HTTPS and get their Serial Numbers.  

 ![Branch FNDN](branchfndn.png?width=550px)

 {{% notice warning %}}
 DO NOT CREATE A PASSWORD!!!!!  Click 'Later' when prompted to Change Your Password. This will break FMG's ability to onboard and push the config.
 
 ![Ignore1](donotchangepassword.png?width=750px)
 {{% /notice %}}
 - Enter your serial numbers in the correct column.
 - Ensure the names of the Device Blueprints you created match what is in the .CSV
 - Save the updated as `Xperts2025CSV.csv` file locally


Now that we have an updated ‘Xperts2025CSV.csv’ with your lab FGT SNs, we can proceed to create Model Device using the CSV.

**Navigate to:**

***Device Manager -> Managed FortiGate***
 - Click ‘Add Device’ button
 - Click ‘Import Model Devices from CSV File’
![CSV3](adddeviceimport.png?width=950px)

&nbsp;

 - Click on the ‘Add Files’ link in the Upload Section
 - Browse to your ‘Xperts2025CSV.csv’ file location
 - Select the file and click the ‘Open’ button
![CSV4](adddeviceimport2.png?width=750px)

&nbsp;

 - Click Next
![CSV5](adddeviceimport3.png?width=750px) 

&nbsp;

The model devices will be imported.
 - Click ‘Finish’
![CSV6](adddeviceimport4.png?width=750px)

&nbsp;
<!-- MSG-COMMENT - HAD TO REFRESH FOR THE POLICY ASSIGNMENT TO WORK -->
Branch1 and Branch2 model devices have now been created.  They have been assiged to the correct Device Group.  They both have Auto-link Enabled (as we set in Blueprint).  Also, the Policy Package and Provisioning Templates have also been assigned.
![CSV7](branchesaddedviacsv.png?width=950px) 

{{% notice note %}}
The 'Auto-link Status' column is added after CSV Import.  A manual refresh might be needed for this to be visible.
{{% /notice %}}

&nbsp;

Take a moment to verify the correct metadata variable values have been configured:
 - Right Click on your Branch1 model device
 - Click on Edit Variable Mapping
![CSV8](br1variablecheck.png?width=950px) 

![CSV10](br1variablecheckgood2.png?width=750px) 


**Our Model Devices are configured and ready.  Let's get the Branch FortiGates to reach out to FMG so it can push the configs down.  Proceed to the next section.**

