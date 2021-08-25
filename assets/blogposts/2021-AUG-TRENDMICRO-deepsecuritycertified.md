Recently I had the pleasure of attending a 4-day training seminar on Trend Micro's Deep Security Product hosted internally by the Education team within Trend Micro. 
The 4 day course was well delivered and ended with myself passing the certification exam and becoming certified within the Deep Security Product. I thought I'd take the
time to formalise a few of my notes that I took during the course that for me were important from both an understanding and architectural design point of view.

We got to learn the basic deployment methods of Deep Security. My focus was understanding the components and the relationships between the Manager (known as DSM), Relay, Modules
and the agents. Below is a diagram from the course material that outlines the big picture of Deep Security.
 
![dsdesign](/assets/img/deepsecurityarchitecture.png)
 
 Deep Security offers 7 protection modules that can be enabled on any host compatible with running the Agent software which include:
  * **Anti Malware** - The title doesn't lie, this module protects hosts from viruses, trojans, spyware and ransomware.
  * **Web Reputation** - This module safeguards web browsing by taking a credibility score of the website taken from the Smart Protection Network that Trend Offers and can block      certain rated websites depending on their credibility score.
  * **Firewall** - The host based L4 firewall that examins the header information in the packet and can allow/deny traffic based upon a set of predefined firewall rules.
  * **Intrusion Prevention** - This module taps into the network traffic just like the Firewall Module except instead of reading the header information of the packet it will look into the payload and block suspecious/dangerous traffic like ransomware or traffic that doesn't comply with standard protocol specifications.
  * **Integrity Monitoring** - Integrity Monitoring is all about analysing the current state of a directory and then reassessing and potentielly alerting administrators of any changes to that directory. Its worth noting this module can't determine a malicious change vs an approved change. Pointing this in critical directories that shouldn't be touched will assist with monitoring important files.
  * **Log Inspection** - This one collects and analyzes operating system logs and puts them into a common format to then analyse and determine suspecious behaviour / security events and more.
  * **Application Control** - This particular module will review software thats installed on the host and alert if the existing software inventory is changed (software is added/removed for example). This is also the module to use to whitelist approved software and lockdown unwanted software.

Within a standard Deep Security Deployment there are 4 main things you'll need:

1. Deep Security Manager (DSM) - The Manager is your main tool as the Administrator. It is where you can make individual host changes or global Policy changes on the fly. Additionally, its also where you'll find reporting and centralised logging. There are many more features of DSM but these are the main to get you up and running. Currently in Deep Security 20 you can install the Manager on Windows Server 2012/2016/2019 or on RHEL 7/8. 
2. Database - This is where all the data that the Manager displays (and more!) is stored. Latency is important as its recommended that a two millisecond latency or lower between the Manager and the database is setup. Compatible databases include Microsoft SQL Server 2012/2014/2016/2017/2019 along with Oracle, PostgreSQL and Cloud based databases for SAAS deployments.
3. Deep Security Relay - A Relay is essentially a host (with the Deep Security Agent installed) that has access to the Trend Cloud in order to download and distribute security updates across your agents. The Manager is where you can advance a standard Trend Host as a Relay.
4. Deep Security Agent - This is the software itself thats installed on a compatible host. This is the mechanism on the host that enforces the pre-defined security posture and communicates back to the DSM to obtain policy updates ect (This is known as the heartbeat).

As I continue to go through my notes this blog post will be updated.
