---
title: "SD-WAN Link Impairment"
linkTitle: "Module 5: SD-WAN Link Impairment"
chapter: false
weight: 5
---

> Using FortiManager for SD-WAN Monitoring

## Adding Impairment to Branch1 Underlay1

1. Navigate back to your FNDN Demo page, click on the **'Demo Helper' HTTPS** button if not already open.

   ![DEMO HELPER](printscreen-05-1.png)

2. In the **Traffic Impairment** section, click on the **'Add Impairment to Branch1'** button.

   ![ADD IMPAIRMENT](printscreen-05-2.png)

   This will increase the latency on Branch1's Underlay1 link to **195ms**, which is over the **150ms** latency threshold configured in your HUB Health Check SLA.

---

## Observing the Impairment

While our impairment is in place, lets look at a few places we can see this.  
Let's look at the Main SD-WAN Monitor Map View again.

### Map View

- Notice **Branch1 is Orange**. You can then see the Underlay1 links that are failing their Performance SLA.

  ![MAP VIEW](printscreen-05-3.png)

### Table View

- Click Table View
- When hovering over the orange **'x'** next to an SD-WAN member, you can see the failed Performance SLA and the Latency (ms) value in red showing it is over the SLA threshold.

  ![TABLE VIEW](printscreen-05-4.png)

### History View

**Navigation:** Click Branch1 → History View → choose **last 10 Minutes** and show the auto refresh options.

![HISTORY VIEW](printscreen-05-5.png)

- Your screen should now show **red status** for all Underlay1 health checks.
- Both over-threshold and dead show red. **Yellow** will show when a time frame bar has both a healthy and unhealthy result in it.
- **Members:** The member with the check mark is the current preferred path. Hovering over the interface member shows details including health statistics — this can quickly provide information about why a path was or wasn't chosen.
- **RingCentral** SD-WAN Rule prefers UL2 (port4), so it is **not impacted** by a UL1 impairment.

---

## SD-WAN Steering Logging During Impairment

**Navigation:** FMG → Log View → Custom Views → SD-WAN Steering

![SD-WAN STEERING](printscreen-05-6.png)

- DIA traffic on Branch1 has moved from **'port3'** to **'port4'** as dictated by the "DIA" SD-WAN Rule.
- HUB1 traffic on Branch1 has moved from **HUB1-VPN1** to **HUB1-VPN1-2** as dictated by the SD-WAN rule "HUB."
- RingCentral did **NOT** move as it was preferring port4 in the rule and port4 was not affected.

---

## Removing the Impairment

1. Return to your SD-WAN Demo Helper Web Page.
2. Click on the **'Remove Impairment'** button.

   ![REMOVE IMPAIRMENT](printscreen-05-7.png)

   This will set the latency on Branch1's Underlay1 link back to **20ms**. Therefore, the latency will be below the 150ms latency threshold configured in your HUB Health Check SLA.

This should normalize your SD-WAN Health Check Status and SD-WAN Rule member selection.

![NORMALIzE](printscreen-05-8.png)
