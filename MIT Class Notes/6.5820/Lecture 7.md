There's a few cleanup notes we need about BGP before we move onto the main topic of discussion.
# BGP, continued
Last time, we ended on the import and export policies of BGP. To recap:
- BGP will export all learned routes to its customers, but it will only export routes to customers and self-origin to peers and providers.
- BGP chooses a route for a given destination based on the longest-prefix match, and will prefer customers over peers over providers in the case of a tie.

Now, an Autonomous System is composed of many servers, each with various addresses and varying traffic. If we used all of them to route BGP, it would become a massive scaling problem. Instead, there are specific routers, known as "Border Routers" that exist on the metaphorical "edge" of an AS. These are what handle communications with other ASes, and this previously used to require a physical IP link to establish communications. It should be obvious that BGP runs over TCP, as you need the reliability to transmit large routes without error.

Here's what a Routing Table might look like:
![[Pasted image 20250929152118.png]]
An interesting note is the AS Path. This shows, from left to right, the order of ASes which will be traversed to reach the destination (the right-most AS). This is mainly to limit the possibility of a cycle. Anyways, the router will chose one of these based on its own local preferences. Maybe the top entry is a customer provided route, so it will forward a packet through there.

# Path-Vector Routing
The structure of routes looks quite similar to those in path-vector routing. This is an extension of distance-vector routing, but works better for flexible routing policies. 

It has a very simple idea: you advertise the entire path to a destination. 
- For each destination d, you would send the distance metric to d, and the entire path to reach it.
Why would we do it this way? Consider classical shortest path algorithms, such as Dijkstra's Algorithm. There's a few reasons why they wouldn't work:
- They scale very poorly with the scale of the Internet.
- There's no way to "keep routes secret" (which here is done by specific export policies)
We can use path-vector routing to achieve both of those qualities, and each AS gets to apply their own local policy.

# Internal and External BGP
You might be wondering how routes are exchanged within an AS. We know that between them, border routers handle most of that communication.