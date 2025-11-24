#TODO: I don't know how to write an introduction to this


# Mechanics of Chord
## The Key/Value Abstraction
The keys of a hash table are a flat m-bit identifier, typically of 128 or 256 bits. The values to store can be anything, depending on the application. If the values are small, these can be stored together. It isn't as trivial if the values are large, such as a movie. In this case, we assume that the values are relatively small.

## Mapping
Suppose we have nodes 0,...,n-1 and a set of keys K to store.
The most obvious method of storing keys is to map k to the node corresponding to k mod n. There are a few issues with this:
- There is a need to track node identifiers
- It is an inconsistent scheme. Given L keys and n nodes, there are about $\frac{L}{n}$ keys to each node, on average. If the number of nodes $n$ changes, then at minimum, $\frac{L}{n+1}$ keys need to move.
Consistent hashing is a way to deal with these issues. It works in the following way:
- Nodes are named with IDs from the same namespace as keys.
- A key k is assigned to its **successor**: the first node with an ID $\geq k$. 
- When a node joins, it gets its set of keys from its successor.
As a concrete example, suppose we had the set of nodes with IDs 3, 16, 43, 56, 89. A new node joins the network with ID 28, and its successor is node 43. It then stores keys 17-27 (which all have the successor as 28).
This scheme is much better, and is what Chord uses. It does introduce a question: how do you assign IDs uniquely in a distributed setting? Typically, the input to the hash function uses something that is likely unique to each node. Examples are IP addresses, port numbers, the internal clock, etc. Note that this is only valid if the hash function has not been broken (meaning there is a way to force a collision between two distinct inputs). In practice, there are much larger problems if the hash function is broken, so this is not a huge concern.


## Finding Keys
How do we find the location of a given key in this distributed setting? The naïve method is a linked list, such as the following:
#drawing graph
This is not optimal, as key lookups will take O(n) time.

To get around this, Chord utilizes a Fingers list. A node with ID n contains the following entries in its list:
- finger\[0] = successor(n)
- finger\[i+1] = successor($n+2^i$ mod k( #clarify  i don't remember what the mod term is)), where i = O(m), the number of bits for the keys
The idea is that each finger points to an element with distances scaling in O($2^{-i}$). Each one represents an element that is $\frac{1}{2}, \frac{1}{4}, \frac{1}{8},\dots$ -way on the circle of nodes. 

Suppose a node $n$ needs to do a lookup for a node $n'$. The method is to find the finger that is closest to $n'$ without overshooting. This is known as a skip list, and its runtime is O($\log(n)$). #clarify variable notation is very inconsistent :\. The proof basically is that the distance is cut in half at each hop, and on average, half the nodes will get skipped. In practice, the expected hops turns out to be $\frac{1}{2}\log(n)$ due to randomization.

## Churn
Churn refers to the action of nodes entering and leaving the system. When a node $n$ joins the network, it contacts some node x (which handles all join interactions) and asks it for the successor to n, and it will set its successor to this node.

Chord also includes a stabilization protocol which maintains the shape of the ring. This runs at a fixed interval across all nodes. The pseudocode is as follows:
```
stabilize(n):
	x = successor.predecessor # This is the node that precedes its successor.
	if x in range(n, successor):
		successor = x
	successor.notify()
```
Let's think about what happens after a node joins. This node knows what it's successor should be, but its successor does not know at this point to update it's predecessor. This function ensures that the links are set correctly, and the notify() method instructs the successor to update its predecessor correctly.

This scheme turns out to be much simpler to prove correctness than other churn methods of Distributed Hash Tables. However, there are cases where this stabilization method cannot reform the ring.