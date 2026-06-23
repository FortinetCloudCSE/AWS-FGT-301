---
title: "Application Performance Monitoring (APM)"
linkTitle: "Section 8: Application Performance Monitoring (APM)"
chapter: false
weight: 8
---

## Enabling Application Performance Monitor

***Navigation: FMG → Policy & Objects → Policy Packages → Branch → Firewall Policy → Rule 2 → Advanced Options***

This is enabled within a firewall policy rule. Both of these settings must be enabled:

- `app-monitor`
- `passive-wan-health-measurement`

![ADVANCED OPTIONS](printscreen-11-1.png)

> [!NOTE]
> This will disable offloading on that policy automatically.

---

## Application Performance Logs

***Navigation: FMG → Log View → Custom Views → Application Performance***

These are the logs that supply the data to create the graphs. Filter on: `logid="0113022941"`

![PERFORMACE MONITOR](printscreen-11-2.png)

---

## Application Performance Overview

***Navigation: FMG → FortiView → SD-WAN → Application → Application Performance Overview***

![PERFORMANCE OVERVIEW](printscreen-11-5.png)

View the application performance data collected.

### Drill Down

Drill down into an application for more detailed information.

![DRILL DOWN](printscreen-11-6.png)
