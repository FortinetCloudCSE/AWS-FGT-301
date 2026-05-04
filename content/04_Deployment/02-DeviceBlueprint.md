---
title: Lab 4 - Create a Device Blueprint
linkTitle: Lab 4 - Device Blueprint
weight: 10
---

## Create a Device Blueprint
You are going to create two Device Blueprints that will be used in the .CSV import for Model Device creation.  A seperate Blueprint is needed per Device Group.  

**Navigate to:**

***Device Manager>Managed FortiGate***
 - Click the down arrow next to the ‘Add Device’ button
 - Click Device Blueprint
![Blueprint1](dvmblueprint1.png?width=650px)

&nbsp;

- Click 'Create New'
![Blueprint2](dvmblueprint1new.png?width=750px)

&nbsp;

Fill out the Device Blueprint with the following attributes:
 - Name – ` Branches-VM64-KVM `
 - Device Model – FortiGate-VM64-KVM
 - Port Provisioning - 5
 - Leave Automatically Link to Real Device On
 - Enforce Firmware Version – 7.6.4-b3596
 - Add to Device Group – Branches
 - Assign Policy Package - Branch
 - Click OK
![Blueprint3](branchesdevblue.png?width=850px)

{{% notice note %}} 
You do NOT need to assign the Provisioning Template Group in the Device Blueprint.  By selecting the ‘Branches’ as Device Group, it will get applied to the model devices as ‘Branches’ device group is an Installation Target.
{{% /notice %}}

&nbsp;

Result:
![Blueprint4](branchesdevblueresult.png?width=950px)
&nbsp;


{{% notice note %}} 
Device Blueprints are created for a specific Device Model.  If your customer is using multiple FGT models, you must have a Device Blueprint for each FGT model.  Also, a different blueprint must be created for same FortiGate Model yet different Device Group like we do in this lab.
{{% /notice %}}

&nbsp;

Let's make a 2nd Device Blueprint for our Branches_DIA Device Group. For this we will utilize the 'Clone' feature and edit items needed for the Branch_DIA.

 - Right Click on 'Branches-VM64-KVM'
   - Select Clone

![Blueprint Clone](branchesdiadevblueclone.png?width=850px)

 - Update the Name – ` Branches_DIA-VM64-KVM `
 - Change the Device Group – Branches_DIA
 - Click OK
![Blueprint3](branchesdiadevblue.png?width=850px)

You should now have two Device Blueprints that look like this:

![Blueprint3](bothdevblues.png?width=850px)

**Now that we have our Device Blueprints, now let's go create our CSV file that will reference them.**