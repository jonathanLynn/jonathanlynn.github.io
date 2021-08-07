<a href="https://jonathanlynn.github.io/">Back to Home</a>

After I wrote my last BGP blog I was doing some captures between Router's and noticed a very peculiar behaviour whenever a router that was duel-homed recieved a BGP UPDATE message from an upstream device.

Below is the Topology I am working with for reference:

![topology](/assets/img/topology-bgp-updatepackets.png)

What I found was that whenever R1 sent an "UPDATE" message R2 would respond with its own UPDATE message back to R1 advising it of the routing change made as a result of R1's original update. When I look at a packet capture between R1 and R2 I indeed two UPDATE messages..

![wireshark capture](/assets/img/bgp-updatemessage-wireshark.png)

Because BGP is designed to be loop free, when R1 recieves this update message it immediately recognises its own AS # within the route and drops the UPDATE as per the debug log below:

![debugmessages](/assets/img/bgp-r1debugmessages.png)

Like with all puzzling questions I have with Cisco deployments, I took to the Cisco Community Page to try and understand why this behaviour would occur.

A Big thanks for Giuseppe Larosa who gave me a really good break-down on what I was seeing and why. I was unaware that Cisco Router's have a feature called "update-groups" that is automatically enabled by default. Its designed to optimise the preperation of routes to be sent to multiple neighbors. When a router does not have any outbound filtering logic enabled (such as a route-map, prefix-list or a path-list in the outbound direction) then the router will assemble all the neighbors within the same update group. Because R1 and R2 are within the same update-group then R2 and R1 will be doing the same outbound update messages. So whenever R1 sends an UPDATE message either advertising a new prefix or withdrawing one then R2 will replicate this behaviour.

Ofcourse, I wouldn't have seen this problem in any of the production envirorments I've worked in because best practise dictates that you filter incoming and outgoing prefix's thus hiding the problem from me until I spun up a very simple lab topology.

Ah, peace at last with this problem.

<a href="https://jonathanlynn.github.io/">Back to Home</a>
