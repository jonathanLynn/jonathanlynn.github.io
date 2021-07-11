[Back to Home](./)

For a while I've wanted to sit down and dive into the BGP RFC. This post is a little deep dive of my own into the 
best path selection critiera.

RFC 4271 specifies that the Decision process takes place between 3 distinct phases which are initiated one after another in succession.
 
The 3 phases are:
 
Phase 1 - Calculating the degree of preference for each new route received from an active peer.
Phase 2 - Triggered by the completion of Phase 1, Phase 2 is responsible for choosing the best path out of all available routes and installs the best path into the Loc-RIB.
Phase 3 - Triggered by Phase 2 completing, Phase 3 aims to then disseminate the new route between to its active peers.
 
*It's worth noting that these phases happen within every router/node that participates in the Boarder Gateway Process.
 
So how are these phases triggered?
 
Phase 1 is triggered whenever a router participating in BGP receives an "UPDATE" message from an active peer. An "UPDATE" message is how two peer's communicate things such as:
-New Routes
-Replacement Routes
-Withdrawn Routes
 
Once an "UPDATE" message is received, the router's job is to then calculate the degree of preference. Only when a new/replacement route is received, the router will follow these specific rules to identify which route to preference:
 
1. Which route within the "Adj-RIB-In" table has been advertised with the higher Local Preference? (commonly abbreviated to Local_Pref)
2. <SHIT FIX THIS>

Phase 2 is all about choosing the best route within the "ADj-RIB-In" Table. Once the Router is in Phase 2, the following rules apply
when the router is in the process of choosing the best route:

1. The router reviews all available routes and remove any entries that have a NEXT_HOP value which is unresolable. (This is why
in certain scenario's you'll need to use the "next-hop self" command).
2. The router will then scan the "AS_PATH" attribute in all available routes and removes all entries where the router's own AS #
appears. This is the BGP loop prevention mechanism taking place.

Whats left now is a set of routes from the original "Adj-RIB-In" table that have reachable next hops and are loop free. The
router will now begin selecting more feasable routes by:
-Selecting the highest degree of preference from any route that has the same destination
-Is the only route to that destination
-Is selected as a result of the Phase 2 tie breaking rules.

The Phase 2 tie breaking rules are:

1. Drop the route that are not tied for having the smallest number of AS numbers present in the "AS_PATH" attribute.
2. Drop the route that are not tied for having the lowest Origin number in the "Origin" attribute.
3. Have a less preferred "MULTI_EXIT_DISC" attributes.
4. Prefer the route learnt via eBGP over iBGP if applicable.
5. Prefer the route with the lowest IGP metric to the BGP next hop.
6. Prefer the route that was advertised by the BGP peer with the lowest BGP identifier value.
7. Prefer the route that was advertised by the BGP Peer with the lowest neighbor address.

Once Phase 2 is complete, we move onto Phase 3 which is all about Route Dissemination.

Phase 3 kicks off once the router has completed Phase 2 and now has a set of the most preferred,
feasible routes. These set of routes are injected into the Loc-RIB and are then processed into the Adj-RIBs-Out where
the router will then send an "UPDATE" message to its other peers where they will perform their own "Best Path Decision" 
process themselves.

[Back to Home](./)
