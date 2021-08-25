Recently I had the pleasure of attending a 4-day training seminar on Trend Micro's Deep Security Product hosted internally by the Education team within Trend Micro. 
The 4 day course was well delivered and ended with myself passing the certification exam and becoming certified within the Deep Security Product. I thought I'd take the
time to formalise a few of my notes that I took during the course that for me were important from both an understanding and architectural design point of view.

We got to learn the basic deployment methods of Deep Security. My focus was understanding the components and the relationships between the Manager (known as DSM), Relay, Modules
and the agents. Below is a quick sketch I took in draw.io to help me put those puzzle peices together.
 
 <INSERT OVERALL ARCHITECTURE DESIGN/RELATIONSHIPS HERE>
 
 Deep Security offers 7 protection modules that can be enabled on any host compatible with running the Agent software which include:
  * **Anti Malware** - The title doesn't lie, this module protects hosts from viruses, trojans, spyware and ransomware.
  * **Web Reputation** - This module safeguards web browsing by taking a credibility score of the website taken from the Smart Protection Network that Trend Offers and can block      certain rated websites depending on their credibility score.
  * **Firewall** - The host based L4 firewall that examins the header information in the packet and can allow/deny traffic based upon a set of predefined firewall rules.
  * **Intrusion Prevention** - This module taps into the Firewall Module except instead of reading the header information of the packet it will look into the payload and block suspecious/dangerous traffic like ransomware or traffic that doesn't comply with standard protocol specifications.
  * **Integrity Monitoring** - Integrity Monitoring is all about analysing the current state of a directory and then reassessing and potentielly alerting administrators of any changes to that directory. Its worth noting this module can't determine a malicious change vs an approved change. Pointing this in critical directories that shouldn't be touched will assist with monitoring important files.
  * **Log Inspection** - This one collects and analyzes operating system logs and puts them into a common format to then analyse and determine suspecious behaviour / security events and more.
  * **Application Control** - This particular module will review software thats installed on the host and alert if the existing software inventory is changed (software is added/removed for example). This is also the module to use to whitelist approved software and lockdown unwanted software.
 
 
 (note that you can run angentless with VMWare plug-ins ect.).
 
 Notes around DSM / RELAY / AGENT / Policy / Modules
