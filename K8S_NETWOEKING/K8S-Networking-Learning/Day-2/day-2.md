Day 2 — Linux routing: How does a packet find its destination
==============================================================


* Today, we’ll learn how Linux decides where to send a packet. We’ll build on what you already know about Pod IPs, network namespaces, and AWS VPC.

* Imagine Pod A wants to send a request to Pod B.

             Pod A
      Destination: 10.0.2.35
      
         Linux routing table
      Which route should I use for this destination?
      
      Next hop / outgoing interface
      Linux follows the selected route.

What is a routing table?

    A routing table is a list of rules that helps Linux decide where packets should go.

For example, imagine a simplified routing table:

    Destination	          Meaning
    10.0.1.0/24	          Send traffic for this network through a local interface.
    10.0.2.0/24	          Use the configured route for that network.
    0.0.0.0/0	            Default route, used when no more specific route matches.

    These are illustrative routes, not actual routes from your EC2 instance.


Important: Linux generally selects the most specific matching route. The default route is the fallback when no more specific route matches.

2. Practical — 10 minutes
   
Let's inspect routing without changing anything.

* Command 1 — View the routing table: ip route ------> Shows the routes Linux currently knows about.

      default via 10.0.1.1 dev eth0
      10.0.1.0/24 dev eth0 proto kernel scope link

      Here's how to understand it:

      default via 10.0.1.1 dev eth0
      - default — used when no more specific route matches.
      - via 10.0.1.1 — send traffic through this next-hop gateway.
      - dev eth0 — use the eth0 network interface.

        Destination: Where the parcel needs to go.
        Gateway: The next delivery point.
        Interface: The road your parcel leaves through.

      10.0.1.0/24 dev eth0
      This route covers the 10.0.1.x subnet. Linux can send traffic for that directly connected network through eth0, without using the default gateway.



* Command 2 — Check the route to a destination: ip route get <IP>

       This displays rules that determine which routing table Linux consults.
       Most basic setups use the main routing table, but advanced networking can use multiple tables and rules.

Shows the route Linux would use for this destination, based on the current routing rules and routing context. The example IP may not exist in your network.

* Command 3 — View routing policy rules: ip rule ---> Shows rules that can influence which routing table Linux consults.

3. Troubleshooting scenario — 10 minutes
   
Imagine an application in Pod A sends a request to Pod B, but the request fails.

You run:

    ip route get 10.0.2.35


* reports that it cannot find a usable route to the destination in the current routing context.
* That gives you a useful clue: investigate the routing configuration first. Then verify the route and connectivity from the relevant Pod or node.
* A missing route in one context doesn't necessarily mean the route is missing everywhere.
* Also remember: a valid route doesn't prove the packet will arrive. Security rules, network configuration, or the destination application may still cause a failure.


