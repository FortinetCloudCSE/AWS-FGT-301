---
title: Lab 2 - Template Groups Review and Edits
linkTitle: Lab 2 - Template Groups
weight: 40
---

## Template Groups Review and Edits

The SOT creates 3 Template Groups as well as the ones we created during the SOT and assigns the Device or Group to them. The screen shot below shows that the appropriate Provisioning Templates have been assigned correctly along with the Device or Device Group.

![Template Groups](tempgroupsnow.png?width=950px)

We have created additional templates needed to complete the SD-WAN configuration in the last several sections.  We created a new CLI Template, and System Templates.

Appropriate action must be taken for these templates to be applied to the correct devices or device groups.  The CLI Template was added to the CLI Template Group which is already applied to the Branch Template Group.  No further action is needed for CLI Templates.

The System Templates still need to be added to the Template Groups which we will complete below.

&nbsp;

### HUB1 Template Group
We need to add our newly create Static Route and System Templates to the HUB1 Template Group.

**Navigate to:**

***Device Manager>Provisioning Templates>Template Groups***

- Start by selecting the ‘2H_AP_BGPOL_HUB1’ Template Group that is applied to our HUB1 fgt and then click the ‘Edit’ button.
![Template Groups2](tempgrouphub1.png?width=950px)

&nbsp;

 - Click the + at the bottom of the Provisioning Templates section to add our new Templates
 ![Template Groups3](tempgrouphubedit1.png?width=850px)

&nbsp;

 - Expand the System Template section by clicking the ‘⌄’ on the right-hand side.
 - Click on the ‘HUB_SYSTEM’ template.
 - After clicking there should be a check mark next to it.
 ![Template Groups4](hub1groupaddsys.png?width=850px)

&nbsp;

Now that we have our Hub System template selected we can click the ‘OK’ button at the bottom of the page.
![Template Groups5](hub1tempgroupupdated.png?width=850px)

&nbsp;

Click ‘OK’ at the bottom of the page after validating that your two new templates appear in the list.
![Template Groups6](hub1tempgroupupdated2.png?width=850px)

## **Follow the same steps outlined above for HUB1 and add the correct System template to the HUB2 and Branches Template Groups.**

&nbsp;
&nbsp;
&nbsp;
&nbsp;

**Please make sure the Template Groups contain the proper Provisioning Templates and Assigned Device/Groups as shown below:**

![Template Groups8](templategroupsfinal.png?width=950px)

&nbsp;
{{% notice note %}}
You will notice that the only assigned group for the 2H_AP_BGPOL_BRANCH template group is the Branches and doesn't include the Branches_DIA. This is because Branches_DIA is a nested member of the Branches group.

![Branches Nested Group](branchesnestedgroup.png?width=650px)
{{% /notice %}}


&nbsp;

### Now that we have completed all the  Templates, take a minute to view the CLI Configuration Preview for all 3 Templates Groups.  YES, you can do it on Template Groups as well!!!

# END OF LAB 2
