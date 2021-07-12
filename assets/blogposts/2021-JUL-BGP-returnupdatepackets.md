After I wrote my last BGP blog I was doing some captures between Router's and noticed a very peculiar behaviour whenever a router that was duel-homed recieved a BGP UPDATE message from an upstream device.

Below is the Topology I am working with for reference:

![topology](/assets/img/topology-bgp-updatepackets.png)

What I found was that whenever R1 sent an "UPDATE" message R2 would respond with its own UPDATE message back to R1 advising it of the routing change made as a result of R1's original update. When I look at a packet capture between R1 and R2 I indeed two UPDATE messages..

![wireshark capture](/assets/img/bgp-updatemessage-wireshark.png)

Because BGP is designed to be loop free, when R1 recieves this update message it immediately recognises its own AS # within the route and drops the UPDATE as per the debug log below:

![debugmessages](/assets/img/bgp-r1debugmessages.png)

Puzzled - I tried to see if either R3 or R4 did the same however because these routers were single-homed they did not repeat any UPDATE messages. Like all puzzling questions I have, I took to the Cisco Community Page to try and understand why this behaviour would occur. I've seen other engineer's have this same issue but it was never successfully answered. Until now!

A Big thanks for Giuseppe Larosa who gave me a really good break-down on what I was seeing. I was unaware that Cisco Router's have a feature called "update-groups" that is automatically enabled by default. Its designed to optimise the preperation of routes to be sent to multiple neighbors. When you do not have any outbound filtering logic enabled (such as a route-map, prefix-list or a path-list in the outbound direction) then the router will put all assemble all the neighbors within the same update group. Because R1 and R2 are within the same update-group then R2 and R1 will be doing the same outbound update messages. So whenever R1 sends an UPDATE message either advertising a new prefix or withdrawing one then R2 will replicate this behaviour.

Ah, peace at last with this problem.
