---
layout: post
slug: Avoiding Downtime? Use ISSU for Cisco NXOS
category: Data Center
---

I don't know about you but downtime is an absolute pain in my A$$. I should clarify my previous statement is more so to do with Data Centres than say a small retail store. The Data Centre (at least in my experience) is always where the mission critical infrastructure sits and it can be really difficult (mere impossible) to take downtime. It's almost the perfect story for advasaries right? Org's are slower in this space to patch vulnerabilities and attackers get more time to infiltrate.

![CAB](https://raw.githubusercontent.com/jonathanlynn/jonathanlynn.github.io/master/images/rambo-cab.png){:.ioda}

Patching and Upgrading Firmware in your Network is no longer optional, its a non-negotiable. You have to do it at a minimum to keep your customers and colleagues safe. So going back to my challange. On one hand I need to keep updating my systems whilst on the other maintaining uptime. We're all familiar with disruptive upgrades for the majority of platforms however there is an upgrade mode called "In Service Software Upgrade" or "ISSU" for short. The goal of ISSU is to upgrade the firmware whilst maintaining both control-plane and data-plane functionality. In the case of my Cisco Nexus Switches its so keep the traffic flowing while the upgrade happens.

An ISSU upgrade is often faster because you don't need to reboot the chassis. It works on a containerised level where your current firmware runs on ContainerA and ISSU will install the new firmware into ContainerB then promote ContainerB to be ContainerA all whilst your routing and switching parameters remain the same.

When I tried this the other week it worked like a charm. my vPC/OSPF/NVE Peers all stayed UP and the downstream devices saw no impact. So without furtherado here is the process I followed.

1. Validate the upgrade path from your current and target firmware are ISSU supported*.

![ISSU](https://raw.githubusercontent.com/jonathanlynn/jonathanlynn.github.io/master/images/Cisco-Nexus-ISSU-Website.png){:.ioda}

2. Copy the firmware over to your Nexus switch and run the below command that should give you the OK to proceed with ISSU for a list of things that need rectifying beforehand to use ISSU.

#show incompatibility nxos bootflash:filename

3. Using the "#Install" command from the upgrade guide* coupled with the "non-disruptive" verb and you'll be able to kick off the upgrade.

#install all nxos bootflash:nxos.x.x.x.bin non-disruptive

When you run an ISSU upgrade over SSH then your terminal will cut out (don't panic!) however in a LXC upgrade (Enhanced ISSU) then SSH will remain.

Handy Links

*Upgrade Path Matrix
https://www.cisco.com/c/dam/en/us/td/docs/dcn/tools/nexus-9k3k-issu-matrix/index.html

*Nexus Upgrade Guide
https://www.cisco.com/c/en/us/td/docs/dcn/nx-os/nexus9000/104x/upgrade/cisco-nexus-9000-series-nx-os-software-upgrade-and-downgrade-guide-104x/m-upgrading-or-downgrading-the-cisco-nexus-9000-series-nx-os-software.html#Cisco_Concept.dita_c52831f8-8da8-4219-a2b8-9c95fd774ecd